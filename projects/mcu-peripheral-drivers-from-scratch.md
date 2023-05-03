<a href="../">Home</a> > <a href="./">Projects</a> > MCU Peripheral Drivers from Scratch

# MCU Peripheral Drivers from Scratch



## Introduction

Developed minimal peripheral drivers (USART, LEDs, Button) for STM32F407-Discovery board to be used in the RTOS development project



## Objective

* Be able to write from scratch the peripheral drivers that initialize peripherals and provide necessary APIs for the 'Bare-Metal RTOS' development project
  * USART driver to print debug message to screen.
  * LEDs driver to check output
  * Button driver to check input
  * Hardware timer driven delay function to insert delays




## Demonstration

The following video demonstrates the functionality of the USART, LEDs, Button drivers and the delay function.

<iframe width="560" height="315" src="https://www.youtube.com/embed/RgoC9oNd7jY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>



## Development Environment

### Hardware

* **Target** - STM32F407 Discovery board

* USB-to-Serial Cable
* mini-USB cable

### Software 

* **IDE** - STM32 CubeIDE Version 1.10.1 (GCC compiler)
* **Host OS** - Ubuntu 22.04 LTS



## Source Code

### Test Program

```c
/*****************************************************************************************
 * @ File name		: stm32f407xx_button.c
 * @ Description	: Test program for stm32f407xx peripheral drivers
 * @ Author			: Kyungjae Lee
 * @ Date created	: 04/11/2023
 ****************************************************************************************/

#include <stdio.h>
#include "stm32f407xx_button.h"
#include "stm32f407xx_delay.h"
#include "stm32f407xx_led.h"
#include "stm32f407xx_usart.h"

uint32_t buttonState;

int main(int argc, char *argv[])
{
	/* Initialize USART */
	usart_tx_init();
	printf("\rInitializing USART...\n\r");
	delay_ms(500); /* Inserted to test delay_ms function */

	/* Initialize LEDs */
	led_init();
	printf("Initializing LEDs...\n\r");
	delay_ms(500);

	/* Initialize button */
	button_init();
	printf("Initializing User Button...\n\r");
	delay_ms(500);

	/* Test USART */
	printf("Board support package test program running...\n\r");
	delay_ms(500);
	printf("Press user button (blue) to turn on all four LEDs.\n\r");

	/* Test button and LEDs */
	while (1)
	{
		/* Read in button state */
		buttonState = button_read();

		/* Button pressed */
		if (buttonState)
		{
			led_green_on();
			led_orange_on();
			led_red_on();
			led_blue_on();
		}
		/* Button released */
		else
		{
			led_green_off();
			led_orange_off();
			led_red_off();
			led_blue_off();
		}
	}
}
```

### Chip Header

```c
/*****************************************************************************************
 * @ File name		: stm32f407xx.h
 * @ Description	: Chip header for stm32f407xx peripheral driver development
 * @ Author			: Kyungjae Lee
 * @ Date created	: 04/11/2023
 ****************************************************************************************/

#ifndef STM32F407xx_H
#define STM32F407xx_H

#include <stdint.h>

/* User LEDs */
#define LED_GREEN			(1U << 12)	/* PD12 */
#define LED_ORANGE			(1U << 13)	/* PD13 */
#define	LED_RED				(1U << 14)	/* PD14 */
#define LED_BLUE			(1U << 15)	/* PD15 */

#define LED_GREEN_MODE_OUTPUT		(1U << 24)
#define LED_ORANGE_MODE_OUTPUT		(1U << 26)
#define LED_RED_MODE_OUTPUT			(1U << 28)
#define LED_BLUE_MODE_OUTPUT		(1U << 30)

/* User button */
#define BUTTON				(1U << 0)	/* PA0 */

/* RCC */
#define RCC_BASE			0x40023800U
#define RCC_AHB1ENR			(*(uint32_t volatile *)(RCC_BASE + 0x30U))
#define RCC_APB1ENR			(*(uint32_t volatile *)(RCC_BASE + 0x40U))

/* GPIO */
#define GPIOA_BASE			0x40020000U
#define GPIOA_MODER			(*(uint32_t volatile *)(GPIOA_BASE + 0x00U))
#define GPIOA_IDR			(*(uint32_t volatile *)(GPIOA_BASE + 0x10U))
#define GPIOA_AFRL			(*(uint32_t volatile *)(GPIOA_BASE + 0x20U))

#define GPIOD_BASE			0x40020C00U
#define GPIOD_MODER			(*(uint32_t volatile *)(GPIOD_BASE + 0x00U))
#define GPIOD_ODR			(*(uint32_t volatile *)(GPIOD_BASE + 0x14U))

#define GPIOAEN				(1U << 0)	/* IO port A clock enable */
#define GPIODEN				(1U << 3)	/* IO port D clock enable */

/* Timer */
#define TIM3_BASE			0x40000400U
#define TIM3_CR1			(*(uint32_t volatile *)(TIM3_BASE + 0x00U))
#define TIM3_SR				(*(uint32_t volatile *)(TIM3_BASE + 0x10U))
#define TIM3_CNT			(*(uint32_t volatile *)(TIM3_BASE + 0x24U))
#define TIM3_PSC			(*(uint32_t volatile *)(TIM3_BASE + 0x28U))
#define TIM3_ARR			(*(uint32_t volatile *)(TIM3_BASE + 0x2CU))
#define CEN					(1U << 0)
#define TIM3EN				(1U << 1)	/* Timer 3 clock enable */

/* USART */
#define USART2_BASE		0x40004400U
#define USART2			(*(uint32_t volatile *)USART2_BASE)
#define USART2_SR		(*(uint32_t volatile *)(USART2_BASE + 0x00U))
#define USART2_DR		(*(uint32_t volatile *)(USART2_BASE + 0x04U))
#define USART2_BRR		(*(uint32_t volatile *)(USART2_BASE + 0x08U))
#define USART2_CR1		(*(uint32_t volatile *)(USART2_BASE + 0x0CU))
#define TE				(1U << 3)	// USART Control Register transmitter enable
#define UE				(1U << 13)	// USART Control Register USART enable
#define TXE				(1U << 7)	// USART Status Register Transmit data reg empty
#define USART2EN		(1U << 17)
#define USART_BAUDRATE	115200		// Desired USART baudrate (standard value)

/* Clock */
#define SYS_FREQ		16000000	// MCU data sheet "Clocks and Startup"
#define APB1_CLK		SYS_FREQ

#endif
```

### LED Driver

```c
/*****************************************************************************************
 * @ File name		: stm32f407xx_button.h
 * @ Description	: Interface for LED driver
 * @ Author			: Kyungjae Lee
 * @ Date created	: 04/11/2023
 ****************************************************************************************/

#ifndef STM32F407xx_LED_H
#define STM32F407xx_LED_H

#include "stm32f407xx.h"

/* Initializes LEDs */
void led_init(void);

/* Green  LED */
void led_green_on(void);
void led_green_off(void);
void led_green_toggle(void);

/* Green  LED */
void led_orange_on(void);
void led_orange_off(void);
void led_orange_toggle(void);

/* Red  LED */
void led_red_on(void);
void led_red_off(void);
void led_red_toggle(void);

/* Blue  LED */
void led_blue_on(void);
void led_blue_off(void);
void led_blue_toggle(void);

#endif
```

```c
/*****************************************************************************************
 * @ File name		: stm32f407xx_led.c
 * @ Description	: LED driver
 * @ Author			: Kyungjae Lee
 * @ Date created	: 04/11/2023
 ****************************************************************************************/

#include "stm32f407xx.h"

/* Initializes LEDs */
void led_init(void)
{
	/* Enable clock for GPIOD peripheral */
	RCC_AHB1ENR |= GPIODEN;

	/* Set LED pin mode to output mode */
	/* Clear */
	GPIOD_MODER &= ~(0xFFU << 24);
	/* Set */
	GPIOD_MODER |= (LED_GREEN_MODE_OUTPUT |
					LED_ORANGE_MODE_OUTPUT |
					LED_RED_MODE_OUTPUT |
					LED_BLUE_MODE_OUTPUT);
}

/*
 * Green LED
 */

void led_green_on(void)
{
	GPIOD_ODR |= LED_GREEN;
}

void led_green_off(void)
{
	GPIOD_ODR &= ~LED_GREEN;
}

void led_green_toggle(void)
{
	GPIOD_ODR ^= LED_GREEN;
}

/*
 * Orange LED
 */

void led_orange_on(void)
{
	GPIOD_ODR |= LED_ORANGE;
}

void led_orange_off(void)
{
	GPIOD_ODR &= ~LED_ORANGE;
}

void led_orange_toggle(void)
{
	GPIOD_ODR ^= LED_ORANGE;
}

/*
 * Red LED
 */

void led_red_on(void)
{
	GPIOD_ODR |= LED_RED;
}

void led_red_off(void)
{
	GPIOD_ODR &= ~LED_RED;
}

void led_red_toggle(void)
{
	GPIOD_ODR ^= LED_RED;
}

/*
 * Blue LED
 */

void led_blue_on(void)
{
	GPIOD_ODR |= LED_BLUE;
}

void led_blue_off(void)
{
	GPIOD_ODR &= ~LED_BLUE;
}

void led_blue_toggle(void)
{
	GPIOD_ODR ^= LED_BLUE;
}
```

### Button Driver

```c
/*****************************************************************************************
 * @ File name		: stm32f407xx_button.h
 * @ Description	: Interface for button driver
 * @ Author			: Kyungjae Lee
 * @ Date created	: 04/11/2023
 ****************************************************************************************/

#ifndef BSP_BUTTON_H
#define BSP_BUTTON_H

#include "stm32f407xx.h"

/* Initializes user button */
void button_init(void);

/* Reads button input data */
uint32_t button_read(void);

#endif
```

```c
/*****************************************************************************************
 * @ File name		: stm32f407xx_button.c
 * @ Description	: User button driver
 * @ Author			: Kyungjae Lee
 * @ Date created	: 04/11/2023
 ****************************************************************************************/

#include "stm32f407xx_button.h"

/* Initializes user button */
void button_init(void)
{
	/* Enable clock for GPIOA peripheral */
	RCC_AHB1ENR |= GPIOAEN;

	/* Set button pin mode to input mode */
	/* Clear */
	GPIOA_MODER &= ~(0x03U << 0);	/* Optional since this region will be 0 by default */
}

/* Reads the button input */
uint32_t button_read(void)
{
	return GPIOA_IDR & BUTTON;
}
```

### USART Driver

```c
/*****************************************************************************************
 * @ File name		: stm32f407xx_button.h
 * @ Description	: Interface for USART driver
 * @ Author			: Kyungjae Lee
 * @ Date created	: 04/11/2023
 ****************************************************************************************/

#ifndef STM32F407xx_USART_H
#define STM32F407xx_USART_H

#include "stm32f407xx.h"

/* Initializes USART */
void usart_tx_init(void);

#endif

```

```c
/*****************************************************************************************
 * @ File name		: stm32f407xx_usart.c
 * @ Description	: USART driver
 * @ Author			: Kyungjae Lee
 * @ Date created	: 04/11/2023
 ****************************************************************************************/

#include "stm32f407xx_usart.h"

static uint16_t compute_usart_bd(uint32_t periph_clk, uint32_t baudrate);
static void usart_set_baudrate(uint32_t periph_clk, uint32_t baudrate);
static void usart_write(int ch);

/* Allows to use 'printf()'. */
int __io_putchar(int ch)
{
	usart_write(ch);
	return ch;
}

/* Initializes USART */
void usart_tx_init(void)
{
	/* Enable clock access to GPIOA. */
	RCC_AHB1ENR |= GPIOAEN;

	/* Set PA2 mode to alternate function mode. */
	GPIOA_MODER &= ~(1U << 4);
	GPIOA_MODER |= (1U << 5);

	/* Set alternate function type to AF7 (USART2_TX) */
	GPIOA_AFRL |= (1U << 8);
	GPIOA_AFRL |= (1U << 9);
	GPIOA_AFRL |= (1U << 10);
	GPIOA_AFRL &= ~(1U << 11);


	/* Enable clock access to USART2. */
	RCC_APB1ENR |= USART2EN;

	/* Configure baudrate (the rate at which communication will take place). */
	usart_set_baudrate(APB1_CLK, USART_BAUDRATE);

	/* Configure transfer direction. */
	USART2_CR1 |= TE;	/* Enable transmitter */

	/* Enable USART module. */
	USART2_CR1 |= UE;	/* Enable USART */
}

/* Writes data */
static void usart_write(int ch)
{
	/* Make sure the transmit data register is empty. */
	while (!(USART2_SR & TXE)) {}

	/* Write to transmit data register */
	USART2_DR = (ch & 0xFF);
}

/* Sets the USART baudrate */
static void usart_set_baudrate(uint32_t periph_clk, uint32_t baudrate)
{
	USART2_BRR = compute_usart_bd(periph_clk, baudrate);
}

/* Computes the USART baudrate */
static uint16_t compute_usart_bd(uint32_t periph_clk, uint32_t baudrate)
{
	return (periph_clk + (baudrate / 2U)) / baudrate;
}
```

### Delay Function (ms)

```c
/*****************************************************************************************
 * @ File name		: stm32f407xx_delay.h
 * @ Description	: Interface for delay function
 * @ Author			: Kyungjae Lee
 * @ Date created	: 04/11/2023
 ****************************************************************************************/

#ifndef STM32F407xx_DELAY_H
#define STM32F407xx_DELAY_H

#include "stm32f407xx.h"

/* Imposes delay in milliseconds */
void delay_ms(uint32_t delay);

#endif
```

```c
/*****************************************************************************************
 * @ File name		: stm32f407xx_delay.c
 * @ Description	: Delay (spinlock) function
 * @ Author			: Kyungjae Lee
 * @ Date created	: 04/11/2023
 ****************************************************************************************/

#include "stm32f407xx_delay.h"

/* Imposes delay in milliseconds */
void delay_ms(uint32_t delay)
{
	/* Enable clock for TIM3 */
	RCC_APB1ENR |= TIM3EN;

	/*
	 * The counter clock frequency CK_CNT is equal to f_(CK_PSC) / (PSC[15:0] + 1).
	 * PSC contains the value to be loaded inthe active prescalar register at each
	 * update event.
	 */
	TIM3_PSC = 160 - 1; 	/* 16 000 000 (default clk freq) / 160 = 100 0000 Hz
							 * 즉, clock cycle 0 부터 159 까지 (160 clock cycle) 지날 때마다
							 * CNT 값을 1씩 증가시키겠다. */
	TIM3_ARR = 100 - 1;		/* 100 000 / 100 = 1000 Hz
	 	 	 	 	 	 	 * CNT 값이 0부터 99까지 count up 된다음 다시 0으로 리셋 될 때마다
	 	 	 	 	 	 	 * Update interrupt 발생시키고 UIF비트를 set 하겠다. 즉,
	 	 	 	 	 	 	 * 100 000 Hz로 증가하는 CNT를 총 100 번 카운트 할 때마다 한 번 UIF
	 	 	 	 	 	 	 * 비트를 set 하므로 최종적으로 UIF 비트는 1000 Hz로 set!
	 	 	 	 	 	 	 * 달리 말하면, UIF 비트는 1 ms 마다 한 번 씩 set! */
	TIM3_CNT = 0;			/* Counter value */
	TIM3_CR1 |= CEN; 		/* Counter enable */

	for (int i = 0; i < delay; i++)
	{
		/*
		 * UIF: Update interrupt flag (bit[0])
		 * 0: No update occurred
		 * 1: Update interrupt pending. This bit is set by hardware when the registers
		 * 	  are updated:
		 */
		while (!(TIM3_SR & 1)); 	/* wait for UIF set */
		TIM3_SR &= ~1;
	}
}
```

