<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">MCU Peripheral Drivers</a> > USART Application 2:  Tx Rx (Interrupt) (`usart_02_tx_rx_interrupt.c`)

# USART Application 2:  Tx Rx (Interrupt) (`usart_02_tx_rx_interrupt.c`)



## Requirements

* USART communication between STM32 Discovery board and Arduino Uno board.

* Write a program for STM32 board which transmits different messages to the Arduino board over UART communication.

  For every message the STM32 board sends, the Arduino code will change the case of alphabets (lower case to upper case and vice versa) and sends the message back to the STM32 board.

  The STM32 board shall capture the response from the Arduino board and display it on the STM32CubeIDE data console.

* Write a program to send some message over USART from STM32 board to Arduino board. The Arduino board will display the message sent from the STM32 board on the Arduino IDE serial monitor 
  1. Baudrate - 115200 bps
  1. Frame format - 1 stop bit, 8-bit user data, no parity


### Parts Needed

1. Arduino board
2. STM32 board
3. Logic level converter
4. Breadboard and jumper wires

### STM32 Board and Arduino Board Communication Interfaces



<img src="img/usart-application-2-communication-interfaces.png" alt="i2c-application-communication-interfaces" width="500">



### STM32 Board and Arduino Board Voltage Levels

* To work around the voltage level difference, a **logic level shifter** will be necessary.



<img src="./img/spi-application-2-stm32-arduino-voltage-levels.png" alt="spi-application-2-stm32-arduino-voltage-levels" width="700">

### Using `printf()` to Print Messages in STM32CubeIDE Console

* See <a href="./using-printf-with-serial-wire-viewer">Using `printf()` with Serial Wire Viewer (SWV)</a>



## Setup

### 1. Find out the GPIO pins that can be used for USART communication

* For this application, USART communication lines Tx, Rx will be used. Find out the GPIO pins over which USART can communicate! Look up the "Alternate function mapping" table in the datasheet.
  * USART2_TX $\to$ PA2 (AF7)
  
  * USART2_RX $\to$ PA3 (AF7)
  

### 2. Connect STM32 Discovery board with Arduino Uno board I2C pins

* Be careful not to directly supply 5 volts to the STM32 board pins when the board is not powered up as they may be damaged. When the **logic level shifter** is used, you don't need to worry about this issue.



<img src="img/usart-application-1-hardware-setup.png" alt="usart-application-1-hardware-setup" width="900">



* To analyze the communication with the logic analyzer, connect the channels as follows:

  * CH0 - STM32 USART Tx

  * CH1 - STM32 USART Rx

  * GND - Common GND of the bread board

### 3. Power Arduino board and upload SPI slave sketch to Arduino

* Sketch name: `002UARTTxString.ino`
  * You don't need to write an application for Arduino board. It is already provided as a sketch.
  * As soon as you upload this sketch to the Arduino board, it will operate as a UART counterpart.
* If you face an issue while uploading the sketch to the Arduino board, simply remove the Rx line, upload the sketch again and connect back the Rx line.




## Code

### `usart_02_tx_rx_interrupt.c`

Path: `Project/Src/`

```c

```



## Arduino Sketch (`002UARTTxString.ino`)

```c
void setup() {
  Serial.begin(115200);
  
  // Define the LED pin as Output
  pinMode (13, OUTPUT);
  
 // Serial.println("Arduino Case Converter program running");
 // Serial.println("-------------------------------------");
}

char changeCase(char ch)
{
  if (ch >= 'A' && ch <= 'Z')
  ch = ch + 32;
    else if (ch >= 'a' && ch <= 'z')
  ch = ch - 32;  

  return ch;
}
void loop() {

  digitalWrite(13, LOW); 
  //wait until something is received
  while(! Serial.available());
  digitalWrite(13, HIGH); 
  //read the data
  char in_read=Serial.read();
  //print the data
  Serial.print(changeCase(in_read));
}
```





## Testing

The following snapshots are taken using the Logic Analyzer.



### Entire Communication

<img src="img/usart-application-1-testing-entire-communication.png" alt="usart-application-1-testing-entire-communication" width="1000">



### Bit Time

<img src="img/usart-application-1-testing-bit-time.png" alt="usart-application-1-testing-bit-time" width="900">



### START / STOP Bits

<img src="img/usart-application-1-testing-start-stop-bits.png" alt="usart-application-1-testing-start-stop-bits" width="900">





### Arduino IDE Serial Monitor

<img src="img/usart-application-1-testing-arduino-ide.png" alt="usart-application-1-testing-arduino-ide" width="700">
