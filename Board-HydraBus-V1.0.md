Cortex M4F
* HydraBus 1.0 http://hydrabus.com/hydrabus-1-0-specifications
* Hardware doc, schematic/board are available on github: https://github.com/bvernoux/hydrabus/tree/master/hardware, Hardware licence CC BY NC (for commercial licence contact info@hydrabus.com).
* HydraBus/HydraNFC... products are Available in Seeed Studio Online Shop: http://www.seeedstudio.com/depot/HydraBus-m-132.html

###Features###
* Standard Dangerous Prototypes PCB size DP6037_v1 (see http://dangerousprototypes.com/docs/Sick_of_Beige_basic_case_v1) (very small 60mm x 37mm size).
* Programming firmware through USB DFU (without any debugger) with USB1 FS.
* Debug/Programming through low cost SWD Debug connector (can be programmed/debugged using a low cost STM32F4 Discovery board for less than 20US$).
* Two MicroUSB port (1 OTG and 1 Device/Host) with ESD protection.
* MicroSD slot with 4bit SD and SDIO mode support in hardware (up to 48MHz about 24MB/s).
* Reset & User Button with User Led (can be disabled to reuse I/O for other stuff).
* Breakout of all 44 I/O (some are used by MicroSD and USB 1&2).
* Micro Python firmware ported to HydraBus see HydraBus Micro Python github: https://github.com/bvernoux/hydrabus/tree/master/firmware/micropython and available in official branch https://github.com/micropython/micropython

###Status###
Initial port completed with same feature as PyBoard except only 1 LED is available.

To build see https://github.com/bvernoux/hydrabus/tree/master/firmware/micropython.
