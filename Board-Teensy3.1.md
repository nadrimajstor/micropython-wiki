Cortex M4 based MK20DX256VLH7
* [Teensy](http://www.pjrc.com/teensy/teensy31.html)

###Features###
* MK20DX256VLH7 72MHz
* 256K Flash, 64kB RAM
* 34 pins available for digitial  I/O
* 21 pins availale for Analog 2x 16bit A/D
* 12 timers, 12 PWM
* SPI, i2c, CANbus

###Status###
1. Work has started on porting to this chip. See [repo](https://github.com/micropython/micropython/tree/master/teensy) Aug 3, 2014 - https://github.com/micropython/micropython/pull/786 compiles and builds for teensy.

###Building###
* You shouldn't need to install teensyduino to build (you will need teensyduino to flash the image)
* You will need to install realpath (this is currently used by add-memzip.sh)
* The deploy target (which flashes the firmware onto the Teensy 3.1) needs to use the bootloader from teensyduino. The Makefile needs the ARDUINO environment variable setup to point to the root of you teensyduino (aka arduino) installation tree. More specifically, the bootloader is found using $(ARDUINO)/hardware/tools/teensy_post_compile is the name of the bootloader.

NOTE: One convenient way of setting the ARDUINO environment variable is to create a GNUmakefile (note that GNUmakefile is spelled with a lowercase m) which looks like this:
```make
$(info Running GNUmakefile)

ARDUINO = /home/dhylands/arduino-1.0.5

include Makefile
```
then when you use GNU make, it will automatically use GNUmakefile instead of Makefile, which allows you to put in user-specific customizations.
###Running Scripts###
Due to memory limitations (the teensy has 256K flash, and micropython currently consumes about 201K) the USB Mass Storage feature of the stmhal build couldn't be supported.

So I introduced something called memzip. This is a zip file which contains uncompressed files. The build system takes all of the files located in $(MEMZIP_DIR) (which you can specify in GNUmakefile as mentioned above for the ARDUINO variable) and this directory tree becomes your filesystem for MicroPython. The sample memzip_files includes a boot.py and main.py. The default main.py flashes the LED twice.

If you want to update a script, you'll need to rebuild and reflash the firmware.

###What's currently supported###

* pyb.delay()
* pyb.disable_irq()
* pyb.enable_irq()
* pyb.freq()
* pyb.have_cdc()
* pyb.info()
* pyb.udelay()
* pyb.unique_id()
* pyb.LED
* pyb.Pin
* pyb.UART