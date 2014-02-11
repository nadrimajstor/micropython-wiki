Cortex M4 STM32F407VG 
* Home - http://www.st.com/web/catalog/tools/FM116/SC959/SS1532/LN1199/PF252419
* [Datasheet](http://www.st.com/st-web-ui/static/active/en/resource/technical/document/data_brief/DM00037955.pdf)

###Features###
* STM32F407VG 168MHz (100pin)
* 1024KB flash ROM, 192KB RAM
* 3axis accelerometer LIS302DL
* mems microphopne MP45DT02, audio amp CS43L22
* 4 user LEDs
* 1 user switch
* Reset switch
* USB OTG

###Status###
* Initial support has been implemented, user switch and LEDs are supported.
* Does not have separate directory - folded into initial stm port.
* To build for STM32F407 Discovery compile with `make TARGET=STM32F4DISC`.
* Also set `#define STM32F4DISC` in mpconfigport.h

###Programming from Linux (via DFU)###
This section covers how to flash the STM32F4DISCOVERY from linux (I was using Linux Mint 14, which is derived from ubuntu 12.10).

* Install dfu-utils
```
sudo apt-get install dfu-util
```

* Create a udev rules file. I used ```sudo vi /etc/udev/49-stmdiscovery.rules``` and put the following contents:
```
# 0483:5740 - STM32F4 Dsicovery in USB Serial Mode (CN5)
ATTRS{idVendor}=="0483", ATTRS{idProduct}=="5740", ENV{ID_MM_DEVICE_IGNORE}="1"
ATTRS{idVendor}=="0483", ATTRS{idProduct}=="5740", ENV{MTP_NO_PROBE}="1"
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="5740", MODE:="0666"
KERNEL=="ttyACM*", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="5740", MODE:="0666"
# 0483:df11 - STM32F4 Discovery in DFU mode (CN5)
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="df11", MODE:="0666"
```
Tell udev to reload its rules:
```
sudo udevadm control --reload-rules
```

* Plug in the discovery board. To use DFU, you need to connect CN5 (the micro-USB connector on the bottom of the board) into your PC. Unfortunately, the CN5 connector doesn't power the board, so you can either connect CN1 (mini-USB connector at the top of the board), or run a jumper wire from PA9 to 5V (PA9 connects to VBUS on CN5) in order to power the board. CN1 connects to the STM32F103 chip near the mini connector in order to support stlink. CN5 connects to the STM32F407 chip.

* Put the board in DFU mode. To do this, you need to make BOOT0 high, and BOOT1 low, and reset the board. Since the onboard bootloader supports bootloading over UARTs, CAN bus, and USB, you need to make sure that UART1 (PA9/PA10) UART3 (PB10/11 and PC10/11) and CAN (PB5/13) are quiescent while bootloading.

On the discovery board, BOOT1 is pulled low through SB19 and a 510 ohm resistor, and BOOT0 is pulled low through SB18 and another 510 ohm resistor. Since BOOT1 is already low, you just need to make BOOT0 high. Conveniently the BOOT0 signal is beside a VDD signal on the P2 header. So you can install a shorting jumper (there are a couple of spares included with the board on JP2 and JP3 on the bottomside of the board) between BOOT0 and VDD and press the reset button.

You should now be able to program the board using:
```
dfu-util -a 0 -D build/flash.dfu
```
and you should get output similar to this:
```
2196 >dfu-util -a 0 -D build/flash.dfu
dfu-util 0.5

(C) 2005-2008 by Weston Schmidt, Harald Welte and OpenMoko Inc.
(C) 2010-2011 Tormod Volden (DfuSe support)
This program is Free Software and has ABSOLUTELY NO WARRANTY

dfu-util does currently only support DFU version 1.0

Opening DFU USB device... ID 0483:df11
Run-time device DFU version 011a
Found DFU: [0483:df11] devnum=0, cfg=1, intf=0, alt=0, name="@Internal Flash  /0x08000000/04*016Kg,01*064Kg,07*128Kg"
Claiming USB DFU Interface...
Setting Alternate Setting #0 ...
Determining device status: state = dfuERROR, status = 10
dfuERROR, clearing status
Determining device status: state = dfuIDLE, status = 0
dfuIDLE, continuing
DFU mode device DFU version 011a
Device returned transfer size 2048
Dfu suffix version 11a
DfuSe interface name: "Internal Flash  "
file contains 1 DFU images
parsing DFU image 1
image for alternate setting 0, (2 elements, total size = 136908)
parsing element 1, address = 0x08000000, size = 392
parsing element 2, address = 0x08020000, size = 136500
done parsing DfuSe file
```

Remove the BOOT0 jumper and press reset. You should now be able to fire up a terminal emulator (i.e. minicom) on ```/dev/ttyACM0``` and have a micropython prompt.

```
minicom -D /dev/ttyACM0
```
where you should see something like this:

```
Welcome to minicom 2.7

OPTIONS: I18n 
Compiled on Jan 11 2014, 10:01:22.
Port /dev/ttyACM0, 23:34:32

Press CTRL-A Z for help on special keys

>>>
```

If you hit Ctl+D the MCU will do a soft reboot and it will run whatever code is in /src/main.py and the following will be written to the console:

```
PYB: sync filesystems
PYB: soft reboot
```

If you hit Ctl+C the running of /src/main.py is interrupted and you will be returned to the python prompt. The following is printed to the console:

```
Traceback (most recent call last):
  File "0://src/main.py", in <module>
VCPInterrupt()
Micro Python build <git hash> on 25/1/2014; F4DISC with STM32F405RG
Type "help()" for more information.
>>>
```
###Programming from Mac OS X (via DFU)###

If you are building on Mac OS X, you could programming like from Linux except install dfu-utils throught [homebrew](http://brew.sh/):

    brew install dfu-util

###Dealing with multiple DFU devices###
When DFU looks for devices, if it only sees one device, then it will pick that device. It's been reported that sometimes other devices may show up, for example on one machine the error:
```
More than one DFU capable USB device found, you might try `--list' and then disconnect all but one device
```
was reported and ```dfu-util --list``` reported:
```
ound DFU: [0483:df11] devnum=0, cfg=1, intf=0, alt=0, name="UNDEFINED"
Found DFU: [0483:df11] devnum=0, cfg=1, intf=0, alt=1, name="UNDEFINED"
Found DFU: [0483:df11] devnum=0, cfg=1, intf=0, alt=2, name="UNDEFINED"
Found DFU: [0483:df11] devnum=0, cfg=1, intf=0, alt=3, name="UNDEFINED"
Found Runtime: [0a5c:21e6] devnum=0, cfg=1, intf=3, alt=0, name="UNDEFINED"
```
In this case, the device with the ID ```0a5c:21e6``` was some type of Broadcom device. In this case you can add ```--device 0483:df11``` to the dfu-util command line to tell it which device to use.