<a href="../">Notebook</a> > <a href="./">Projects</a> > RTOS from Scratch

# RTOS from Scratch

==This project is currently IN PROGRESS. Contents will be continuously updated (04/13/2023)!==

To see the full source code, visit my GitHub repository ([https://github.com/kyungjae-lee/rtos-from-scratch](https://github.com/kyungjae-lee/rtos-from-scratch)).



## Introduction

A real‐time operating system with board support package developed from scratch without using libraries on STM32‐Discovery board



## Objective

* Understand how the real-time operating systems concepts are implemented in code.
  * Context switch, memory management, scheduling algorithms, etc.
* Understand which part of the code in an RTOS kernel is hardware dependent.
* Be able to debug RTOS software issues.



## Demonstration

### Round-Robin scheduling algorithm (Time quantum: 500 ms)

<iframe width="560" height="315" src="https://www.youtube.com/embed/eqX8kh5V9WA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Round-Robin scheduling algorithm (Time quantum: 50 ms)

<iframe width="560" height="315" src="https://www.youtube.com/embed/VdAeSHJr9FY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>



## Source Code

[!] Note: This section only presents the test program and the kernel code. Source code for the peripheral drivers used in testing can be found in [Peripheral Drivers from Scratch](./peripheral-drivers-from-scratch).

### Test Program

```c
/*****************************************************************************************
 * @ File name		: kernel.c
 * @ Description	: Test program for RTOS kernel
 * @ Author			: Kyungjae Lee
 * @ Date created	: 04/13/2023
 ****************************************************************************************/

#include <stdio.h>
#include "stm32f407xx_led.h"
#include "stm32f407xx_usart.h"
#include "kernel.h"

void task0_handler(void);
void task1_handler(void);
void task2_handler(void);

int main(void)
{
	/* Initialize USART */
	usart_tx_init();
	printf("Initializing USART...\n\r");

	/* Initialize USART */
	led_init();
	printf("Initializing LEDs...\n\r");

	/* Initialize kernel */
	kernel_init();
	printf("Initializing kernel...\n\r");

	/* Create tasks */
	create_tasks(task0_handler, task1_handler, task2_handler);
	printf("Creating tasks...\n\r");

	/* Start kernel */
	start_kernel();
	printf("Starting kernel...\n\r");
}

void task0_handler(void)
{
	while (1)
	{
		//printf("task0\n\r");
		led_green_off();
		led_blue_on();
	}
}

void task1_handler(void)
{
	while (1)
	{
		//printf("task1\n\r");
		led_blue_off();
		led_red_on();
	}
}

void task2_handler(void)
{
	while (1)
	{
		//printf("task2\n\r");
		led_red_off();
		led_green_on();
	}
}
```

### Kernel

```c
/*****************************************************************************************
 * @ File name		: kernel.h
 * @ Description	: Interface for kernel
 * @ Author			: Kyungjae Lee
 * @ Date created	: 04/11/2023
 ****************************************************************************************/

#ifndef KERNEL_H
#define KERNEL_H

#include <stdint.h>
#include "stm32f4xx.h"

/* Initializes kernel */
void kernel_init(void);

/* Starts kernel */
void start_kernel(void);

/* Creates three tasks */
uint8_t create_tasks(void (*task0)(void), void (*task1)(void), void (*task2)(void));

/*
 * Task calling this function will relinquish the CPU and trigger context switch for
 * other task to run.
 */
void task_yield(void);

#endif
```

```c
/*****************************************************************************************
 * @ File name		: kernel.c
 * @ Description	: Implementation of RTOS kernel
 * @ Author			: Kyungjae Lee
 * @ Date created	: 04/11/2023
 ****************************************************************************************/

#include <stdint.h>
#include "kernel.h"

#define NUM_TASKS			3
#define STACK_SIZE			300
#define PROC_CLK_FREQ		16000000
#define RR_TIME_QUANTUM		50			/* in milliseconds */

/* SysTick Control and Status Register */
#define ENABLE				(1U << 0)	/* Enable the counter */
#define TICKINT				(1U << 1)	/* Enable SysTick esception request */
#define CLKSOURCE			(1U << 2)	/* Clock source = processor clock */
#define COUNTFLAG			(1U << 16)	/* Return 1 if timer counted to 0 since last time this was read */

/* SysTick Reload Value Register */
#define SYST_RVR			(*(uint32_t volatile *)0xE000E014U)

/* SysTick Current Value Register */
#define SYST_CVR			(*(uint32_t volatile *)0xE000E018U)

/* Interrupt Control and State Register */
#define ICSR				(*(uint32_t volatile *)0xE000ED04U)
#define PENDSTSET			(1U << 26)	/* SysTick exception set-pending bit */

void start_scheduler(void);

/* Task Control Block (TCB) */
typedef struct TCB
{
	uint32_t *p_stack;	/* Stack pointer */
	struct TCB *p_next;	/* Pointer to next TCB */
} TCB_Type;

/* Array of TCBs */
TCB_Type tcbs[NUM_TASKS];

/* Pointer to current TCB */
TCB_Type *p_curr;

/* Stack space for threads (Each thread will be given stack of size 200, i.e., 200 * 4 bytes) */
uint32_t stacks[NUM_TASKS][STACK_SIZE];

/* Initialize task's stack */
void stack_init(uint32_t tid)
{
	/*
	 * ARM Cortex-M processor's stack model - Full Descending (FD)
	 * Stack frame: r0-r3, r12, lr, pc, xpsr (8 byte data)
	 */

	/* Initialize xPSR - Set bit[24] (T-bit of EPSR) to operate in Thumb mode */
	stacks[tid][STACK_SIZE - 1] |= (1U << 24);

	/* Initialize SP  */
	tcbs[tid].p_stack = &stacks[tid][STACK_SIZE - 16]; /* Top of stack assuming all 16 processor core register has been pushed */

	/*
	 * Initialize the rest of the stack content with a dummy value.
	 * This is optional and for debugging purpose only.
	 */
	stacks[tid][STACK_SIZE - 3] 	= 0xAAAAAAAA;	/* r14 (LR) */
	stacks[tid][STACK_SIZE - 4] 	= 0xAAAAAAAA;	/* r12 */
	stacks[tid][STACK_SIZE - 5] 	= 0xAAAAAAAA;	/* r3  */
	stacks[tid][STACK_SIZE - 6] 	= 0xAAAAAAAA;	/* r2  */
	stacks[tid][STACK_SIZE - 7] 	= 0xAAAAAAAA;	/* r1  */
	stacks[tid][STACK_SIZE - 8] 	= 0xAAAAAAAA;	/* r0  */

	stacks[tid][STACK_SIZE - 9] 	= 0xAAAAAAAA;	/* r11 */
	stacks[tid][STACK_SIZE - 10] 	= 0xAAAAAAAA;	/* r10 */
	stacks[tid][STACK_SIZE - 11] 	= 0xAAAAAAAA;	/* r9  */
	stacks[tid][STACK_SIZE - 12] 	= 0xAAAAAAAA;	/* r8  */
	stacks[tid][STACK_SIZE - 13] 	= 0xAAAAAAAA;	/* r7  */
	stacks[tid][STACK_SIZE - 14] 	= 0xAAAAAAAA;	/* r6  */
	stacks[tid][STACK_SIZE - 15] 	= 0xAAAAAAAA;	/* r5  */
	stacks[tid][STACK_SIZE - 16] 	= 0xAAAAAAAA;	/* r4  */
}

/* Creates three tasks */
uint8_t create_tasks(void (*task0)(void), void (*task1)(void), void (*task2)(void))
{
	/* Disable global interrupts */
	__disable_irq();

	/*
	 * Initialize task0
	 */

	/* Initialize next pointer */
	tcbs[0].p_next = &tcbs[1];

	/* Initialize PC */
	stacks[0][STACK_SIZE - 2] = (uint32_t)task0;

	/* Initialize stack */
	stack_init(0);


	/*
	 * Initialize task1
	 */

	/* Initialize next pointer */
	tcbs[1].p_next = &tcbs[2];

	/* Initialize PC */
	stacks[1][STACK_SIZE - 2] = (uint32_t)task1;

	/* Initialize stack */
	stack_init(1);


	/*
	 * Initialize task2
	 */

	/* Initialize next pointer */
	tcbs[2].p_next = &tcbs[0];

	/* Initialize PC */
	stacks[2][STACK_SIZE - 2] = (uint32_t)task2;

	/* Initialize stack */
	stack_init(2);


	/* Enable global interrupts */
	__enable_irq();

	return 1;
}

/* Initializes kernel */
void kernel_init(void)
{
	/* Reset SysTick Control and Status Register */
	SysTick->CTRL = 0x0;		/* Force reset to 0 since it resets to either 0x0 or 0x4 */

	/* Clear SysTick Current Value Register */
	SysTick->VAL = 0x0;

	/* Load Round-Robin time quantum */
	SysTick->LOAD = (RR_TIME_QUANTUM * (PROC_CLK_FREQ / 1000)) - 1;

	/* Set SysTick to low priority */
	NVIC_SetPriority(SysTick_IRQn, 15);

	/* Enable counter, enable SysTick interrupt, select internal clock */
	SysTick->CTRL |= (ENABLE | TICKINT | CLKSOURCE);
}

/* Starts kernel */
void start_kernel(void)
{
	/* Start scheduler */
	start_scheduler();
}

/*
 * Launches the very first task by updating the processor's SP to the first task's SP, and
 * restoring the first task's context.
 */
void start_scheduler(void)
{
	/* Start scheduling with task0 */
	p_curr = &tcbs[0];

	/* Disable global interrupts. */
	// __asm("cpsid i");	// Do we need synchronization here?

	/* Load r0 with the address of p_curr. */
	__asm("ldr r0, =p_curr");

	/* Load r2 with p_curr. [r2] <- [[r0]]; */
	__asm("ldr r2, [r0]");

	/* Load sp with [r2]. sp = p_curr->p_stack. */
	__asm("ldr sp, [r2]");

	/* Pop (restore) r4-r11 from stack. */
	__asm("pop {r4-r11}");

	/* Pop (restore) r0-r3 from stack. */
	__asm("pop {r0-r3}");

	/* Pop (restore) r12 from stack. */
	__asm("pop {r12}");

	/* Skip LR in the stack. */
	__asm("add sp, sp, #4");

	/* Update processor's LR with the PC value in the stack. */
	__asm("pop {lr}");

	/* Skip PSR in the stack. */
	__asm("add sp, sp, #4");

	/* Enable global interrupts. */
	// __asm("cpsie i");	// why enable when we didn't even disable in the first place?

	/* Return from the subroutine.*/
	__asm("bx lr");
}

/*
 * If a function is defined as a "Naked" function, the prologue and epilogue
 * will not be generated during compilation. This function is defined as a
 * naked function since we want to keep the stack pointer as is when entering
 * this function.
 */
 __attribute__((naked)) void SysTick_Handler(void)
{
	/*
	* Switch-out current task.
	*/

	/* Disable global interrupts. */
	__asm("cpsid i");

	/* Push r4-r11 on to stack since they are not part of those registers (stack frame)
	* that get pushed onto stack automatically by the processor as an exception entry
	* sequence. */
	__asm("push {r4-r11}");

	/* Load r0 with the address of the p_curr (&p_curr). */
	__asm("ldr r0, =p_curr");

	/* Load r1 with p_curr. */
	__asm("ldr r1, [r0]");

	/* Store the sp into the current TCB's p_stack. This works because p_stack is the
	* first member of a TCB struct. Start address of a TCB is the same as the address
	* of its p_stack. */
	__asm("str sp, [r1]");


	/*
	* Switch-in next task.
	*/

	/* Load r1 with p_curr->p_next. [r1] <- [[r1] + 4];  */
	__asm("ldr r1, [r1, #4]");

	/* Store r1 into the address r0 points to. p_curr = p_curr->p_next; Advance p_curr. */
	__asm("str r1, [r0]");

	/* Load SP with r1 value. Load SP with the SP of the next task to run. */
	__asm("ldr sp, [r1]");

	/* Pop (restore) r4-r11 from stack. Rest of the registers will be automatically popped
	* by the processor as an exception exit sequence. */
	__asm("pop {r4-r11}");

	/* Enable global interrupts. */
	__asm("cpsie i");

	/* Return from the subroutine. */
	__asm("bx lr");

	/* At the exit of exception, registers r0-r3, r12, lr, pc, psr will be automatically
	* restored from stack by the processor. */
}

/*
 * Task calling this function will relinquish the CPU and trigger context switch for
 * other task to run.
 */
void task_yield(void)
{
	/* Clear SysTick Current Value Register */
	SysTick->VAL = 0x0;

	/* Trigger SysTick.
	 * Interrupt Control and State Register -> bit[26]: SysTick exception set-pending bit.
	 * Set bit[26] to 1. */
	ICSR |= PENDSTSET;
	/* OR? or assign? Teaching assistant is saying, we are forcing SysTick to generate
	 * exception in order to switch to the next thread so, we are clearing everything
	 * and generate only SysTick exception. */
}
```

