<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">MCU Peripheral Drivers</a> > SPI Application 1: Send Data (`01_spi_tx_data.c`)

# SPI Application 1: Send Data (`01_spi_tx_data.c`)



## Requirements

* Test the `SPI_TxData()` API to send the string "Hello world" with the following configurations:
  * SPI2 - Master mode
  * SCLK - Max possible
  * DFF = 0, and DFF = 1
* For this application, only MOSI and SCLK pins will be used. (No slave, so MISO, NSS pins are not necessary.) Find out the GPIO pins over which SPI2 can communicate! Look up the "Alternate function mapping" table in the datasheet.
  * **SPI2_MOSI $\to$ PB15 (AF5)**
  * **SPI2_SCK $\to$ PB13 (AF5)**
  * SPI2_MISO $\to$ PB14 (AF5)
  * SPI2_NSS $\to$ PB12 (AF5)




## Code

### `01_spi_tx_data.c`

Path: `Project/Src/`

```c

```

