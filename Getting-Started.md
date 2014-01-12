Getting started with micropython development requires first building the appropriate binaries for your platform.

## UNIX
* [DEB-based systems](https://github.com/micropython/micropython/wiki/Getting-Started#debian-ubuntu-mint-and-variants)
* [FreeBSD-based systems](https://github.com/micropython/micropython/wiki/Getting-Started#freebsd)
* [RPM-based systems](https://github.com/micropython/micropython/wiki/Getting-Started#fedora-centos-and-red-hat-enterprise-linux-and-variants)
* [Pacman-based systems](https://github.com/micropython/micropython/wiki/Getting-Started#archlinux)
* [Gentoo-based systems](https://github.com/micropython/micropython/wiki/Getting-Started#gentoo-linux)
* [Mac systems](https://github.com/micropython/micropython/wiki/Getting-Started#mac-osx)


### Debian, Ubuntu, Mint, and variants

The following packages will need to be installed before you can compile and run MicroPython:

* build-essential
* libreadline-dev
* git

To install these packages, use the following command:

> sudo apt-get install build-essential libreadline-dev git

Then, clone the repository to your local machine:

> git clone https://github.com/micropython/micropython.git

Change directory to the Unix build directory:

> cd ./micropython/unix

And then make the executable

> make

At that point, you will have a functioning micropython executable, which may be launched with the command:

> ./py

### FreeBSD
 
(Release 9.2 tested)

Ensure that you have git, GCC, gmake, pythyon3, and bash packages installed:

> [as root] pkg_add -r git gcc gmake python3 bash

Clone the git repository to your local machine:

> git clone https://github.com/micropython/micropython.git

Change directory to the Unix build directory:

> cd ./micropython/unix

Edit main.c, replacing "malloc.h" with "stdlib.h", then:

> gmake

This will generate the 'py' executable, which may be executed by:

> ./py

### Fedora, CentOS, and Red Hat Enterprise Linux and variants

The required packages can be installed with:

> sudo yum install git gcc readline-devel

Clone the git repository to your local machine:

> git clone https://github.com/micropython/micropython.git

Change directory to the Unix build directory:

> cd ./micropython/unix

And then make the executable

> make

At that point, you will have a functioning micropython executable, which may be launched with the command:

> ./py

### ArchLinux

The following packages will need to be installed before you can compile and run MicroPython:

* gcc or gcc-multilib
* readline
* git

To install these packages, use the following command:

> pacman -S gcc readline git

Then, clone the repository to your local machine:

> git clone https://github.com/micropython/micropython.git

Change directory to the Unix build directory:

> cd micropython/unix

And then make the executable

> make

At that point, you will have a functioning micropython executable, which may be launched with the command:

> ./py

### Gentoo Linux

### Mac OSX

OSX is only supported on the osx branch. First do :

git checkout osx 

## Microcontrollers (Bare-Metal, without an OS)
### ARM-based microcontrollers
