<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">MCU Peripheral Drivers</a> > SPI Driver (`stm32f407xx_spi_driver.h/.c`)

# SPI Driver (`stm32f407xx_spi_driver.h/.c`)



## SPI Driver API Requirements

* SPI initialization
* SPI peripheral clock control
* SPI Tx
* SPI Rx
* SPI interrupt configuration & handling
* Other SPI management APIs

### SPIx Peripheral Configurable Items for User Applications

* SPI_DeviceMode
  * Master / slave
* SPI_BusConfig
  * Full-duplex (default) / half-duplex / simplex
* SPI_DFF
  * 8-bit data format (default) / 16-bit data format
  * Shift register width will be decided accordingly.
* SPI_CPHA
  * Defaults to 0
* SPI_CPOL
  * Defaults to 0
* SPI_SSM
  * Software slave management (SSM = 1) / Hardware slave management (SSM = 0)
* SPI_SCLKSpeed
  * Required serial clock speed



## Code

### `stm32f407xx_spi_driver.h`

Path: `Project/Drivers/Inc/`

```c

```



### `stm32f407xx_spi_driver.c`

Path: `Project/Drivers/Src/`

```c

```
