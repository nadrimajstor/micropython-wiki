# Getting Started with MicroPython

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