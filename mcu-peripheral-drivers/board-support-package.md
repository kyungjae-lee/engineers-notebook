<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">MCU Peripheral Drivers</a> > Board Support Package (BSP) (`rtc_ds1307.h/.c`, `lcd_hd44780u.h/.c`)

# Board Support Package (BSP) (`rtc_ds1307.h/.c`, `lcd_hd44780u.h/.c`)



## `rtc_ds1307.h`

* Contains device (RTC) related information
  * I2C address (For this project, slave address)
  * Register addresses (Registers from which the date and time information can be extracted)
  * Data structure to handle information
  * Prototypes of the functions that are to be exposed to user applications
  * Application configurable items

```c
/*******************************************************************************
 * Filename		: rtc_ds1307.h
 * Description	: APIs for DS1307 RTC module
 * Author		: Kyungjae Lee
 * History 		: Jun 25, 2023 - Created file
 ******************************************************************************/

#ifndef RTC_DS1307_H
#define RTC_DS1307_H

#include "stm32f407xx.h"

/* Application configurable items */
#define DS1307_I2C				I2C1
#define DS1307_I2C_GPIO_PORT	GPIOB
#define DS1307_I2C_PIN_SCL		GPIO_PIN_6
#define DS1307_I2C_PIN_SDA		GPIO_PIN_7
#define DS1307_I2C_SPEED		I2C_SCL_SPEED_SM /* Doesn't support fast mode */
#define DS1307_I2C_PUPD			GPIO_PIN_PU		 /* Using internal pull-up R */

/* Register addresses */
#define DS1307_SEC				0x00
#define DS1307_MIN				0x01
#define DS1307_HR				0x02
#define DS1307_DAY				0x03
#define DS1307_DATE  			0x04
#define DS1307_MONTH			0x05
#define DS1307_YEAR				0x06

/* DS1307 I2C address */
#define DS1307_I2C_ADDR			0x68 /* 1101000(2) */

/* Time formats */
#define TIME_FORMAT_12HRS_AM 	0
#define TIME_FORMAT_12HRS_PM	1
#define TIME_FORMAT_24HRS	 	2

/* Days */
#define SUNDAY					0
#define MONDAY					1
#define TUESDAY					2
#define WEDNESDAY				3
#define THURSDAY				4
#define FRIDAY					5
#define SATURDAY				6

typedef struct
{
	uint8_t date;
	uint8_t month;
	uint8_t year;
	uint8_t day;
} RTC_Date_TypeDef;

typedef struct
{
	uint8_t seconds;
	uint8_t minutes;
	uint8_t hours;
	uint8_t	timeFormat;
} RTC_Time_TypeDef;

/*******************************************************************************
 * APIs (See the function definitions for more information)
 ******************************************************************************/

uint8_t DS1307_Init(void);
void DS1307_SetCurrentTime(RTC_Time_TypeDef *);
void DS1307_GetCurrentTime(RTC_Time_TypeDef *);
void DS1307_SetCurrentDate(RTC_Date_TypeDef *);
void DS1307_GetCurrentDate(RTC_Date_TypeDef *);


#endif /* RTC_DS1307_H */
```



## `rtc_ds1307.c`

```c
/*******************************************************************************
 * Filename		: rtc_ds1307.c
 * Description	: Implementation of APIs for DS1307 RTC module
 * Author		: Kyungjae Lee
 * History 		: Jun 25, 2023 - Created file
 ******************************************************************************/

#include <rtc_ds1307.h>
#include <string.h> 	/* memset() */
#include <stdint.h>

/* Private function prototypes */
static void DS1307_I2CPinConfig(void);
static void DS1307_I2CConfig(void);
static void DS1307_Write(uint8_t value, uint8_t regAddr);
static uint8_t DS1307_Read(uint8_t regAddr);
static uint8_t BcdToBinary(uint8_t bcd);
static uint8_t BinaryToBcd(uint8_t binary);

/* Global variables */
I2C_Handle_TypeDef gDS1307I2CHandle;

/**
 * DS1307_Init()
 * Desc.	: Initializes DS1307 RTC module
 * Param.	: None
 * Return	: 0 if setting DS1307_SEC CH bit to 0 was successful (init success),
 * 			  1 otherwise (init fail)
 * Note		: N/A
 */
uint8_t DS1307_Init(void)
{
	/* 1. Initialize the I2C pins */
	DS1307_I2CPinConfig();

	/* 2. Initialize the I2C peripheral */
	DS1307_I2CConfig();

	/* 3. Enable the I2C peripheral */
	I2C_PeriControl(DS1307_I2C, ENABLE);

	/* 4. Set Clock Halt (CH) bit to 0 to initiate time keeping
	 * Note: At power on, the time keeping will not function until the CH bit
	 * 		 becomes 0. So, to initiate the time keeping functionality the
  	 *		 master needs to write 0 to the slave's CH bit.
  	 *		 For data write logic, see the "I2C Data Bus" section of DS1307
  	 *		 reference manual.
  	 */
	DS1307_Write(0x0, DS1307_SEC);

	/* 5. Read back Clock Halt (CH) bit
	 * Note: For data read logic, see the "I2C Data Bus" section of DS1307
	 * 		 reference manual.
	 */
	uint8_t clockState = DS1307_Read(DS1307_SEC);

	return ((clockState >> 7) & 0x1);
}

/**
 * DS1307_SetCurrentTime()
 * Desc.	: Sets the DS1307 Seconds, Minutes, Hours registers according to
 * 			  the values configured in @rtcTime
 * Param.	: @rtcTime - pointer to the RTC Time structure which contains the
 * 			  values configured by the user
 * Return	: None
 * Note		: N/A
 */
void DS1307_SetCurrentTime(RTC_Time_TypeDef *rtcTime)
{
	uint8_t secs, hrs;

	/* Set seconds ************************************************************/

	secs = BinaryToBcd(rtcTime->seconds);

	/* Make sure to keep the CH bit (bit[7]) of the Seconds register */
	secs &= ~(0x1 << 7);

	DS1307_Write(secs, DS1307_SEC);

	/* Set minutes ************************************************************/

	DS1307_Write(BinaryToBcd(rtcTime->minutes), DS1307_MIN);

	/* Set hours **************************************************************/

	hrs = BinaryToBcd(rtcTime->hours);

	if (rtcTime->timeFormat == TIME_FORMAT_24HRS)
	{
		/* To use 24HRS time format, clear bit[6] of the Hours register */
		hrs &= ~(0x1 << 6);
	}
	else
	{
		/* To use 12HRS time format, set bit[6] of the Hours register */
		hrs |= (0x1 << 6);

		/* If PM, set bit[5] of the Hours register, if AM, clear it */
		hrs = (rtcTime->timeFormat == TIME_FORMAT_12HRS_PM) ? hrs | (0x1 << 5) : hrs & ~(0x1 << 5);
	}

	DS1307_Write(hrs, DS1307_HR);
} /* End of DS1307_SetCurrentTime */

/**
 * DS1307_GetCurrentTime()
 * Desc.	:
 * Param.	:
 * Return	: None
 * Note		: N/A
 */
void DS1307_GetCurrentTime(RTC_Time_TypeDef *rtcTime)
{
	uint8_t secs, hrs;

	/* Get seconds ************************************************************/

	secs = DS1307_Read(DS1307_SEC);
	secs &= ~(0x1 << 7); 	/* Exclude unnecessary bits */
	rtcTime->seconds = BcdToBinary(secs);

	/* Get minutes ************************************************************/

	rtcTime->minutes = BcdToBinary(DS1307_Read(DS1307_MIN));

	/* Get hours **************************************************************/

	hrs = DS1307_Read(DS1307_HR);

	if (hrs & (0x1 << 6))
	{
		/* 12HRS format */
		rtcTime->timeFormat = !((hrs & (0x1 << 5)) == 0);
		hrs &= ~(0x3 << 5);	/* Clear bit[6] and bit[5] */
	}
	else
	{
		/* 24HRS format */
		rtcTime->timeFormat = TIME_FORMAT_24HRS;
	}

	rtcTime->hours = BcdToBinary(hrs);
} /* End of DS1307_GetCurrentTime */

/**
 * DS1307_SetCurrentDate()
 * Desc.	:
 * Param.	:
 * Return	: None
 * Note		: N/A
 */
void DS1307_SetCurrentDate(RTC_Date_TypeDef *rtcDate)
{
	DS1307_Write(BinaryToBcd(rtcDate->date), DS1307_DATE);
	DS1307_Write(BinaryToBcd(rtcDate->month), DS1307_MONTH);
	DS1307_Write(BinaryToBcd(rtcDate->year), DS1307_YEAR);
	DS1307_Write(BinaryToBcd(rtcDate->day), DS1307_DAY);
} /* End of DS1307_SetCurrentDate */

/**
 * DS1307_GetCurrentDate()
 * Desc.	:
 * Param.	:
 * Return	: None
 * Note		: N/A
 */
void DS1307_GetCurrentDate(RTC_Date_TypeDef *rtcDate)
{
	rtcDate->day = BcdToBinary(DS1307_Read(DS1307_DAY));
	rtcDate->date = BcdToBinary(DS1307_Read(DS1307_DATE));
	rtcDate->month = BcdToBinary(DS1307_Read(DS1307_MONTH));
	rtcDate->year = BcdToBinary(DS1307_Read(DS1307_YEAR));
} /* End of DS1307_GetCurrentDate */


/*******************************************************************************
 * Private functions
 ******************************************************************************/

/**
 * DS1307_I2CPinConfig()
 * Desc.	:
 * Param.	:
 * Return	: None
 * Note		: N/A
 */
static void DS1307_I2CPinConfig(void)
{
	GPIO_Handle_TypeDef i2cSda, i2cScl;

	/* Zero-out all the fields in the structures (Very important! i2cSda, i2cScl
	 * are local variables whose members may be filled with garbage values before
	 * initialization. These garbage values may set (corrupt) the bit fields that
	 * you did not touch assuming that they will be 0 by default. Do NOT make this
	 * mistake!
	 */
	memset(&i2cSda, 0, sizeof(i2cSda));
	memset(&i2cScl, 0, sizeof(i2cScl));

	/*
	 * I2C1_SCL: PB6
	 * I2C1_SDA: PB7
	 */

	i2cSda.pGPIOx = DS1307_I2C_GPIO_PORT;
	i2cSda.GPIO_PinConfig.GPIO_PinMode = GPIO_PIN_MODE_ALTFCN;
	i2cSda.GPIO_PinConfig.GPIO_PinAltFcnMode = 4;
	i2cSda.GPIO_PinConfig.GPIO_PinNumber = DS1307_I2C_PIN_SDA;
	i2cSda.GPIO_PinConfig.GPIO_PinOutType= GPIO_PIN_OUT_TYPE_OD;
	i2cSda.GPIO_PinConfig.GPIO_PinPuPdControl = DS1307_I2C_PUPD;
	i2cSda.GPIO_PinConfig.GPIO_PinSpeed = GPIO_PIN_OUT_SPEED_HIGH;

	GPIO_Init(&i2cSda);

	i2cScl.pGPIOx = DS1307_I2C_GPIO_PORT;
	i2cScl.GPIO_PinConfig.GPIO_PinMode = GPIO_PIN_MODE_ALTFCN;
	i2cScl.GPIO_PinConfig.GPIO_PinAltFcnMode = 4;
	i2cScl.GPIO_PinConfig.GPIO_PinNumber = DS1307_I2C_PIN_SCL;
	i2cScl.GPIO_PinConfig.GPIO_PinOutType= GPIO_PIN_OUT_TYPE_OD;
	i2cScl.GPIO_PinConfig.GPIO_PinPuPdControl = DS1307_I2C_PUPD;
	i2cScl.GPIO_PinConfig.GPIO_PinSpeed = GPIO_PIN_OUT_SPEED_HIGH;

	GPIO_Init(&i2cScl);
} /* End of DS1307_I2CPinConfig */

/**
 * DS1307_I2CConfig()
 * Desc.	: Configures and initializes DS1307 I2C peripheral
 * Param.	: None
 * Return	: None
 * Note		: N/A
 */
static void DS1307_I2CConfig(void)
{
	gDS1307I2CHandle.pI2Cx = DS1307_I2C;
	gDS1307I2CHandle.I2C_Config.I2C_ACKEnable = I2C_ACK_ENABLE;
	gDS1307I2CHandle.I2C_Config.I2C_SCLSpeed = DS1307_I2C_SPEED;
	I2C_Init(&gDS1307I2CHandle);
} /* End of DS1307_I2CConfig */

/**
 * DS1307_Write()
 * Desc.	: Writes @value to @regAddr
 * Param.	: @value - value to write to @regAddr
 *            @regAddr - DS1307 register address to write @value to
 * Return	: None
 * Note		: N/A
 */
static void DS1307_Write(uint8_t value, uint8_t regAddr)
{
	uint8_t tx[2];
	tx[0] = regAddr;
	tx[1] = value;
	I2C_MasterTxBlocking(&gDS1307I2CHandle, tx, 2, DS1307_I2C_ADDR, 0);

} /* End of DS1307_Write */

/**
 * DS1307_Read()
 * Desc.	: Writes @value to @regAddr
 * Param.   : @regAddr - DS1307 register address to read from
 * Return	: 1-byte data received from the slave (DS1307 RTC module)
 * Note		: N/A
 */
static uint8_t DS1307_Read(uint8_t regAddr)
{
	uint8_t rxData;
	I2C_MasterTxBlocking(&gDS1307I2CHandle, &regAddr, 1, DS1307_I2C_ADDR, 0);
	I2C_MasterRxBlocking(&gDS1307I2CHandle, &rxData, 1, DS1307_I2C_ADDR, 0);

	return rxData;
} /* End of DS1307_Read */

/**
 * BcdToBinary()
 * Desc.	: Converts the passed Binary-Coded Decimal (BCD) value to binary
 * Param.   : @bcd - binary-coded decimal value to be converted into binary
 * Return	: @bcd in binary representation
 * Note		: N/A
 */
static uint8_t BcdToBinary(uint8_t bcd)
{
	uint8_t tens, ones;

	tens = (uint8_t)((bcd >> 4) * 10);
	ones = bcd & (uint8_t)0xF;

	return tens + ones;
} /* End of BcdToBinary */

/**
 * BinaryToBcd()
 * Desc.	: Converts the passed binary value to Binary-Coded Decimal (BCD)
 * Param.   : @binary - binary value to be converted into binary-coded decimal
 * Return	: @binary in binary-coded decimal representation
 * Note		: N/A
 */
uint8_t BinaryToBcd(uint8_t binary)
{
	uint8_t tens, ones;
	uint8_t bcd;

	bcd = binary;

	if (binary >= 10)
	{
		tens = binary / 10;
		ones = binary % 10;
		bcd = (tens << 4) | ones;
	}

	return bcd;
} /* End of BinaryToBcd */
```



## `lcd_hd44780u.h`

```c
/*******************************************************************************
 * Filename		: lcd_hd44780u.h
 * Description	: APIs for 16x2 Character LCD module
 * Author		: Kyungjae Lee
 * History 		: Jun 27, 2023 - Created file
 ******************************************************************************/

#ifndef LCD_HD44780U_H
#define LCD_HD44780U_H

#include "stm32f407xx.h"

/* Application configurable items */
#define LCD_GPIO_PORT	GPIOD
#define LCD_PIN_RS		GPIO_PIN_0
#define LCD_PIN_RW		GPIO_PIN_1
#define LCD_PIN_EN		GPIO_PIN_2
#define LCD_PIN_D4		GPIO_PIN_3
#define LCD_PIN_D5		GPIO_PIN_4
#define LCD_PIN_D6		GPIO_PIN_5
#define LCD_PIN_D7		GPIO_PIN_6

/* LCD instructions */
#define LCD_INST_4DL_2N_5X8F	0x28 /* 4 data lines, 2 lines, 5x8 font size */
#define LCD_INST_DON_CURON		0x0E /* Display on, cursor on */
#define LCD_INST_INCADD			0x06 /* Entry mode set */
#define LCD_INST_CLEAR_DISPLAY	0x01 /* Clear display */
#define LCD_INST_RETURN_HOME	0x02 /* Return home */

/*******************************************************************************
 * APIs (See the function definitions for more information)
 ******************************************************************************/

void LCD_Init(void);
void LCD_TxInstruction(uint8_t instruction);
void LCD_ClearDisplay(void);
void LCD_ReturnHome(void);
void LCD_PrintChar(uint8_t ch);
void LCD_PrintString(char *msg);
void LCD_SetCursor(uint8_t row, uint8_t column);


#endif /* LCD_HD44780U_H */
```



## `lcd_hd44780u.c`

```c
/*******************************************************************************
 * Filename		: lcd_hd44780u.h
 * Description	: APIs for 16x2 Character LCD module
 * Author		: Kyungjae Lee
 * History 		: Jun 27, 2023 - Created file
 ******************************************************************************/

#ifndef LCD_HD44780U_H
#define LCD_HD44780U_H

#include "stm32f407xx.h"

/* Application configurable items */
#define LCD_GPIO_PORT	GPIOD
#define LCD_PIN_RS		GPIO_PIN_0
#define LCD_PIN_RW		GPIO_PIN_1
#define LCD_PIN_EN		GPIO_PIN_2
#define LCD_PIN_D4		GPIO_PIN_3
#define LCD_PIN_D5		GPIO_PIN_4
#define LCD_PIN_D6		GPIO_PIN_5
#define LCD_PIN_D7		GPIO_PIN_6

/* LCD instructions */
#define LCD_INST_4DL_2N_5X8F	0x28 /* 4 data lines, 2 lines, 5x8 font size */
#define LCD_INST_DON_CURON		0x0E /* Display on, cursor on */
#define LCD_INST_INCADD			0x06 /* Entry mode set */
#define LCD_INST_CLEAR_DISPLAY	0x01 /* Clear display */
#define LCD_INST_RETURN_HOME	0x02 /* Return home */

/*******************************************************************************
 * APIs (See the function definitions for more information)
 ******************************************************************************/

void LCD_Init(void);
void LCD_TxInstruction(uint8_t instruction);
void LCD_ClearDisplay(void);
void LCD_ReturnHome(void);
void LCD_PrintChar(uint8_t ch);
void LCD_PrintString(char *msg);
void LCD_SetCursor(uint8_t row, uint8_t column);


#endif /* LCD_HD44780U_H */
```
