Welcome to the micropython wiki!

## Introduction
This is the Micro Python project, which puts an implementation of Python 3.x on a microcontroller. The project also includes a small microcontroller board based around the STM32F405RG.
###What is Micro Python

### The Kickstarter board
The STM32F405RG is a 32 bit ARM M4 CPU with some useful capabilities. Technical data on the chip is here: [STMS website](http://www.st.com/web/catalog/mmc/FM141/SC1169/SS1577/LN1035/PF252144) and the datasheet can be found here: [datasheet](http://www.st.com/st-web-ui/static/active/en/resource/technical/document/datasheet/DM00037051.pdf)
###Other hardware targets

###The [[pyb module|pyb module]]
Is the module that allows access to the internals of the chip. Initially designed for the 405RG chip noted above. There will be different targets for different chipsets.
More information on teh exposed functionality is here: [[pyb module|pyb module]]

