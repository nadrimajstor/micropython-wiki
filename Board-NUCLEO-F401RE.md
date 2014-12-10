NUCLEO-F401RE
* [Home](http://www.st.com/web/catalog/tools/FM116/SC959/SS1532/LN1847/PF260000?icmp=nucleo-ipf_pron_pr-nucleo_feb2014&sc=nucleoF401RE-pr)
* [Datasheet](http://www.st.com/st-web-ui/static/active/en/resource/technical/document/data_brief/DM00105918.pdf)

###Features###
* STM32F401RE ARM 32 bit CORTEX M4™ 84MHz (64pin)
* 512KB FLASH, 96kB SRAM
* 1x ADC 12 bit
* 3 TIMERS - up to 72Mhz
* 2 I2C, 2 UART, 2 SPI
* 1 user LED
* 1 user switch
* Reset switch
* Arduino pin headers
* Additional Morpho headers
* mbed enabled (to program it just copy the file "firmware.bin" inside)

###Status###
* Initial support [here](https://github.com/nuraci/micropython)
* test in progress

### How build for ###
* Change directory to micropython/nucleo
* To build use one of the following methods:
   1. ```make BOARD=STM32NUCF401```
   1. ```export BOARD=STM32NUCF401``` in your environment and then just use make.
   1. ```make```


