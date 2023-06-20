<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">MCU Peripheral Drivers</a> > I2C Driver (`stm32f407xx_i2c_driver.h/.c`)

# I2C Driver (`stm32f407xx_i2c_driver.h/.c`)



## SPI Driver API Requirements

* I2C initialization

  1. Configure the mode (e.g., standard, fast, ...)

  2. Configure the serial clock (SCL) speed (e.g., 25 KHz, 50 KHz, 100 KHz, 200 KHz, 400 KHz, ...)

     When using higher frequency, the length of the communication wire must be short.

     > In STM32F4x I2C peripheral, CR2 and CCR registers are used to control the I2C serial clock settings and other I2C timings like setup time and hold time.

  3. Configure the device address (Applicable only when the device is slave.)

  4. Enable ACKing (For STM32F407xx MCUs, ACKing is disabled by default.)

  5. Configure the rise time (the time it takes for the signal to reach the VCC level from the GND level) for I2C pins (Also called as "slew rate")

  All of the above configuration must be done while the I2C peripheral is in DISABLED state. (Check the control register!)

* I2C master Tx
  * Since in I2C, only master can initiate the communication, master Tx/Rx and slave Tx/Rx must be separated.

* I2C master Rx

* I2C slave Tx

* I2C slave Rx

* I2C error interrupt handling

* I2C event interrupt handling

### SPIx Peripheral Configurable Items for User Applications

* I2C_SCLSpeed
* I2C_DeviceAddress
* I2C_ACKControl
  * I2C automatic ACKing is disabled by default. This item will provide the user the ability to enable/disable ACKing.

* I2C_FMDutyCycle
  * This structure will give the user the ability to configure the duty cycle of the clock when the I2C peripheral is in "fast mode".


### Exercise

1. Create `stm32f407xx_i2c_driver.c` and `stm32f407xx_i2c_driver.h`
2. Add I2Cx related details to MCU specific header file
   * I2C peripheral register definition structure
   * I2Cx base address macros
   * I2Cx peripheral definition macros
   * Macros to enable and disable I2Cx peripheral clock
   * Bit position definitions of I2C peripheral



## Code

### `stm32f407xx_i2c_driver.h`

Path: `Project/Drivers/Inc/`

```c

```



### `stm32f407xx_i2c_driver.c`

Path: `Project/Drivers/Src/`

```c

```
