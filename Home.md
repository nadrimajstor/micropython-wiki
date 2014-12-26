Welcome to the micropython dev wiki!
This is the [Micro Python](http://micropython.org/) project, which puts an implementation of Python 3.x on a microcontroller or embedded system. The project also includes a small [microcontroller board](https://github.com/micropython/pyboard) based around the STM32F405RG chip.

This wiki is to support development of the Micro Python core code and the various ports to new hardware.
The users guide on how to use Micro Python, and all user oriented library and module information, is on the [main Micro Python site](http://micropython.org/).

![micropython-logo](https://avatars1.githubusercontent.com/u/6298560?s=140 '"micro" the project mascot')

###What is Micro Python
Micro Python is Python interpreter (with partial native code emission feature). It provides subset of Python 3.4 features, implemented for embedded processors and constrained systems.

###Boards
The Kickstarter board
* The board relies on a 32 bit ARM Cortex M4 CPU (STM32F405RG, DSP with FPU, 1Mbyte Flash, 128+64 Kbyte RAM, 168 MHz).
* Technical data on the chip can be found here: [STMicroelectronics website](http://www.st.com/web/catalog/mmc/FM141/SC1169/SS1577/LN1035/PF252144) and the datasheet can be found here: [datasheet](http://www.st.com/st-web-ui/static/active/en/resource/technical/document/datasheet/DM00037051.pdf)

A list of other boards and their ports is here: **[[Other Boards|Boards-Summary]]**

### I want to try using micropython now!
If you have a machine that runs some flavor of Unix, see the [[Getting-Started]] instructions on how to obtain the binary to run micropython and start developing.

### Performance
Some [[numbers|Performance]] from a very simple benchmark

### Documentation

* For introduction and tutorials on using MicroPython on Pyboard follow links on http://micropython.org/
* MicroPython is an implementation of (subset of) Python language, so "Tutorial" and "Language Reference" sections of https://docs.python.org/3.4/ apply. (Note: only few, core library modules are provided with MicroPython, few more may be available as user-installable modules, so "Library Reference" applies only partially).
* Ongoing effort to document [[Differences]] between CPython and MicroPython.

#### The pyb module
This module allows access to the internal peripherals of the microcontroller chip. Initially, the 405RG chip noted above will be supported. Support for more microcontrollers may be added in future releases.
Some developer related information can be found here: [[pyb module|pyb module]]. But the user centric information is on the [main site](http://micropython.org/).

### Development
* [[ContributorGuidelines]] - get to know MicroPython approach.
* [[DevelWorkflow]] - suggested development workflow for contributors.
* [[Internals]] - some notes on VM/interpreter internals.
* [[Servos|Servos]] - how should we implement Servos
* [[PWM and Timers|PWM-Timers]]
* [[Watchdogs|Watchdogs,-Crash-prevention,-Crash-recovery]]
* [[How to port micro python to a new hardware target|porting]]

### Tips and Tricks / Howtos
[How to perform a soft reset](https://github.com/micropython/micropython/wiki/Soft-reset)

### Current Limitations
* The entire set of standard python libraries is **not** supported. If a module is missing it will be due to the inapplicability of that module for use in an embedded controller. High memory consumption (e.g. sqlite3) or a lack of a certain required hardware feature (e.g. multiprocessing) are reasons that some modules can not be implemented for some microcontrollers. 
The full list list of standard python libraries can be found here: [Python 3.4 standard lib](http://docs.python.org/3/library/).
* There are differences between CPython3 (considered to be a reference implementation of the Python3 language) and MicroPython. Documented [[here|Differences]].
