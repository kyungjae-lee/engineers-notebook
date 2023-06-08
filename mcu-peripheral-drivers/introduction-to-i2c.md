<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">MCU Peripheral Drivers</a> > Introduction to I2C

# Introduction to I2C



### Inter-Integrated Circuit (I2C) Protocol

* Pronounced as "I squared C" or "I two C"
* I2C is a protocol to achieve serial data communication between integrated circuits (ICs) which are close to each other. (Considered more serious protocol than SPI because companies have come to design a specification.)
* I2C protocol details such as how data should be sent/received, how hand shaking should happen between sender and receiver, error handling, etc. are more complex than those of SPI's. In other words, SPI is a simpler protocol compared to I2C.)



## Difference Between SPI and I2C

* I2C is based on a dedicated global specification which can be downloaded from https://www.nxp.com/docs/en/user-guide/UM10204.pdf.

  For SPI, however, there's no dedicated global specification. Only some vendors such as TI and Motorola have their own proprietary specifications.

* I2C supports multi-master mode, whereas SPI provides no official guidelines to achieve this (it depends on MCU designers or vendors.)

  e.g., STM SPI peripherals can be used in multi-master mode, but software is responsible for the arbitration. (In the case of I2C, arbitration is taken care of by the hardware automatically.)

  $\to$ One of the most important features of the I2C peripheral!

* I2C hardware automatically ACKs upon reception of every byte, whereas SPI does not support automatic ACKing. (Must be implemented in the software)
* I2C needs just 2 pins to connect all the masters and slaves, whereas SPI may need 4 or more pins depending on the number of slaves involved in communication.



<img src="img/i2c-2-pins.png" alt="i2c-2-pins" width="800">



* I2C master talks to slaves based on their addresses, whereas in SPI a dedicated pin (SS) is used to select the slave to talk to. (Each device has their own address.)

* I2C is half-duplex, whereas SPI is full-duplex.

* I2C's max speed is 4 MHz in ultra speed plus mode (which is supported by some higher-end MCUs). For some STM MCUs, the max speed is even slower (e.g., 400 KHz). However, SPI's max speed is its F~pclk~/2, which means that if the peripheral clock is 20 MHz, then SPI max speed can be 10 MHz.

  I2C is slower than SPI in general!

* In I2C, slave can make master wait by holding the clock down if it's busy thanks to the clock stretching feature (supported by most of the MCUs) of I2C. However, in SPI, slave has no control over the clock. Programmers must come up with their own tricks and solutions to overcome this situation.

* I2C's data rate (i.e., number of bits transferred from sender to receiver in 1 second) is much less than that of the SPI's. For example, when the STM32F4xx MCU supports the peripheral clock of 40 MHz, SPI can support up to 20 Mbps, whereas I2C can support only 400 Kbps.

  Due to this advantage of SPI over I2C, it is impossible to simply replace all SPI applications with that of I2Cs.



## I2C Terminology

* Definitions of I2C bus terminology

  | Term            | Description                                                  |
  | --------------- | ------------------------------------------------------------ |
  | Transmitter     | Device sending data (to bus)                                 |
  | Receiver        | Device receiving data (from bus)                             |
  | Master          | Device that initiates data transfer, generates clock signals and terminates data transfer |
  | Slave           | Device addressed by master                                   |
  | Multi-master    | More than one master can attempt to control the bus at the same time without corrupting the message |
  | Arbitration     | Procedure to ensure that if more than one master tries to control the bus simultaneously, only one is allowed to do so and the winning message is not corrupted |
  | Synchronization | Procedure to synchronize the clock signals of two or more devices |

  > I2C communication is always initiated by the master.



## SDA & SCL Signals

* Both the SDA and SCL are bidirectional lines connected to a positive supply voltage via pull-up resistors. When the bus is free, both lines are pulled to HIGH.
* The output stages of devices connected to the bus must have an open-drain or open-collector configuration.
* The bus capacitance limits the number of interfaces connected to the bus.
* Both the SDA and SCL pins must be in open-drain configuration and must use pull-up resistors (either internal or external).



<img src="img/i2c-pin-configuration.png" alt="i2c-pin-configuration" width="800">



* Troubleshooting tip: When the bus is idle, both the SDA and SCL are pulled to +V~DD~.

  Whenever you face problems in I2C, probe the SDA and SCL line after I2C initialization. (The voltage between GND-SDA(or SCL) should read 3.3V or 1.8V depending on the I/O voltage levels of the board.)



## I2C Modes

| Mode            | Data Rate      | Notes                                                    |
| --------------- | -------------- | -------------------------------------------------------- |
| Standard mode   | Up to 100 Kbps | Supported by STM32F4xx                                   |
| Fast mode       | Up to 400 Kbps | Supported by STM32F4xx                                   |
| Fast mode +     | Up to 1Mbps    | Supported by some STM32F4xx (check the reference manual) |
| High speed mode | Up to 3.4 Mbps | Not supported by STM32F4xx                               |

> bps: bits per second

### Standard Mode

* In standard mode communication, data transfer rate can reach up to maximum of 100 Kbps.
* Standard mode was the very first mode introduced when the first I2C specification was released.
* Standard-mode devices, however, are not forward compatible. (They cannot communicate with the devices of fast mode or above.)
* Standard-mode devices include basic sensors such as RTC, temperature sensor, etc. (The devices that does not require fast communication.)

### Fast Mode

* In fast mode, devices can transmit and receive data up to 400 Kbps.
* Fast-mode devices are backward-compatible and can communicate with standard-mode devices in a 0-100 Kbps I2C bus system.
* Standard-mode devices, however, are not forward compatible. So, they should not be incorporated in a fast-mode I2C bus system as they cannot follow the faster transfer rate which in turn can lead to unpredictable states.
* To achieve data transfer rate up to 400 Kbps, you must put the I2C device in the fast mode.
