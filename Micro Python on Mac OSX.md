# Introduction
This article has been renamed from [OSX Tips and Tricks](https://github.com/micropython/micropython/wiki/OSX-Tips-and-Tricks) because Micro Python now (as of early October 2014) appears to be supported on OSX as well as it is on Linux: once the necessary software dependencies are installed on an OSX system, development and usage is essentially the same on both platforms, so no special 'tricks' are required to work with Micro Python on OSX.  (If any discrepancies are found, they should be reported here or on the [user forum](http://forum.micropython.org).) 

Specifically, this means that both the unix and stmhal (pyboard) ports of Micro Python can be compiled on Mac OSX directly from the master branch without any alteration. This article aims to describe which software packages are required to build Micro Python and how to install them on your OSX system. I use [Macports](http://www.macports.org) so this article will focus on that; it is also possible to Homebrew to install the required software dependencies.

This article is targeted towards Micro Python developers using OSX, it will address OSX specific issues rather than general development questions.

# Compiling the unix port on OSX

Recent modifications to the build scripts have made the build process for the unix port on OSX the same as it is that on Linux, despite the fact that Micro Python is built using gcc on Linux and clang on OSX.
In order to compile on OSX you will need the following software packages installed:
* A relatively recent version of Xcode to provide the clang compiler (micropython has been confirmed to compile under clang versions 3.1 and 4.2)
* [Macports](http://www.macports.org) for the next commands
* libffi (minimum version 3.1-4 from Macports) `sudo port install libffi`
* pkgconfig `sudo port install pkgconfig`
* Python >= 3.3 `sudo port install python34`
* git - to keep local source tree up to date

## Build Process
Same as on Linux - go to `micropython/unix` and run `make` (or `make -B` or `make clean; make` if rebuilding.

## Running tests
Go to `micropython/tests` and run the test script `./run-tests`
The test script will look for the executable in `../unix/micropython` and will compare the output of the micropython executable to the locally installed version of Python3
* If your Python3 executable is named `python3.x` then it may be necessary to create a link named `python3` to this executable
* All tests should pass on OSX, however it may be necessary to set the encoding in the terminal to 'UTF-8' if the unicode tests are failing

# Compiling the stmhal port for the pyboard

Required software:
* mac version of gcc-arm-none-eabi, version >= 4.8
* dfu-util >= 0.7 to load firmware to pyboard
* Python3 >= 3.3
* pyserial package for Python3

TODO: building stmhal

TODO: Using test scripts to run tests on pyboard
```
cd micropython/tests
./run-tests --pyboard --device /dev/tty.usbmodem*
```

Using pyboard.py to run python scripts residing on the mac:
```
python3 tools/pyboard.py --device /dev/tty.usbmodem* FILE
```

### Using `screen` to access REPL
Type `screen /dev/tty.usbmodem*` with `*` replaced by your specific device number; use `ls /dev/tty.usbmodem*` to find the name of your device.
To exit from 'screen' type 'Ctl-a, k'; and then answer 'y' to exit

Development workflow on mac is the same as for Linux; see [DevelWorkflow](DevelWorkflow) for recommended workflow practices and usage of git.

TODO: Issues that may be encountered using previous versions of pyboard firmware (USB mass storage issue) that can be fixed with firmware upgrade