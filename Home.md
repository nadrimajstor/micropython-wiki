Welcome to the micropython dev wiki!

## Introduction
This is the Micro Python project, which puts an implementation of Python 3.x on a microcontroller. The project also includes a small microcontroller board based around the STM32F405RG.

This wiki is to support development of Micro Python and the various ports to new hardware.

###What is Micro Python
Based on python 3.x but implemented for embedded processors.

### The Kickstarter board
The board relies on a 32 bit ARM Cortex M4 CPU (STM32F405RG, DSP with FPU,1 Mbyte Flash, 168 MHz). Technical data on the chip can be found here: [STMicroelectronics website](http://www.st.com/web/catalog/mmc/FM141/SC1169/SS1577/LN1035/PF252144) and the datasheet can be found here: [datasheet](http://www.st.com/st-web-ui/static/active/en/resource/technical/document/datasheet/DM00037051.pdf)

###Other hardware targets
Future releases of micropython will support other microcontrollers and microcontroller families as well as dedicated third-party boards.
* How to port micro python to a new hardware target
* Implementation details:
    - [[i2c|i2c_info]]

###The [[pyb module|pyb module]]
This module allows access to the internal peripherals of the microcontroller chip. Initially, the 405RG chip noted above will be supported. Support for different microcontrollers are will be added in future releases.
More information on the exposed functionality can be found here: [[pyb module|pyb module]]

### Current Limitations
The entire set of standard python libraries is **not** supported. If a module is missing it will be due to the inapplicability of that module for use in an embedded controller. High memory consumption (e.g. sqlite3) or a lack of a certain required hardware feature (e.g. multiprocessing) are reasons that some modules can not be implemented for some microcontrollers. 
The full list list of standard python libraries can be found here: [Python 3.3 standard lib](http://docs.python.org/3/library/) 

Modules which will (at the moment) **NOT** be covered in micropython are:

* dbm
* sqlite3
* threading
* multiprocessing