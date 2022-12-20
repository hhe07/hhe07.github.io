---
layout: post
author: hhe07
category: hw
completed: in progress
---

### 19 Dec 2022

On the Tuesday night between my two finals, I was bored, so I started looking up Arduinos and associated hardware to fall asleep. I was thinking of a previous project idea that I had, which was to build an e-Ink MP3 player using an Arduino or Raspberry Pi, so the [Arduino MKR Zero](https://store-usa.arduino.cc/products/arduino-mkr-zero-i2s-bus-sd-for-sound-music-digital-audio-data?selectedStore=us) caught my eye, as it claimed to have the I2S bus for sound device communication. Another thing that was interesting to me was the SAMD21 microcontroller that the MKR Zero was built around, since I remembered that my father had a SAMD21 evaluation kit at home somewhere. While it doesn't provide nearly the convenience of the Arduino ecosystem, I did manage to get it to blink a LED, and to get the software installed.

I followed [this](https://omzlo.com/articles/programming-the-samd21-using-atmel-ice-with-openocd-(updated)) wonderful guide on how to get the SAMD21 working with Linux, but did make some adaptations for Manjaro / Arch. The other steps are basically the same.

1. Install the necessary tools:
   1. ``arm-none-eabi-gcc``, ``arm-none-eabi-gdb``: used for compiling and debugging code on microcontroller
   2. ``openocd``: used to interface with the microcontroller
   3. ``avr-gcc`` and ``avr-gdb``: probably not necessary, but they may include useful headers
   4. ``avrdude``: probably not necessary, but may be useful
2. Create a directory for the project
3. Create ``openocd.cfg`` in the directory:
   1. I found the SAMD21 Xplained's config [here](https://github.com/arduino/OpenOCD/blob/master/tcl/board/atmel_samd21_xplained_pro.cfg)
5. For the SAMD21 Xplained specifically, no additional power source is necessary, just the microUSB cable plugged into DEBUG USB
6. Also, the integrated LED is plugged into PB30, instead of PA17 on the board that the tutorial used, so change ``REG_PORT_DIR0`` to ``REG_PORT_DIR1``, and do similar for ``REG_PORT_OUT0``. This changes the output register from the one controlling the PAXX I/O pins to the PBXX I/O pins. Then, change the left shift afterwards to ``1<<30`` rather than ``1<<17``, which selects the 30th pin rather than the 17th.
7.  The article mistakenly encourages downloading the Atmel Software Framework rather than the Advanced Software Framework. Download the Advanced Software Framework zip [here](https://www.microchip.com/en-us/tools-resources/develop/libraries/advanced-software-framework), then extract. Otherwise, follow the spirit of the instructions --- copy the correct startup and linker files. The use of a new environment variable is somewhat overkill imo but w/e
8.  The Makefile is correct, just change the processor name.

### next steps
- dig around in Advanced Software Framework for more useable header files.
- look around in the [Atmel Software Package](https://github.com/atmelcorp/atmel-software-package) for more of the above, as well as examples that can be adapted
- try writing more complex programs
- honestly, probably not the best choice for a MP3 player at my current skill level, but *basic familiarity* with the embedded programming workflow will be helpful.