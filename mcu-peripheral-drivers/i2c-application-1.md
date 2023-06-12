<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">MCU Peripheral Drivers</a> > I2C Application 1: Master-Slave Tx (`i2c_01_master_slave_tx.c`)

# I2C Application 1: Master-Slave Tx (`i2c_01_master_slave_tx.c`)



## Requirements

* I2C master (STM32 Discovery board) and I2C slave (Arduino board) communication.

* When the button on the STM32 board (master) is pressed, the master shall send data to the Arduino board (slave). The data received by the Arduino board shall be displayed on the serial monitor terminal of the Arduino IDE.

  1. Use I2C SCL = 100 kHz (i.e., standard mode)
  2. Use external pull-up resistors (3.3 kΩ) for SDA and SCL line

  [!] Note: If you don't have external pull-up resistors, you can also try activating the STM32 I2C pin's internal pull-up resistors.

### External Pull-Up Resistance Calculation


$$
R_p(max) = \frac{t_r}{0.8473 \times C_b}
$$

### Parts Needed

1. Arduino board

2. STM32 board

3. Logic level converter

4. Breadboard and jumper wires

5. 2 pull-up resistors of resistance 3.3 kΩ or 4.7 kΩ 

   (You can also use internal pull-up resistors of the pins in place of external resistors.)

### STM32 Board and Arduino Board Communication Interfaces



<img src="img/i2c-application-communication-interfaces.png" alt="i2c-application-communication-interfaces" width="700">








## Code

### `i2c_01_master_slave_tx.c`

Path: `Project/Src/`

```c

```

