(First draft)
As of this writing (3 October 2014), both the unix and stmhal (pyboard) ports of Micro Python can be compiled on Mac OSX using the master branch without any alteration. This article aims to describe which software packages are required to build Micro Python and how to install them on your OSX system. I use [Macports](www.macports.org) so this article will focus on that; it is also possible to Homebrew to install the required software dependencies.

# Compiling the unix port on OSX

Recent modifications to the build scripts have made the build process for the unix port on OSX the same as it is that on Linux, despite the fact that Micro Python is built using gcc on Linux and clang on OSX.
In order to compile on OSX you will need the following software packages installed:
* A relatively recent version of Xcode to provide the clang compiler (micropython has been confirmed to compile under clang versions 3.1 and 4.2)
* libffi (minimum version 3.1-4 from Macports)
* pkgconfig
* Python >= 3.3
* git - to keep local source tree up to date
* (TODO: others?)


# Compiling the stmhal port for the pyboard

Required software:
* mac version of gcc-arm-none-eabi, version >= 4.8
* dfu-util >= 0.7 to load firmware to pyboard