<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">MCU Peripheral Drivers</a> > Application: Real-Time Clock on LCD (`rtc_lcd.c`)

# Application: Real-Time Clock on LCD (`rtc_lcd.c`)



## `rtc_lcd.c`

```c
/*******************************************************************************
 * Filename		: rtc_lcd.c
 * Description	: Application to read time and date information from DS1207 RTC
 *				  chip and display it on the 16x2 Character LCD
 * Author		: Kyungjae Lee
 * History 		: Jun 25, 2023 - Created file
 ******************************************************************************/

#include <lcd_hd44780u.h>
#include "rtc_ds1307.h"
#include <string.h> 		/* strlen() */
#include <stdio.h> 			/* printf() */
#include "stm32f407xx.h"

#define SYSTICK_TIM_CLK		16000000UL

/* SysTick timer registers */
#define SYST_CSR			(*(uint32_t volatile *)0xE000E010)
#define SYST_RVR			(*(uint32_t volatile *)0xE000E014)

/* SysTick Control and Status Register (SYST_CSR) bit fields */
#define SYST_CSR_ENABLE		(1 << 0U) /* Counter enabled */
#define SYST_CSR_TICKINT	(1 << 1U)
#define SYST_CSR_CLKSOURCE	(1 << 2U) /* Processor clock */

#define PRINT_ON_LCD	/* Comment this out if LCD is not installed */

/* Function prototypes */
char* TimeToString(RTC_Time_TypeDef *rtcTime);
char* DateToString(RTC_Date_TypeDef *rtcDate);
char* GetDay(uint8_t dayCode);
void NumToString(uint8_t num, char *buf);
void SysTickTimer_Init(uint32_t tickHz);
void DelayMs(uint32_t delayInMs);


int main(int argc, char *argv[])
{
	RTC_Time_TypeDef currTime;
	RTC_Date_TypeDef currDate;
	char *amPm;

	printf("RTC test application running...\n");

#ifndef PRINT_ON_LCD
    printf("RTC test without LCD");
#else
	LCD_Init();

    LCD_PrintString("RTC test on LCD");

    DelayMs(2000);

    /* Start printing from the beginning */
    LCD_ClearDisplay();
    LCD_ReturnHome();
#endif

	if (DS1307_Init())
	{
		/* RTC initialization was unsuccessful */
		printf("RTC init failed\n");

		while (1);
	}

	/* Initialize the SysTick Timer so it generates 1 interrupt per second */
	SysTickTimer_Init(1);

	currDate.day = FRIDAY;
	currDate.date = 25;
	currDate.month = 6;
	currDate.year = 23;		/* Only the last two digits of the year */

	currTime.hours = 11;
	currTime.minutes = 59;
	currTime.seconds = 30;
	currTime.timeFormat = TIME_FORMAT_12HRS_PM;

	DS1307_SetCurrentDate(&currDate);
	DS1307_SetCurrentTime(&currTime);

	DS1307_GetCurrentDate(&currDate);
	DS1307_GetCurrentTime(&currTime);

	/* Print current time *****************************************************/

	if (currTime.timeFormat != TIME_FORMAT_24HRS)
	{
		/* 12HRS format (e.g., 08:33:45 PM) */
		amPm = (currTime.timeFormat) ? "PM" : "AM";
#ifndef PRINT_ON_LCD
		printf("Current time = %s %s\n", TimeToString(&currTime), amPm);
#else
		LCD_PrintString(TimeToString(&currTime));
		LCD_PrintString(amPm);
#endif
	}
	else
	{
		/* 24HRS format (e.g., 20:33:45) */
#ifndef PRINT_ON_LCD
		printf("Current time = %s\n", TimeToString(&currTime));
#else
		LCD_PrintString(TimeToString(&currTime));
#endif
	}

	/* Print current date in 'MM/DD/YY <Day>' format **************************/

#ifndef PRINT_ON_LCD
	printf("Current date = %s <%s>\n", DateToString(&currDate), GetDay(currDate.day));
#else
	LCD_SetCursor(1, 2);
	LCD_PrintString(DateToString(&currDate));
#endif

	while (1);

	return 0;
} /* End of main */

/**
 * TimeToString()
 * Desc.	: Converts the time represented by @rtcTime to a string
 * Param.	: @rtcTime - pointer to RTC Time structure
 * Return	: Time in string format (e.g., HH:MM:SS PM)
 * Note		: N/A
 */
char* TimeToString(RTC_Time_TypeDef *rtcTime)
{
	static char buf[9]; /* Make it static not to return a dangling ptr */

	buf[2] = ':';
	buf[5] = ':';

	NumToString(rtcTime->hours, buf);
	NumToString(rtcTime->minutes, &buf[3]);
	NumToString(rtcTime->seconds, &buf[6]);

	buf[8] = '\0';

	return buf;
} /* End of TimeToString */

/**
 * DateToString()
 * Desc.	: Converts the date represented by @rtcDate to a string
 * Param.	: @rtcDate - pointer to RTC Date structure
 * Return	: Date in string format (e.g., MM/DD/YY <Day>)
 * Note		: N/A
 */
char* DateToString(RTC_Date_TypeDef *rtcDate)
{
	static char buf[9]; /* Make it static not to return a dangling ptr */

	buf[2] = '/';
	buf[5] = '/';

	NumToString(rtcDate->month, buf);
	NumToString(rtcDate->date, &buf[3]);
	NumToString(rtcDate->year, &buf[6]);

	buf[8] = '\0';

	return buf;
} /* End of DateToString */

/**
 * GetDay()
 * Desc.	: Gets the day represented by @dayCode in string format
 * Param.	: @dayCode - day macro
 * Return	: Day in string format (e.g., Sunday)
 * Note		: N/A
 */
char* GetDay(uint8_t dayCode)
{
	char *days[] = {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"};

	return days[dayCode];
} /* End of GetDay */

/**
 * NumToString()
 * Desc.	: Converts the passed uint8_t number @num and store it into @buf
 * Param.	: @num - a number to be converted into a string
 * 			  @buf - pointer to a string buffer
 * Return	: None
 * Note		: N/A
 */
void NumToString(uint8_t num, char *buf)
{
	if (num < 10)
	{
		buf[0] = '0';
		buf[1] = num + '0';
	}
	else if (10 <= num && num < 99)
	{
		buf[0] = num / 10 + '0';
		buf[1] = num % 10 + '0';
	}
} /* End of NumToString */

/**
 * SysTickTimer_Init()
 * Desc.	: Initializes the SysTick Timer
 * Param.	: @tickHz - number of interrupts to be triggered per second
 * Return	: None
 * Note		: Counting down to zero asserts the SysTick exception request.
 */
void SysTickTimer_Init(uint32_t tickHz)
{
	/* Calculate reload value */
	uint32_t reloadVal = (SYSTICK_TIM_CLK / tickHz) - 1;

	/* Clear the least significant 24 bits in the SYST_RVR */
	SYST_RVR &= ~0x00FFFFFF;

	/* Load the counter start value into SYST_RVR */
	SYST_RVR |= reloadVal;

	/* Configure SYST_CSR */
	SYST_CSR |= (SYST_CSR_TICKINT | SYST_CSR_CLKSOURCE | SYST_CSR_ENABLE);
		/* TICKINT	: Enable SysTick exception request */
		/* CLKSOURCE: Specify clock source; processor clock source */
		/* ENABLE	: Enable counter */
} /* End of SysTickTimer_Init */

/**
 * SysTick_Handler()
 * Desc.	: Handles the SysTick exception
 * Param.	: None
 * Return	: None
 * Note		: N/A
 */
void SysTick_Handler(void)
{
	RTC_Time_TypeDef currTime;
	RTC_Date_TypeDef currDate;
	char *amPm;

	DS1307_GetCurrentTime(&currTime);

	/* Print current time *****************************************************/

	if (currTime.timeFormat != TIME_FORMAT_24HRS)
	{
		/* 12HRS format (e.g., 08:33:45 PM) */
		amPm = (currTime.timeFormat) ? "PM" : "AM";
#ifndef PRINT_ON_LCD
		printf("Current time = %s %s\n", TimeToString(&currTime), amPm);
#else
		LCD_SetCursor(1, 1);
		LCD_PrintString(TimeToString(&currTime));
		LCD_PrintString(amPm);
#endif
	}
	else
	{
		/* 24HRS format (e.g., 20:33:45) */
#ifndef PRINT_ON_LCD
		printf("Current time = %s\n", TimeToString(&currTime));
#else
		LCD_SetCursor(1, 1);
		LCD_PrintString(TimeToString(&currTime));
#endif
	}

	/* Print current date in 'MM/DD/YY <Day>' format **************************/

	DS1307_GetCurrentDate(&currDate);
#ifndef PRINT_ON_LCD
	printf("Current date = %s <%s>\n", DateToString(&currDate), GetDay(currDate.day));
#else
	LCD_SetCursor(1, 1);
	LCD_PrintString(DateToString(&currDate));
	LCD_PrintChar('<');
	LCD_PrintString(GetDay(currDate.day));
	LCD_PrintChar('>');
#endif
} /* End of SysTick_Handler */

/**
 * DelayMs()
 * Desc.	: Spinlock delays for @delayInMs milliseconds
 * Param.	: @delayInMs - time to delay in milliseconds
 * Returns	: None
 * Note		: N/A
 */
void DelayMs(uint32_t delayInMs)
{
	for (uint32_t i = 0; i < delayInMs * 1000; i++);
} /* End of DelayMs */
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
