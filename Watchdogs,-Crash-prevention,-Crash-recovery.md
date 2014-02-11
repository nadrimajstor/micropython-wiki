##  Introduction.

Software bugs, unforseen sequences of events and hardware events such us power glitches, transients from motors, ESD (electrostatic discharge) can cause an MCU to crash and stop running properly. Common failures are corruption of RAM and registers including the program counter. The program may jump to a different place and continue to run, but using corrupt parameters or it may get into and endless loop of doing nothing, or worse enabling or disabling an output it shouldn't. Watchdog timers and service routines (or Computer Operating Properly - COP in Freescale talk) can save the day by detecting these failures and resetting the MCU or system.

The purpose of this page is to (1) examine best practice in watchdog systems, (2) examine the hardware capabilities of target MCU's for Micro Python and (3) propose a solution for two independent watchdog systems - firstly a build in watchdog service in Micro Python and secondly the framework for users to add a more robust system in their Python code.

##  1. Best practice.
* Don't service the watchdog under a timer ISR. Timer ISR's often continue to run after a crash.
* Monitor correct program execution before servicing the watchdog.

## 2. Hardware capabilities.
* STM32F405 (pyboard). Independent Watchdog Timer (IWDT) with its own RC oscillator. Separate Window Watchdog Timer (WWDT) using system clock.

## 3a.  Built in Watchdog.
* Use the IWDT in Micro Python. This will recover from most hardware events and some software events.
* The IWDT will detect an oscillator (crystal failure). See if the STM32 can test the crystal oscillator after reset and run with the high speed RC oscillator if the crystal is dead.

## 3b. User Watchdog.
* Use the WWDT for user programs. Provide hooks in Micro Python for user Python code to manage this Watchdog.

## 4. Glossary
* WDT Watchdog timer. Hardware in the MCU or an external IC.
* WDSR.  Watchdog Service Routine. Software that services the watchdog to prevent it resetting the MCU.
* ISR.  Interrupt Service Routine.

## 5. References.

Great Watchdog Timers For Embedded Systems. Jack Ganssle. Version 1.3, updated September, 2011.
     http://www.ganssle.com/watchdogs.htm


