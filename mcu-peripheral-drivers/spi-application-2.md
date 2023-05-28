<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">MCU Peripheral Drivers</a> > SPI Application 2: Master-Slave Communication (`02_spi-master-slave-communication.c`)

# SPI Application 2: Master-Slave Communication (`02_spi-master-slave-communication.c`)



## Requirements

* SPI master(STM) and SPI slave(Arduino) communication
* When the button on the master is pressed, master shall send a string of data to the slave in connection. The data received by the slave shall be displayed on the slave's serial port.
* Requirements
  1. Use SPI full duplex mode
  2. ST board will be in SPI master mode, and Arduino board will be in SPI slave mode.
  3. Use DFF = 0
  4. Use hardware slave management (SSM = 0)
  5. SCLK speed = 2MHz, f~clk~ = 16MHz
* In this application, master is not going to receive anything from the slave. So, configuring the MISO pin is not necessary.
* The slave does not know how many bytes of data master is going to send. So, the master must first send the number of bytes the slave should expect to receive.

### Parts Needed

1. Arduino board
2. STM32 board
3. Logic level converter
4. Breadboard and jumper wires




## Code

### `02_spi-master-slave-communication.c`

Path: `Project/Src/`

```c

```


