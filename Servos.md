This page is about a mechanism for controlling Hobby Servos. There are two main kinds of Hobby Servo. Analog and Digital. Both are controlled identically. Digital servos can be updated more frequently.

For the complete list of servos and their specs you can't go past http://www.servodatabase.com/
Each servo is listed with its update rate (pulse cycle) and pulse width.

####Analog servos:
* Cheap.
* Generally have +/-90 degree rotations about a centered rest position
  * but can also be continuous rotation or other ranges (e.g. +/- 180 degree).
* Use a PWM mechanism to control their desired position with a central frequency approximately 1500 usecs (666 Hz).
* Need to get updates at from 20-65 times per second (Hz).

####Digital servos:
* have a higher refresh frequency - upto 400Hz and therefore:
  * hold position better, 
  * have better torque, speed, and accuracy
  * and therefore also use more power.
* are more expensive.

####PWM
So we need a PWM signal to control them. This is not the 'normal' use of PWM where the position is set by duty cycle (perhaps by integrating voltage over time as used for dimming Leds). This method is based on individual pulse widths, not a chain of pulses. This method is used because a R/C vehicle may lose its signal from time to time and the servos must stay in position in the absence of any pulse train.

So the pulse widths are: notionally 1ms (1000us) = full CCW rotation, 1.5ms (1500us) = detent, center, rest position, and 2ms (2000us) is maximum CW rotation. This 'standard' is not always used (detent may be at 1.8ms and maximum at 2.6ms). So we need these values to be adjustable.

The pulses are typically sent at 20Hz with rates as high as 65Hz. This is a repetition with a period of 50-15ms.

PWM signals can be generated in hardware or software.
The exact update frquency is (as mentioned) not critical but the pulse width is.We need to define the rest position accurately enough that the servo can sit in its rest position without jittering.

####Useful aspects to control:
* How many servos to control
  * more servos means less time spent on each one (in a loop) and a lower update rate.
  * resulting in 'soft' servo positioning where the servo will move if it is under load (which sometimes doesn't matter).
* PWM frequency range - from 1ms (1kHz) to 1.5ms (666kHz), to a max of 3ms (333kHz).
* PWM frequency accuracy especially around 1.5ms
  * 8 bit PWM would give us 255 steps. This can make it difficult to get the stationary center position. 
  * 12 bits gives us 4096 steps
  * Actual clock freq and accuracy is a function of many aspects of clocks in a chipset.
  *  Accuracy around 1.5ms is what's important.
* Update rate of 20-65 Hz

####Calculations:
If we update every 20ms and we have a maximum pulse width of 3ms - we can cycle through 6 servo pins with that signal. As we change update rates we get less servo pins we can service before the cycle is up.
Table period vs number of servos with a single PWM generator
```
Period  Freq    Max pulse width
                  3ms     2ms 
20Hz   (50ms)     16      25   servos
30Hz   (33ms)     11      16   servos
40Hz   (25ms)      8      12   servos
50Hz   (20ms)      6      10   servos  * standard update rate
60Hz   (17ms)      5       8   servos
70Hz   (14ms)      3       7   servos
```
So if we add more parallel timers we can service more servos.
This suggests the following mechanism:

**Using hardware timers:**

Where S = number of servos and R = rate of update in Hz.

1. Allocate a number of Timers based on the update rate and number of servos.
  * This can be minimised by querying the servos maximum pulse widths and calculating the maximum servos possible for that update rate and number of servos. Or conservatively, using the table above.
  * I.e. if need 50Hz and 12 servos then need 2 timers.
  * When you run out of Timers then update rate drops.

2. Have a software interrupt routine per Timer which interrupts R times a second and:
  * steps to the next servo in the list of servos for that Timer
  * reads the pin and pulse_width required for that servo from the python class.
  * sets the PWM freq to that pulse width
  * outputs the pulse on correct pin using anoeyhr s/w interrupt or hardware timer.
  * sleeps

The top level python structure sets the servo instances up and adds them to a servo pool. Which manages how many servos are in a Timer queue and setting up and activating the timer interrupt routines.

**Using Software timer:**

Need different interrupt routines for setting and resetting the pulses. No reason why it can't be as efficient as the hardware version.


####Hardware considerations:
The pyboard has 23 pins that can output hardware PWM. It has 12x 16bit timers and 2x 32bit timers. Only 4 pins are configured with servo headers on the main board but this is obviously for convenience only.

The Teensy3.1 has 12 pins that can output PWM but (I think) only 3 Timers. So a software interrupt routine would be an excellent library for the Teensy. The Teensy 3.1 has PWM 12bit resolution. Some data here: https://www.pjrc.com/teensy/td_pulse.html

So Servo setup might look like the following.
ISR defined within - need to be weritten in c so as to use interrupts. Initially just software based.
```
#!/usr/bin/python

### Potential Servo implementation ideas

### top level servo pool
# servos = Servo_pool(update_rate, Servo_pool.SOFTWARE) # optional rate in Hz
# servos.set_desired_update_rate(freq_in_Hz)
# servos.update_rate() # actual update rate
## 
# s1 = servos.add(pin, serrvoargs)
# s1.setup(min_width, detent_width, max_width)
# s1.move(percent) # set pulse width for this percent
# s1.remove() # deletes servo from pool
## Optionally if angle is important.
# s1.set_angles(min, max) # set angle values for rotate function
# s1.rotate(angle)

import Servo_ISR # C lib which exposes interrupt request routine for servos.
# has:
# - isr = Servo_ISR(type)  # creates a non-runing interrupt service routine for Servos (h/w or s/w)
# - Servo_ISR.start()      #
# - Servo_ISR.stop()       #
# - Servo_ISR.set_rate(Hz) # ISR will interrupt at this rate
# - Servo_ISR.set_active(pin_width_pairs) # list of python class variables of pin, pulse_width pairs
#                                          - E.g. [[s1.pin, s1.value], [s2.pin, s2.value]]
# - When started this ISR routine will step through the list one pair at each interrupt.
#   Setting the pin to active high pulse of that width in usecs
#   H/w interrupt wil exit very quickly as pulse happens without its attention
#   S/w interrupt wil need to wait or trigger itself to come back and switch pulse off at right time.

class Servo_pool(object):
    SOFTWRE = 0
    HARDWARE = 1
    
    def __init__(self, update_rate=50, ISR_type=Servo_pool.HARDWARE):
        " holds all servos, allocates to ISRs "
        self.pool = []
        self.desired_rate = update_rate # user wants this rate
        self.rate = update_rate         # the rate we can actually do (may be lower than desired)
        self.isrs = []

    class ISR(self):
        """ holds a number of servos - externally managed
            - rate is in Hz
        """
        IDLE = 0
        STARTED = 1
        
        def __init__(self, rate, ISR_type):
            self.isr_type = ISR_type
            self.my_servos = []
            self.pin_width_pairs = []
            self.state = ISR.IDLE
            self.isr = Servo_ISR(ISR_type)
            self.rate = rate
            self.set_rate(rate)

        def add_servo(self, servo):
            " "
            self.my_servos.append(servo) 
            self.pin_width_pairs.append([servo.pin, servo.value])
            
        def remove_servo(self, servo):
            " "
            self.my_servos.pop(servo)
            pos = self.pin_width_pairs.index([servo.pin, servo.value])
            self.pin_width_pairs.pop(pos)
            # !! deal with empty my_servos

        def start(self):
            self.isr.start()
            self.state = ISR.STARTED
            
        def stop(self):
            self.isr.stop()
            self.state = ISR.IDLE

        def set_rate(self, rate):
            self.isr.set_rate(rate) 
            

    def add(self, pin, *args, **kwargs):
        s = Servo(self, pin, *args, **kwargs)
        self.pool.append(s) # !! check for existing etc...
        # do we have enough ISRs
        # add servo to ISR's my_servos list
        # !! should we degrade update_rate ?
        if not self.isrs:
            isr = ISR(self.rate, self.isr_type)
            self.isrs.append(isr)
        else:
            if self.need_new_isr_p():
                isr = ISR(self.rate, self.isr_type)
                self.isrs.append(isr)
            else: # use existing (last)
                isr = self.isrs[-1]
        #
        isr.add_servo(s)
        if isr.state == ISR.IDLE:
            isr.start() # we added one - lets go!
        return s

    def remove_servo(self, servo):
        " servo wants itself removed form ISR routines and list of servos "
        pass # !!

    def need_new_isr_p(self):
        " do we need a new isr "
        # check to see if adding one more to latest isr group will bust interrupts
        if len(self.isrs[-1]) * 3/100.0 > 10.0/self.desired_rate:
            #rate will drop if we add - need new ISR
            return True
        else: return False
    

class Servo(object):
    " hold servo settings "
    def __init__(self, pool, pin, min_angle=-90, detent=0, max_angle=90, min_width=1000, detent_width=1500, max_width=2000):
        self.pool = pool
        self.pin = pin
        self.min_width = min_width
        self.detent_width = detent_width
        self.max_width = max_width
        #
        self.detent = 0
        self.min_angle = min_angle
        self.max_angle = max_angle
        #
        self.value = detent_width  # pulse width used by ISR routine

    def move(self, percent):
        " move servo to this percent of range defined by angles"
        # !! set self.value to pulse width needed.
        # !! two linear interps - min-detent, detent-max
        pass
    def rotate(self, angle):
        " move servo to angle as cal using min. max angles "
        # !! set self.value to pulse width needed
        # !! two linear interps - min-detent, detent-max
        pass

    def remove(self):
        " take self out of servo pool "
        self.pool.remove_servo(self)
 
```