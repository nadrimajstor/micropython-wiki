## Task:
During development, one might want to restart a python program over and over again and keep the source code in an editor to perform quick changes. This is useful to test new features and to achieve a quick debug cycle.

## Problem:
Pressing the reset button will disconnect the board (confirmed on the STM Discovery boards) from the system and reattach it. This includes loosing connection to the mass storage device which keeps the main.py file for execution. Mount points on Linux systems will be not longer valid and you have remount (or use an automounter) and reenter the mounted folder.
This tends to confuse text editors including the loss of data.

## Solution:
To avoid this problem, one can perform a soft reset. This restarts python and the main.py file without physically disconnection of the mass storage device.
To perform a softreset, create a connection to the board via a serial console program e.g..

`screen /dev/ttyACM0`.
Since micropython executes the main.py file, there will be no prompt.

Pressing `Ctrl-D` will interrupt the execution of the main file and drop back into the interactive micropython shell. 

Pressing `Ctrl-C` will terminate the interactive shell and soft reset the controller to restart the main.py file.

### Further improvements
One can echo those commands into the serial connection of the board to achieve the same goal (make sure you have write permissions).
`echo "\x04\x05" > /dev/ttyASM0`

### Integrate into IDE/Editor
Several IDEs and Editors allow to customize shortkeys to execute arbitrary code.
 