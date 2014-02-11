Cortex M4 STM32F429ZI
* on STM site - http://www.st.com/web/catalog/tools/FM116/SC959/SS1532/LN1199/PF259090
* [datasheet](http://www.st.com/st-web-ui/static/active/en/resource/technical/document/data_brief/DM00094498.pdf)

###Features###
* STM32F429ZI 168MHz (144pin)
* 2MB flashROM, 256KB RAM
* 64Mbits SDRAM
* 3axis gyro L3GD20
* 2.4 inch TFT display
* 6 LEDs
* USB OTG

###Status###
No work has yet been done on porting to this chip

Unlike the STM32F4 Discovery board, this board is not recognized under Linux via the microUSB port. To power the board one has to use the miniUSB connector in addition or bridge 5V with PB13. However, the schematics looks very much like the STM32F4 Discovery board and porting should be easily done after the above problem is solved.