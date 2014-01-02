Getting started with micropython development requires first building the appropriate binaries for your platform.

## UNIX

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

Ensure that you have git and the GCC packages installed:

> [as root] pkg_add -r git gcc gmake

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

### Gentoo Linux