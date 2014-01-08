Cortex M4 STM32F407VG 
* Home - http://www.st.com/web/catalog/tools/FM116/SC959/SS1532/LN1199/PF252419
* [Datasheet](http://www.st.com/st-web-ui/static/active/en/resource/technical/document/data_brief/DM00037955.pdf)

###Features###
* STM32F407VG 168MHz (100pin)
* 1024KB flash ROM, 192KB RAM
* 3axis accelerometer LIS302DL
* mems microphopne MP45DT02, audio amp CS43L22
* 4 user LEDs
* 1 user switch
* Reset switch
* USB OTG

###Status###
* Initial support has been implemented, user switch and LEDs are supported.
* To build for STM32F407 Discovery compile with <code>make TARGET=STM32F4DISC</code>.