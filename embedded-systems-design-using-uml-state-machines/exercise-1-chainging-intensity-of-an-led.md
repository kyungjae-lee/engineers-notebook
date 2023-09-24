<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">Embedded Systems Design using UML State Machines</a> > Exercise 1: Changing Intensity of an LED

# Exercise 1: Changing Intensity of an LED



## Overview

### Procedure

1. Connect the LED to any one of the PWM pins on the Arduino Uno board.
2. Modify the duty cycle of the PWM wave using the Arduino API's `analogWrite()` function.
3. Utilize the Arduino serial interface to transmit ON and OFF events from the host.

### Hardware



<img src="./img/arduino-uno-board.png" alt="arduino-uno-board" width="500">



* Arduino Uno board PWM pins
  * Pin 3, 5, 6, 9, 10, 11 are PWM pins
  * On these pins Arudino Uno can generate PWM signals
  * PWM signal frequency:
    * 490Hz on 3, 9, 10, 11
    * 980Hz on 5 and 6
