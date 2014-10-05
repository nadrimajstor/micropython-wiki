Cortex M4
* Olimex STM32-E407 https://www.olimex.com/Products/ARM/ST/STM32-E407/open-source-hardware

The E407 is like the [Olimex H407](https://github.com/micropython/micropython/wiki/Board-Olimex-STM32-H407), but with Ethernet PHY chip and jack.

###Features###
* open source hardware
* STM32F407ZGT6 Cortex-M4 210DMIPS
* 1024KB FLASH, 196kB RAM
* 3x ADC 12 bit 2.4 MSPS
* 2x DAC 12bit
* 14 Timers
* USB Host and OTG, 2 CAN, 3 I2C, 3 UART, 3 SPI, Ethernet
* 114 GPIO pins
* Camera interface
* SD card
* User buttons
* Arduino platform with unsoldered headers

###Status###
Preliminary support is in https://github.com/markushx/micropython/tree/olimex-e407. Untested.

###Jumpers###

For programming the board, the jumpers on the board need to be set as in the following picture:
![](https://github.com/markushx/micropython/blob/olimex-e407/stmhal/boards/OLIMEX_E407/doc/Olimex-Pyb-Programming.jpg)

###Build instructions###
Follow https://github.com/micropython/micropython/wiki/Getting-Started-STM.

    cd stmhal
    BOARD=OLIMEX_E407 make
    dfu-util -d 0483:df11 -a 0 -D build-OLIMEX_E407/firmware.dfu

###Running micropython on the board###
Change the top right jumper from the left to the right position and power up the board.