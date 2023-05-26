<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">MCU Peripheral Drivers</a> > GPIO Application 2: Toggling On-board LED with On-board Button (`02_led_toggle_with_button.c`)

# GPIO Application 2: Toggling On-board LED with On-board Button (`02_led_toggle_with_button.c`)



## Requirements

* Write a program to toggle the on-board LED whenever the on-board button is pressed.
* In the case of STM32F407 Discovery board, when the user button is pressed, the GPIO pin connected to it is pulled to HIGH. (Check the schematic of the board. It is board specific!)



## Code

### `02_led_toggle_with_button.c`

Path: `Project/Src/`

```c
/**
 * Filename		: 02_led_toggle_with_button.c
 * Description	: Program to toggle the on-board LED whenever the on-board button is pressed
 * Author		: Kyungjae Lee
 * Created on	: May 23, 2023
 */

#include "stm32f407xx.h"

#define HIGH			1
#define BTN_PRESSED 	HIGH	/* This is not universal. Check the board schematic */

/* Spinlock delay */
void delay(void)
{
	for (uint32_t i = 0; i < 500000 / 2; i++);
}

int main(int argc, char *argv[])
{
	GPIO_Handle_TypeDef GPIOLed, GPIOBtn;

	/* GPIOLed configuration */
	GPIOLed.pGPIOx = GPIOD;
	GPIOLed.GPIO_PinConfig.GPIO_PinNumber = GPIO_PIN_12;
	GPIOLed.GPIO_PinConfig.GPIO_PinMode = GPIO_PIN_MODE_OUT;
	GPIOLed.GPIO_PinConfig.GPIO_PinSpeed = GPIO_PIN_OUT_SPEED_FAST;
	GPIOLed.GPIO_PinConfig.GPIO_PinOutType = GPIO_PIN_OUT_TYPE_PP;
	GPIOLed.GPIO_PinConfig.GPIO_PinPuPdControl = GPIO_PIN_NO_PUPD;
	GPIO_PeriClockControl(GPIOLed.pGPIOx, ENABLE);
	GPIO_Init(&GPIOLed);

	/* GPIOBtn configuration */
	GPIOBtn.pGPIOx = GPIOA;
	GPIOBtn.GPIO_PinConfig.GPIO_PinNumber = GPIO_PIN_0;
	GPIOBtn.GPIO_PinConfig.GPIO_PinMode = GPIO_PIN_MODE_IN;
	GPIOBtn.GPIO_PinConfig.GPIO_PinSpeed = GPIO_PIN_OUT_SPEED_FAST; /* Doesn't matter */
	//GPIOBtn.GPIO_PinConfig.GPIO_PinOutType = GPIO_PIN_OUT_TYPE_PP;	/* N/A */
	GPIOBtn.GPIO_PinConfig.GPIO_PinPuPdControl = GPIO_PIN_NO_PUPD;
		/* External pull-down resistor is already present (see the schematic) */
	GPIO_PeriClockControl(GPIOBtn.pGPIOx, ENABLE);
	GPIO_Init(&GPIOBtn);

	while (1)
	{
		if (GPIO_ReadFromInputPin(GPIOBtn.pGPIOx, GPIOBtn.GPIO_PinConfig.GPIO_PinNumber) == BTN_PRESSED)
		{
			delay();	/* Introduce debouncing time */
			GPIO_ToggleOutputPin(GPIOLed.pGPIOx, GPIOLed.GPIO_PinConfig.GPIO_PinNumber);
		}
	}

	return 0;
}
```
