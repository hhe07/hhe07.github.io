---
layout: post
author: hhe07
category: cs
completed: 2022-2-1
---

I was an IB diploma student in secondary school. One of the required components is the [Extended Essay](https://www.ibo.org/programmes/diploma-programme/curriculum/extended-essay/). Quoting from the IB website,

> The extended essay is an independent, self-directed piece of research, finishing with a 4,000-word paper.

The essay must have a *title* (topic), and a *category* (from a selection of fields determined by the IBO).

My paper was in the Computer Science category, and investigated two different data structures for storing text in text editors. The actual research results and methodology is more thoroughly discussed in the paper itself, which will be linked below, so this post will provide contextual information, discuss the process of completing the project, and what I learned from the process.

### buffers, the gap buffer, and the rope.
Briefly, text is stored in text editors using a type of data structure called a "buffer", which benefits performance and simplifies implementation of features when compared to directly manipulating a block of text in memory. As an example, take the case of adding a few characters:

Let's say we're storing the following 6 characters in memory:
```
abcdef
```

If we want to add characters after a position in the middle of the buffer, say, index 2: (let ``_`` represent the cursor position)
```
abc_def
```

we would need to reallocate the entire buffer for the desired number of characters, and shift / copy part of the buffer to the newly allocated space. This becomes problematic for larger text files of hundreds or thousands of characters, so storing the text in a structure that isn't an array is preferred.

The two data structures I investigated use two different approaches to "break up" the text being stored. In doing so, they both have their own unique advantages and disadvantages. 

The *gap buffer* splits the text up into three components --- the part before the current cursor position, the current cursor position and some pre-allocated space for text insertion, and the part after the current cursor position. By turning the cursor position (the "gap") into a separate object, the problems caused by adding to the centre of an array are mitigated. My implementation treated the gap buffer as a doubly-linked list, with pointers to the start and end of the gap. I thought this was a logical next-step from the gap buffer's concept, because it further atomised the text segments to allow for cleaner addition / removal operations. The disadvantages of this data structure are the complexity of moving the gap (which happens whenever the user moves to a different line), and (in my implementation) the inherent time complexity of traversing through a doubly-linked-list.

The *rope* splits the text up into a binary search tree. Each node is given a "weight" parameter, which is the sum of the weights of its left sub-tree. Only leaves actually store text, in the form of either an array or a string, thus "breaking up" the text. The advantages of this data structure result from the fact that it's a binary search tree, so concatenation, insertion, and deletion are all O(log n) time complexity worst-case. Additionally, while $O(log n)$ time complexity is worse than O(1) indexing time for an array, it's theoretically better when  compared to the O(n) time complexity for indexing in a doubly-linked list. The disadvantages of this data structure are its implementation complexity and memory usage.

### investigation content

Personally, I wanted to learn about and implement these data structures, and gain experience with Golang. While I had used Go before, and I had worked with data structures before, I hadn't done something nearly as complex as what my research required, nor had I worked with the more complex data structures that I was investigating.

In terms of the research goal, I was to test the (properly implemented) data structures for adherance to their theoretical time complexities (Big O notation), and their performance compared to one another. I planned to do this using the benchmarking and unit testing tools built into Go. This was also something new for me, since I hadn't done a "scientific" investigation of programs I had written before. I had done a science fair project in computer science during my sophomore year, but there wasn't much statistical analysis or detailed investigation of the program's *operation* like this project involved.

### implementation process

Implementation was somewhat difficult, but in a manageable way. 

For the gap buffer, the Wikipedia article's notes were (and still are) quite vague, so I relied upon the linked segment from the [Emacs manual](https://www.gnu.org/software/emacs/manual/html_node/elisp/Buffer-Gap.html) (since Emacs uses gap buffers), and an [article by Mark Chu-Carroll](https://scienceblogs.com/goodmath/2009/02/18/gap-buffers-or-why-bother-with-1) about his implementation of a gap buffer in Scala. My implementation differs from the latter article in that it uses a linked list, whereas the Scala implementation abstracts over an array instead. Other than some difficulties figuring out how to make pointers to the start and end of the gap behave properly, I honestly didn't have too much trouble with the gap buffer, since I had worked with linked lists before, and it was just a more specialised linked list.

I had a few false starts on the rope. First, I briefly searched for implementations by other people on Github but didn't find anything that fully satisfied my requirements for a "well-implemented" data structure, even after I modified them a little bit, so I ultimately decided to make my own. Then, since the Wikipedia article on the topic was rather vague, I encountered unforseen issues during the implementation process, such as the assumption that the rope would be balanced without indicating a specific mechanism for this to take place, or the implicit need for tree nodes to reference their parent for more efficient deletion operations. Otherwise, since it was my first time writing anything to do with a binary search tree, the task was honestly somewhat daunting.

For the rope, I largely followed the Wikipedia article's description of the features, with some references to a paper that had been written about ropes by [Boehm et. al ](https://web.archive.org/web/20200308005351/https://citeseer.ist.psu.edu/viewdoc/download?doi=10.1.1.14.9450&rep=rep1&type=pdf) for some parts that weren't clear in the article. The article now has been updated with sample code (not by me), which I didn't have access to at the time, so I had to rely on the textual description of feature implementation and figure out some specifics on my own. For example, the length of text segments chosen during the rope initialization was a mystery to me, so I chose to split the text up evenly into 10 character segments, with the last segment being underfilled if the text length wasn't a multiple of 10. Otherwise, since the article previously didn't give any suggestion about how to preserve tree balance, and now recommends completely rebuilding unbalanced tree segments, I instead opted for an AVL-type tree to form the basis for my rope, which still preserves the core functionality of the rope while preserving balance. The idea to use this type of tree was my own, but I did still follow Wikipedia's article on the AVL tree for specific implementation details.

The testing suite (first unit tests and then benchmarks) also presented some difficulties. I had worked with Go's unit tests before, so those went fairly smoothly. In anticipation of these two sets of tests, I had made both buffers conform to a common *interface*, which allowed me to make generic tests that worked for both data structures. I did have to fix some errors that were made in the implementation of the two data structures, but generally the implementations of both were relatively successful. The benchmarks proved to be problematic, however, in that many benchmarking runs produced wildly varying or nonexistent time results. It took several iterations and attempts at different approaches to produce something that was at least more consistent, but there were definitely some gaps in the data produced as a result. Generally, though, benchmarking wasn't too difficult to figure out, although I wish I had increased the level of automation for transferring data to a table and then graphing it with Python and matplotlib. 


### experience notes
One of the parts of this project that I'm most proud of, other than the actual results, is my general involvement in the process of implementation, from start to end. From the outset, I think I chose a reasonable and well-researched title to undertake. Previously, I was prone to flights of fancy with programming projects, since I usually overestimated what I was capable of and underestimated the difficulty of the task. During the implementation process, I learned that I really enjoyed the process of programming for research, and was able to focus on the topic even if it became challenging at times. Even though I had to pull very long days / nights over winter break to get data and analyze it, it felt productive and meaningful throughout. I think this project has helped me create my own meaning out of programming, turning it from something my parents suggested I grow some skills in to something that I could really be passionate about. After the implementation, I also enjoyed the technical writing component and the discussion of results. I was fortunate to have learned about technical writing fairly early in school, so naturally, after some practice, it became something that I was quite good at. I enjoyed it especially in this instance because it provided an opportunity for me to present what I had done and learned in *my* way, and properly reflected the work that I had put into the project.

### endnotes
I think that the Extended Essay process was really beneficial for me --- it revitalised my interest in programming, proved that I could handle advanced and difficult work, that I could handle the time commitment, and that I could produce notable results if I put my heart into the task. The joy of doing this research and the genuine knowledge that I gained serve as a template for any sort of project that I do now. I hope to learn a similar amount, commit myself to a similar degree, and ultimately enjoy myself to a similar extent.

You can read the full paper [here](/assets/pdf/main.pdf)