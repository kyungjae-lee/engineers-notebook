<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">MCU Peripheral Drivers</a> > USART Driver (`stm32f407xx_usart_driver.h/.c`)

# USART Driver (`stm32f407xx_usart_driver.h/.c`)



## SPI Driver API Requirements

* USART initialization / Peripheral clock control

* USART Tx

* USART Rx

* USART interrupt configuration & handling

* Other USART management APIs


### SPIx Peripheral Configurable Items for User Applications

* USART_Mode
  * Transmitter mode
  * Receiver mode
  * Transmitter/receiver mode

* USART_Baud
  * Baudrate

* USART_NumOfSTOPBits
* USART_WordLength
  * 8-bit
  * 9-bit

* USART_ParityControl
  * No parity
  * Even parity
  * Odd parity

* USART_HWFlowControl


### Exercise

1. Create `stm32f407xx_usart_driver.c` and `stm32f407xx_usart_driver.h`
2. Complete USART register definition structure and other macros (e.g., peripheral base addresses, device definition, clock enable, clock disable, etc.) in the MCU specific header file.
3. Add USART register bit definition macros in the MCU specific header file.
4. ADD USART configuration structure and USART handle structure in USART header file.



## Code

### `stm32f407xx_usart_driver.h`

Path: `Project/Drivers/Inc/`

```c

```



### `stm32f407xx_usart_driver.c`

Path: `Project/Drivers/Src/`

```c

```
