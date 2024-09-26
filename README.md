

# Driver Assistance System Using CAN Bus

## Project Overview

This project implements a Driver Assistance System using CAN (Controller Area Network) communication. The system utilizes ultrasonic sensors to detect the distance between the vehicle and objects at the front and rear. The data is transmitted over the CAN bus, which can be used by other nodes for further processing.

## Features
- Measures distances using ultrasonic sensors at the front and rear of the vehicle.
- Transmits distance data via CAN bus for real-time communication.
- Implemented on an STM32 microcontroller.
- Real-time data transmission with error handling for robust communication.

## Hardware Requirements
- STM32 microcontroller (e.g., STM32F407)
- Two ultrasonic sensors (for front and rear distance measurement)
- CAN bus transceiver (e.g., MCP2551)
- Power supply (3.3V/5V depending on sensors)
- Jumper wires and breadboard for connections
- Oscilloscope (optional, for debugging CAN signals)

## Software Requirements
- STM32CubeMX and HAL libraries for peripheral configuration.
- Keil IDE / STM32CubeIDE for code development.
- CAN protocol for inter-device communication.
- Ultrasonic sensor drivers.

## System Architecture
1. **Ultrasonic Sensors**: Detects the distance from objects at the front and rear of the vehicle.
2. **STM32 MCU**: Reads data from the ultrasonic sensors, processes it, and sends it via the CAN bus.
3. **CAN Bus**: Transmits the sensor data to another node for real-time monitoring.
4. **Node 2 (Receiver)**: Could be another microcontroller or system that receives and processes the transmitted data for driver alerts.

## How It Works
1. The ultrasonic sensors trigger a pulse and wait for the echo to return.
2. The STM32 microcontroller calculates the distance using the time between the pulse and the echo.
3. The distance values are sent through the CAN bus in a specific message format.
4. Other nodes connected to the CAN bus can receive and utilize the distance information.
5. The system continues to monitor and transmit data in real-time.

## Code Overview
### Key Modules:
- **CAN2_Tx()**: Function to transmit front and rear distance data over CAN bus.
- **getDistance()**: Function to calculate the distance based on ultrasonic sensor data.
- **CAN_Filter_Config()**: Configures CAN filters for proper data reception.
- **SystemClock_Config()**: Configures the system clock.
- **MX_GPIO_Init()**: Initializes GPIO pins for ultrasonic sensor and CAN.
- **MX_TIM2_Init()**: Initializes the timer for time-sensitive operations like distance calculation.

## Future Enhancements
- Add more sensors for side detection.
- Implement advanced driver assistance features like automatic braking based on distance data.
- Improve error handling for lost CAN messages or corrupted data.





## For Receiving Data ==>>
receiving code is a FreeRTOS-based embedded system program designed to receive distance data (front and rear) over CAN bus, display it on an LCD, and control a buzzer based on the distance values. The system also includes task handling and interrupts for managing CAN messages, as well as LED indication using GPIO pins.

### Key Components:
1. **FreeRTOS Integration**:
   - The program utilizes FreeRTOS for task management, with tasks for deferred processing (`Deferred_Task`) and LED indication (`WasteFullLED`).
   - `vTaskStartScheduler()` starts the RTOS task scheduler.
   
2. **CAN Bus Configuration**:
   - The CAN bus (CAN2) is initialized using the HAL library. A filter is applied to receive messages of interest, and the CAN interrupt is activated to handle incoming data.
   - The `CAN2_RX0_IRQHandler` function is responsible for handling incoming CAN messages, extracting the front and rear distance values, and notifying the task for further processing.
   
3. **Distance Processing**:
   - The `Deferred_Task` processes the received distance data. It determines the minimum of the front and rear distances and controls the buzzer accordingly.
   - If the distance is less than 30 cm, the buzzer will beep faster based on the calculated delay, which varies inversely with the distance.

4. **LCD Display**:
   - The received front and rear distances are displayed on an LCD using the I2C interface.
   - The `lcd_send_string()` function is used to display the distances, with the LCD updated every 100 ms.

5. **Buzzer Control**:
   - The buzzer is controlled based on the `min_distance`. A smaller distance results in a faster buzzing rate to alert the user.

6. **Error Handling**:
   - The program contains an `Error_Handler` function, which is invoked whenever there is a failure in CAN initialization, message reception, or any other critical operation.

7. **Segger SystemView**:
   - The code is integrated with Segger SystemView for real-time debugging and system analysis. The program logs distance values and task states to help with performance monitoring.





