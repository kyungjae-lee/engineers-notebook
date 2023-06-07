<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > MCU Peripheral Drivers

# MCU Peripheral Drivers



### Introduction to Microcontroller

1. **<a href="./memory-map">Memory Map</a>**
2. **<a href="./mcu-bus-interfaces">MCU Bus Interfaces</a>**
3. **<a href="./mcu-block-diagram">MCU Block Diagram</a>**
4. **<a href="./clocks">Clocks</a>**
5. **<a href="./vector-table">Vector Table</a>**
6. **<a href="./interrupts">Interrupts</a>**



### MCU Peripheral Driver Development

1. **<a href="./architecture-and-development-plan">Architecture & Development Plan</a>**
1. **<a href="./device-header-file">Device Header File (`stm32f407xx.h`)</a>**

##### General Purpose Input/Output (GPIO) Driver

1. **<a href="./introduction-to-gpio">Introduction to GPIO</a>**
1. **<a href="./gpio-driver">GPIO Driver (`stm32f407xx_gpio_driver.h/.c`)</a>**
1. **<a href="./gpio-application-1">GPIO Application 1: Toggling On-board LED (`gpio_01_led_toggle_pushpull.c`, `gpio_01_led_toggle_opendrain.c`)</a>**
1. **<a href="./gpio-application-2">GPIO Application 2: Toggling On-board LED with On-board Button (`gpio_02_led_toggle_with_button.c`)</a>**
1. **<a href="./gpio-application-3">GPIO Application 3: Toggling External LED with External Button (`gpio_03_ext_led_toggle_with_ext_button.c`)</a>**
1. **<a href="./gpio-application-4">GPIO Application 4: Toggling LED with External Button Interrupt (`gpio_04_led_toggle_with_ext_button_interrupt.c`)</a>**

##### Serial Peripheral Interface (SPI) Driver

1. **<a href="./introduction-to-spi">Introduction to SPI</a>**
1. **<a href="./common-problems-in-spi-and-debugging-tips">Common Problems in SPI & Debugging Tips</a>**
1. **<a href="./spi-driver">SPI Driver (`stm32f407xx_spi_driver.h/.c`)</a>**
1. **<a href="./spi-application-1">SPI Application 1: Master Only Tx (Blocking) (`spi_01_master_only_tx_blocking.c`)</a>**
1. **<a href="./spi-application-2">SPI Application 2: Master-Slave Tx (Blocking) (`spi_02_master_slave_tx_blocking.c`)</a>**
1. **<a href="./spi-application-3">SPI Application 3: Master-Slave Tx Rx (Blocking) (`spi_03_master_slave_tx_rx_blocking.c`)</a>**
1. **<a href="./spi-application-4">SPI Application 4: Master-Slave Rx (Interrupt) (`spi_04_master_slave_rx_interrupt`)</a>**
