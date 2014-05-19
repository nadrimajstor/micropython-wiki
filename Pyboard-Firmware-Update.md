## Download

You can download the latest firmware from http://micropython.org/download/.

## Preparation

First, disconnect everything from your pyboard. Then connect the DFU pin with the 3.3V pin (they're right next to each other) to put the board into DFU (*Device Firmware Update*) mode. Now connect the pyboard to your computer via USB.

## Flashing

# dfu-util

First, install dfu-util via your package manager.

You can list all DFU-capable devices using the `-l` argument:

```
$ sudo dfu-util -l
dfu-util 0.5

(C) 2005-2008 by Weston Schmidt, Harald Welte and OpenMoko Inc.
(C) 2010-2011 Tormod Volden (DfuSe support)
This program is Free Software and has ABSOLUTELY NO WARRANTY

dfu-util does currently only support DFU version 1.0

Found DFU: [0483:df11] devnum=0, cfg=1, intf=0, alt=0, name="@Internal Flash  /0x08000000/04*016Kg,01*064Kg,07*128Kg"
Found DFU: [0483:df11] devnum=0, cfg=1, intf=0, alt=1, name="@Option Bytes  /0x1FFFC000/01*016 e"
Found DFU: [0483:df11] devnum=0, cfg=1, intf=0, alt=2, name="@OTP Memory /0x1FFF7800/01*512 e,01*016 e"
Found DFU: [0483:df11] devnum=0, cfg=1, intf=0, alt=3, name="@Device Feature/0xFFFF0000/01*004 e"
```

First, backup the original firmware:

```
$ sudo dfu-util --alt 0 -U pyboard-original.dfu
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
Limiting default upload to 16384 bytes
bytes_per_hash=2048
Starting upload: [#######] finished!
```

Then write the downloaded firmware to the pyboard:

```
$ sudo dfu-util --alt 0 -D pybv10-2014-05-19-v1.0.1-24-g5cdff5f.dfu 
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
Determining device status: state = dfuUPLOAD-IDLE, status = 0
aborting previous incomplete transfer
Determining device status: state = dfuIDLE, status = 0
dfuIDLE, continuing
DFU mode device DFU version 011a
Device returned transfer size 2048
Dfu suffix version 11a
DfuSe interface name: "Internal Flash  "
file contains 1 DFU images
parsing DFU image 1
image for alternate setting 0, (2 elements, total size = 245928)
parsing element 1, address = 0x08000000, size = 392
parsing element 2, address = 0x08020000, size = 245520
done parsing DfuSe file
```

# dfu-programmer

Some other DFU programmer. TODO: Document.

# On Windows

TODO: Document