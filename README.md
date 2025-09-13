# STM32 SPI ADC Communication Project

##  Overview
This project demonstrates SPI communication between two STM32 microcontrollers (STM32F411RE as Master and STM32F103C8T6 as Slave). The master reads analog input using ADC and sends the digital value over SPI. The slave receives the value and displays it via UART on the serial monitor.

---

## Boards and Roles
| Board          | Role   | Function                                           |
|----------------|--------|----------------------------------------------------|
| STM32F411RE    | Master | Reads analog voltage via ADC and sends over SPI   |
| STM32F103C8T6  | Slave  | Receives ADC value via SPI and prints over UART   |

---

## Software Requirements and Installation

### 1. **STM32CubeMX**
- Use to configure clocks, peripherals (SPI, ADC, UART, GPIO)
- Download: https://www.st.com/en/development-tools/stm32cubemx.html

### 2. **Keil uVision (MDK-ARM)**
- IDE for writing, compiling, and uploading firmware
- Download: https://www.keil.com/download/product/

### 3. **ST-Link Utility**
- Used to program STM32 boards and view memory contents
- Download: https://www.st.com/en/development-tools/stsw-link004.html

### 4. **Tera Term (or Arduino IDE Serial Monitor)**
- Serial monitor to view UART output from slave
- Download: https://github.com/TeraTermProject/teraterm/releases

---

##  Pin Configuration Summary

### SPI (Full-Duplex, 2-Wire):
| Signal | STM32F411 (Master) | STM32F103 (Slave) |
|--------|--------------------|-------------------|
| MOSI   | PA7                | PA7               |
| MISO   | PA6                | PA6               |
| SCK    | PA5                | PA5               |
| NSS    | PA4                | PA4 (HW NSS)      |

### ADC:
- **Pin**: PA0 (on STM32F411)
- **Channel**: ADC_CHANNEL_0

### UART (Slave):
- **TX (STM32F103)**: PA9 (connects to FT232RL RX)
- **Baud Rate**: 115200

---

## Physical Connections

| Signal | Connection                    |
|--------|-------------------------------|
| MOSI   | PA7 (F411) <-> PA7 (F103)     |
| MISO   | PA6 (F411) <-> PA6 (F103)     |
| SCK    | PA5 (F411) <-> PA5 (F103)     |
| NSS    | PA4 (F411) <-> PA4 (F103)     |
| GND    | Common GND between boards     |
| UART   | PA9 (F103 TX) -> FT232RL RX   |

---

##  Steps to Generate and Build Code

### 1. Create New STM32CubeMX Projects
- Open STM32CubeMX and then click on File and choose New Project.
- In the MCU/MPU selecter enter the commercial part number as STM32F411RET6 , then double click on the result shown , and new project loads.
- Next in the pinout configuration , under connectivity section click on  SPI1 , and then choose the mode as Full-Duplex Master, and Hardware NSS as disable.
- Then go to Analog section and selct ADC and choose IN0.
- Next switch to the Project Manager Tab and then name your project and then create a directory to store the projects and paste that path in ToolChain Folder Location and then choose toolchain/ide as MDK-ARM , and then click on generate code , you will be directed to keil ide .
- If any of the firmware packages are missing download it .
- Create  another project for  STM32F103C8T6 (Slave)
- In Pinout View choose PC13 as GPIO_Output , PA9 as USART1_TX, PA10 as USART1_RX.
- Now in "SYS" Category choose the debug as serial wire and Time base Source as "SysTick".
- Then in Connectivity Section choose SPI1 , and choose mode as Full duplex Slave and Hardware NSS Signal as Hardware NSS input Signal.
- Then choose USART1 and select the mode to be asynchronous and hardware flow control as disbale, and also just below that you will notice configuration tab , choose NVIC Settings and enable the USART1 global interrupt.
- Similarly save file and click on  generate code .

### 2. Open in Keil IDE

- Write the code in the `main.c` file  under Application/User/Core subfolder.
- Then right click on the folder which has project name , and select "options for target" and then in the "target" tab choose `arm complier` as default version and next switch to Debug tab and choose `Use` as ST-Link Debugger, then click on ok and exit the window.
- Next right click on the project again and select build .
-Next power your nucleo board  and then load it into your board.
- In case of blue-pill board just build the project , to load it go to ST link utility and connect the board to the pc via ST-Link V2 , then click on firmware update and then select device and update the firmware package, then click on connect option , and then click on program and verify in order to load the code to slave device. 

### 3. Run and Monitor Output
- Power both the boards
- Also connect the slave to  FT232RL which in turn is connected to pc.
- Then Use Tera Term as serial monitor and select the ports (by verifying it on Device Manager) and then set the baud rates appropriately to view the output.
- STM32F103C8T6 powered via ST-Link V2
- STM32F411RE powered via USB (Vin supports 5V input)
- Ensure all devices share common GND


---

##  Notes
- Ensure ADC input does not exceed 3.3V (use voltage divider for 5V)
- UART output is unidirectional (Slave TX -> PC RX)
- Test with hardcoded values to debug SPI before using ADC

---

## Author
- K.Sethu Madhav(2022EEB1186), A.D.V.M.S.Nikhil(2022EEB1165)

