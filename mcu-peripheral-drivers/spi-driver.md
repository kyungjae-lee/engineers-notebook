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
/*
 * Filename		: stm32f407xx_spi_driver.h
 * Description	: STM32F407xx MCU specific SPI driver header file
 * Author		: Kyungjae Lee
 * History		: May 21, 2023 - Created file
 */

#ifndef STM32F407XX_SPI_DRIVER_H
#define STM32F407XX_SPI_DRIVER_H

#include "stm32f407xx.h"

/**
 * SPIx peripheral configuration structure
 */
typedef struct
{
	uint8_t SPI_DeviceMode;		/* Available values @SPI_DeviceMode 		*/
	uint8_t SPI_BusConfig;		/* Available values @SPI_BusConfig	 		*/
	uint8_t SPI_SCLKSpeed;		/* Available values @SPI_SCKLSpeed	 		*/
	uint8_t SPI_DFF;			/* Available values @SPI_DFF		 		*/
	uint8_t SPI_CPOL;			/* Available values @SPI_CPOL		 		*/
	uint8_t SPI_CPHA;			/* Available values @SPI_CPHA		 		*/
	uint8_t SPI_SSM;			/* Available values @SPI_SSM		 		*/
} SPI_Config_TypeDef;

/**
 * SPIx peripheral handle structure
 */
typedef struct
{
	SPI_TypeDef *pSPIx;	/* Holds the base address of the SPIx(x:0,1,2) peripheral */
	SPI_Config_TypeDef SPI_Config;
} SPI_Handle_TypeDef;

/**
 * @SPI_DeviceMode
 * Note: SPI_CR1 MSTR bit[2] - Master selection
 */
#define SPI_DEVICE_MODE_SLAVE			0
#define SPI_DEVICE_MODE_MASTER			1

/**
 * @SPI_BusConfig
 */
#define SPI_BUS_CONFIG_FULL_DUPLEX		0
#define SPI_BUS_CONFIG_HALF_DUPLEX		1
//#define SPI_BUS_CONFIG_SIMPLEX_TX_ONLY	2	// Simply removing Rx line = Same config as FD
#define SPI_BUS_CONFIG_SIMPLEX_RX_ONLY	2

/**
 * @SPI_CLKSpeed
 * Note: SPI_CR1 BR[5:3] - Baud rate control
 */
#define SPI_SCLK_SPEED_PRESCALAR_2		0
#define SPI_SCLK_SPEED_PRESCALAR_4		1
#define SPI_SCLK_SPEED_PRESCALAR_8		2
#define SPI_SCLK_SPEED_PRESCALAR_16		3
#define SPI_SCLK_SPEED_PRESCALAR_32		4
#define SPI_SCLK_SPEED_PRESCALAR_64		5
#define SPI_SCLK_SPEED_PRESCALAR_128	6
#define SPI_SCLK_SPEED_PRESCALAR_256	7

/**
 * @SPI_DFF
 * Note: SPI_CR1 DFF bit[11] - Data frame format/
 */
#define SPI_DFF_8BITS					0	/* Default */
#define SPI_DFF_16BITS					1

/**
 * @SPI_CPOL
 * Note: SPI_CR1 CPOL bit[1] - Clock polarity
 */
#define SPI_CPOL_LOW					0	/* Default */
#define SPI_CPOL_HIGH					1

/**
 * @SPI_CPHA
 * Note: SPI_CR1 CPHA bit[0] - Clock phase
 */
#define SPI_CPHA_LOW					0	/* Default */
#define SPI_CPHA_HIGH					1

/**
 * @SPI_SSM
 * Note: SPI_CR1 SSM bit[9] - Software slave management
 */
#define SPI_SSM_DI						0	/* Default */
#define SPI_SSM_EN						1


/*****************************************************************************************
 * APIs supported by the SPI driver (See function definitions for more information)
 ****************************************************************************************/

/**
 * Peripheral clock setup
 */
void SPI_PeriClockControl(SPI_TypeDef *pSPIx, uint8_t state);

/**
 * Init and De-init
 */
void SPI_Init(SPI_Handle_TypeDef *pSPIHandle);
void SPI_DeInit(SPI_TypeDef *pSPIx);	/* Utilize RCC_AHBxRSTR (AHBx peripheral reset register) */

/**
 * Data send and receive
 * - Blocking type: Non-interrupt based
 * - Non-blocking type: interrupt based
 * - DMA based (Will not be considered in this project)
 *
 * Note: Standard practice for choosing the size of 'length' variable is uint32_t or greater
 */
void SPI_TxData(SPI_TypeDef *pSPIx, uint8_t *pTxBuffer, uint32_t len);
void SPI_RxData(SPI_TypeDef *pSPIx, uint8_t *pRxBuffer, uint32_t len);

/**
 * IRQ configuration and ISR handling
 */
void SPI_IRQInterruptConfig(uint8_t irqNumber, uint8_t state);
void SPI_IRQPriorityConfig(uint8_t irqNumber, uint32_t irqPriority);
void SPI_IRQHandling(SPI_Handle_TypeDef *pSPIHandle);

/**
 * Other peripheral control APIs
 */
void SPI_PeriControl(SPI_TypeDef *pSPIx, uint8_t state);
void SPI_SSIConfig(SPI_TypeDef *pSPIx, uint8_t state);
void SPI_SSOEConfig(SPI_TypeDef *pSPIx, uint8_t state);

#endif /* STM32F407XX_SPI_DRIVER_H */
```



### `stm32f407xx_spi_driver.c`

Path: `Project/Drivers/Src/`

```c
/**
 * Filename		: stm32f407xx_spi_driver.c
 * Description	: STM32F407xx MCU specific SPI driver source file
 * Author		: Kyungjae Lee
 * History		: May 27, 2023 - Created file
 * 				  Jun 02, 2023 - Implemented 'SPI_RxData()'
 */

#include "stm32f407xx.h"

/*****************************************************************************************
 * APIs supported by the SPI driver (See function definitions for more information)
 ****************************************************************************************/

/**
 * Peripheral clock setup
 */

/**
 * SPI_PeriClockControl()
 * Desc.	: Enables or disables peripheral clock for SPIx
 * Param.	: @pSPIx - base address of SPIx peripheral
 * 			  @state - ENABLE or DISABLE macro
 * Returns	: None
 * Note		: N/A
 */
void SPI_PeriClockControl(SPI_TypeDef *pSPIx, uint8_t state)
{
	if (state == ENABLE)
	{
		if (pSPIx == SPI1)
			SPI1_PCLK_EN();
		else if (pSPIx == SPI2)
			SPI2_PCLK_EN();
		else if (pSPIx == SPI3)
			SPI3_PCLK_EN();
		else if (pSPIx == SPI4)
			SPI4_PCLK_EN();
	}
	else
	{
		if (pSPIx == SPI1)
			SPI1_PCLK_DI();
		else if (pSPIx == SPI2)
			SPI2_PCLK_DI();
		else if (pSPIx == SPI3)
			SPI3_PCLK_DI();
		else if (pSPIx == SPI4)
			SPI4_PCLK_DI();
	}
}

/**
 * Init and De-init
 */

/**
 * SPI_Init()
 * Desc.	:
 * Param.	: @pSPIHandle - pointer to the SPI handle structure
 * Returns	: None
 * Note		: For the serial communication peripherals (e.g., SPI, I2C, ...), there
 * 			  may be
 * 			  - one or more control registers where configurable parameters are stored
 * 			  - one or more data registers where user data is stored
 * 			  - one or more status registers where various status flags are stored.
 */
void SPI_Init(SPI_Handle_TypeDef *pSPIHandle)
{
	/* Enable peripheral clock */
	SPI_PeriClockControl(pSPIHandle->pSPIx, ENABLE);

	/**
	 * Configure SPIx_CR1 register
	 */

	uint32_t temp = 0;	/* Temporary register to store SPIx_CR1 configuration information */

	/* Configure device mode (MSTR bit[2] - Master selection) */
	temp |= pSPIHandle->SPI_Config.SPI_DeviceMode << 2;

	/* Configure bus config
	 *
	 * Full Duplex: BIDIMODE=0, RXONLY=0
	 * Simplex (unidirectional receive-only): BIDIMODE=0, RXONLY=1
	 * Half-Duplex, Tx: BIDIMODE=1, BIDIOE=1
	 * Half-Duplex, Rx: BIDIMODE=1, BIDIOE=0
	 */
	if (pSPIHandle->SPI_Config.SPI_BusConfig == SPI_BUS_CONFIG_FULL_DUPLEX)
	{
		/* Clear BIDIMODE bit[15] - 2-line unidirectional data mode selected */
		temp &= ~(1 << SPI_CR1_BIDIMODE);
	}
	else if (pSPIHandle->SPI_Config.SPI_BusConfig == SPI_BUS_CONFIG_HALF_DUPLEX)
	{
		/* Set BIDIMODE bit[15] - 1-line bidirectional data mode selected */
		temp |= (1 << SPI_CR1_BIDIMODE);
	}
	else if (pSPIHandle->SPI_Config.SPI_BusConfig == SPI_BUS_CONFIG_SIMPLEX_RX_ONLY)
	{
		/* Clear BIDIMODE bit[15] - 2-line unidirectional data mode selected */
		temp &= ~(1 << SPI_CR1_BIDIMODE);

		/* Set RXONLY bit[10] - Output disabled (Receive-only mode) */
		temp |= (1 << SPI_CR1_RXONLY);
	}

	/* Configure SPI serial clock speed (baud rate) */
	temp |= pSPIHandle->SPI_Config.SPI_SCLKSpeed << SPI_CR1_BR;

	/* Configure DFF */
	temp |= pSPIHandle->SPI_Config.SPI_DFF << SPI_CR1_DFF;

	/* Configure CPOL */
	temp |= pSPIHandle->SPI_Config.SPI_CPOL << SPI_CR1_CPOL;

	/* Configure CPHA */
	temp |= pSPIHandle->SPI_Config.SPI_CPHA << SPI_CR1_CPHA;

	/* Configure SSM */
	temp |= pSPIHandle->SPI_Config.SPI_SSM << SPI_CR1_SSM;

	pSPIHandle->pSPIx->CR1 = temp;
}

/**
 * SPI_DeInit()
 * Desc.	:
 * Param.	: @pSPIx - base address of SPIx peripheral
 * Returns	: None
 * Note		: N/A
 */
void SPI_DeInit(SPI_TypeDef *pSPIx)	/* Utilize RCC_AHBxRSTR (AHBx peripheral reset register) */
{
	/* Set and clear the corresponding bit of RCC_APBxRSTR to reset */
	if (pSPIx == SPI1)
		SPI1_RESET();
	else if (pSPIx == SPI2)
		SPI2_RESET();
	else if (pSPIx == SPI3)
		SPI3_RESET();
	else if (pSPIx == SPI4)
		SPI4_RESET();
}

/**
 * Data send and receive
 * - Blocking type: Non-interrupt based
 * - Non-blocking type: interrupt based
 * - DMA based (Will not be considered in this project)
 *
 * Note: Standard practice for choosing the size of 'length' variable is uint32_t or greater
 */

/**
 * SPI_TxData()
 * Desc.	: Send from @pSPIx the data of length @len stored in @pTxBuffer
 * Param.	: @pSPIx - base address of SPIx peripheral
 * 			  @pTxBuffer - address of the Tx buffer
 * 			  @len - length of the data to transmit
 * Returns	: None
 * Note		: This is a blocking function. This function will not return until
 *            the data is fully sent out.
 */
void SPI_TxData(SPI_TypeDef *pSPIx, uint8_t *pTxBuffer, uint32_t len)
{
	while (len > 0)
	{
		/* Wait until TXE (Tx buffer empty) bit is set */
		while (!(pSPIx->SR & (0x1 << SPI_SR_TXE)));	/* Blocking (Polling for the TXE flag to set) */

		/* Check DFF (Data frame format) bit in SPIx_CR1 */
		if (pSPIx->CR1 & (0x1 << SPI_CR1_DFF))
		{
			/* 16-bit DFF */
			/* Load the data into DR */
			pSPIx->DR = *((uint16_t *)pTxBuffer);	/* Make it 16-bit data */

			/* Decrement the length (2 bytes) */
			len--;
			len--;

			/* Adjust the buffer pointer */
			(uint16_t *)pTxBuffer++;
		}
		else
		{
			/* 8-bit DFF */
			/* Load the data into DR */
			pSPIx->DR = *pTxBuffer;

			/* Decrement the length (1 byte) */
			len--;

			/* Adjust the buffer pointer */
			pTxBuffer++;
		}
	}
}

/**
 * SPI_RxData()
 * Desc.	: Receive the data of length @len stored in @pRxBuffer
 * Param.	: @pSPIx - base address of SPIx peripheral
 * 			  @pRxBuffer - address of the Rx buffer
 * 			  @len - length of the data to transmit
 * Returns	: None
 * Note		: This is a blocking function. This function will not return until
 *            the data is fully received.
 */
void SPI_RxData(SPI_TypeDef *pSPIx, uint8_t *pRxBuffer, uint32_t len)
{
	while (len > 0)
	{
		/* Wait until RXNE (Rx buffer not empty) bit is set */
		while (!(pSPIx->SR & (0x1 << SPI_SR_RXNE)));	/* Blocking (Polling for the RXNE flag to set) */

		/* Check DFF (Data frame format) bit in SPIx_CR1 */
		if (pSPIx->CR1 & (0x1 << SPI_CR1_DFF))
		{
			/* 16-bit DFF */
			/* Read the data from DR into RxBuffer */
			*((uint16_t *)pRxBuffer) = pSPIx->DR;

			/* Decrement the length (2 bytes) */
			len--;
			len--;

			/* Adjust the buffer pointer */
			(uint16_t *)pRxBuffer++;
		}
		else
		{
			/* 8-bit DFF */
			/* Read the data from DR into RxBuffer */
			*pRxBuffer = pSPIx->DR;

			/* Decrement the length (1 byte) */
			len--;

			/* Adjust the buffer pointer */
			pRxBuffer++;
		}
	}
}

/**
 * IRQ configuration and ISR handling
 */

/**
 * ()
 * Desc.	:
 * Param.	: @
 * Returns	: None
 * Note		: N/A
 */
void SPI_IRQInterruptConfig(uint8_t irqNumber, uint8_t state)
{
}

/**
 * ()
 * Desc.	:
 * Param.	: @
 * Returns	: None
 * Note		: N/A
 */
void SPI_IRQPriorityConfig(uint8_t irqNumber, uint32_t irqPriority)
{
}

/**
 * ()
 * Desc.	:
 * Param.	: @
 * Returns	: None
 * Note		: N/A
 */
void SPI_IRQHandling(SPI_Handle_TypeDef *pSPIHandle)
{
}

/**
 * SPI_PeriControl()
 * Desc.	: Enables or disables SPI peripheral @pSPIx
 * Param.	: @pSPIx - base address of SPIx peripheral
 * 			  @state - ENABLE or DISABLE macro
 * Returns	: None
 * Note		: N/A
 */
void SPI_PeriControl(SPI_TypeDef *pSPIx, uint8_t state)
{
	if (state == ENABLE)
		pSPIx->CR1 |= (0x1 << SPI_CR1_SPE);		/* Enable */
	else
		pSPIx->CR1 &= ~(0x1 << SPI_CR1_SPE);	/* Disable */
}

/**
 * SPI_SSIConfig()
 * Desc.	: Sets or resets SPI CR1 register's SSI (Internal Slave Select) bit
 * Param.	: @pSPIx - base address of SPIx peripheral
 * 			  @state - ENABLE or DISABLE macro
 * Returns	: None
 * Note		: This bit has an effect only when the SSM bit is set.
 * 			  The value of this bit is forced onto the NSS pin and
 * 			  the IO value of the NSS pin is ignored.
 * 			  This bit is not used in I2S mode and SPI TI mode.
 */
void SPI_SSIConfig(SPI_TypeDef *pSPIx, uint8_t state)
{
	if (state == ENABLE)
		pSPIx->CR1 |= (0x1 << SPI_CR1_SSI);		/* Enable */
	else
		pSPIx->CR1 &= ~(0x1 << SPI_CR1_SSI);	/* Disable */
}

/**
 * SPI_SSOEConfig()
 * Desc.	: Sets or resets SPI CR2 register's SSOE (Slave Select Output Enable) bit
 * Param.	: @pSPIx - base address of SPIx peripheral
 * 			  @state - ENABLE or DISABLE macro
 * Returns	: None
 * Note		: When SSOE = 1, SS output is disabled in master mode and the cell
 * 			  can work in multimaster configuration
 * 			  When SSOE = 0, SS output is enabled in master mode and when the cell
 * 			  is enabled. The cell cannot work in a multimaster environment.
 */
void SPI_SSOEConfig(SPI_TypeDef *pSPIx, uint8_t state)
{
	if (state == ENABLE)
		pSPIx->CR2 |= (0x1 << SPI_CR2_SSOE);	/* Enable */
	else
		pSPIx->CR2 &= ~(0x1 << SPI_CR2_SSOE);	/* Disable */
}
```
