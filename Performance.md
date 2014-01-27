Here's a sample little tight counter loop, running for 10 seconds on 3 different setups:

**MicroPython on Teensy 3.1: (96Mhz ARM)**

    endTime = pyb.millis() + 10000  
    count = 0  
    while pyb.millis() < endTime:  
        count += 1  
    print("Count: ", count)  

**Count:  396,505**

***

**C++ on Teensy 3.1: (96Mhz ARM)**

    #include <Arduino.h>  
    void setup() {  
        Serial1.begin(115200);  
        uint32_t endTime = millis() + 10000;  
        uint32_t count = 0;  
        while (millis() < endTime)  
            count++;  
        Serial1.print("Count: ");  
        Serial1.println(count);  
    }  
    void loop() {  
    }  

**Count: 95,835,923**

***

**C++ on Arduino Pro Mini: (16 MHz Atmega328)**

    #include <Arduino.h>  
    void setup() {  
        Serial.begin(115200);  
        uint32_t endTime = millis() + 10000;  
        uint32_t count = 0;  
        while (millis() < endTime)  
            count++;  
        Serial.print("Count: ");  
        Serial.println(count);  
    }  
    void loop() {  
    }  

**Count: 4,970,227**
