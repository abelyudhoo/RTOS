# Embedded RTOS Project

This project leverages a Real-Time Operating System (RTOS) to manage and synchronize multiple tasks on an embedded platform. The primary functions include reading and processing analog sensor data, controlling an LED based on button input, and communicating status information over UART.

## Table of Contents

- [Overview](#overview)
- [System Components](#system-components)
- [Tasks](#tasks)
  - [StartDefaultTask](#startdefaulttask)
  - [Startledtask](#startledtask)
  - [Startbutontask](#startbutontask)
  - [StartTask04](#starttask04)
- [Inter-Task Communication](#inter-task-communication)
- [Summary](#summary)

## Overview

This project is designed to run on an RTOS platform, with four main tasks:
1. **StartDefaultTask**: Reads ADC values from a sensor and stores them in a global variable.
2. **Startledtask**: Uses the ADC values to calculate a voltage, then sends the voltage data via UART.
3. **Startbutontask**: Monitors a button and toggles an LED status based on button presses.
4. **StartTask04**: Controls the physical LED and sends the LED state via UART.

The tasks communicate with each other using shared global variables, ensuring coordinated and responsive operation.

## System Components

- **ADC (Analog-to-Digital Converter)**: Reads sensor data which is then processed.
- **UART (Universal Asynchronous Receiver-Transmitter)**: Sends data to an external system for monitoring.
- **GPIO (General Purpose Input/Output)**: Used for reading button input and controlling LED output.
- **RTOS**: Manages task scheduling and execution.

## Tasks

### StartDefaultTask

**Purpose**: This task reads an analog value from an ADC channel and updates a shared global variable `adc_value` with the most recent reading.

- **Operation Details**:
  - Starts ADC conversion and waits for it to complete.
  - On successful conversion, retrieves the ADC result and stores it in `adc_value`.
  - Loops with a 500 ms delay to continuously provide updated sensor data.
  
- **Dependencies**:
  - Provides data used by `Startledtask` for real-time voltage calculation.

- **RTOS Priority**: Runs at a normal priority, ensuring it doesn't interrupt more time-sensitive tasks.

### Startledtask

**Purpose**: This task reads the `adc_value` set by `StartDefaultTask`, calculates the corresponding voltage, and sends this information via UART.

- **Operation Details**:
  - Converts `adc_value` to a voltage using a scaling factor (e.g., 3.3V reference divided by the ADC resolution).
  - Formats the voltage and ADC value into a string message.
  - Sends the message over UART.
  - Repeats every 1000 ms, providing continuous data updates.

- **Dependencies**:
  - Requires `adc_value` from `StartDefaultTask` to ensure the voltage is current and accurate.

- **RTOS Priority**: Runs at a normal priority, spaced apart from sensor reading to avoid potential conflicts.

### Startbutontask

**Purpose**: This task monitors a button connected to a GPIO pin, debounces the input, and toggles an `led_status` variable based on button presses.

- **Operation Details**:
  - Reads the GPIO state associated with the button and uses a simple debounce logic with a delay of 200 ms.
  - Detects state transitions from high to low (indicating a button press).
  - Toggles `led_status`, which is used by `StartTask04` to control the LED.
  - Runs every 500 ms to ensure quick responsiveness without excessive polling.

- **Dependencies**:
  - Sets `led_status` which is read by `StartTask04` to manage the LED state.

- **RTOS Priority**: Also runs at a normal priority, as it is a responsive but not critical task in the system.

### StartTask04

**Purpose**: This task reads the `led_status` variable and sets the actual LED GPIO accordingly. Additionally, it sends the LED state over UART.

- **Operation Details**:
  - Checks the value of `led_status` to determine the LED state.
  - Controls the LED using GPIO based on `led_status`.
  - Sends a UART message indicating whether the LED is on or off.
  - Runs every 500 ms, frequently checking and updating the LED state.

- **Dependencies**:
  - Depends on `led_status` which is set by `Startbutontask` based on user interaction.

- **RTOS Priority**: Runs at a normal priority, coordinated with other tasks to avoid resource contention.

## Inter-Task Communication

The tasks interact through shared global variables:

- **adc_value**: Updated by `StartDefaultTask`, accessed by `Startledtask`.
- **led_status**: Updated by `Startbutontask`, accessed by `StartTask04`.

This shared variable setup allows tasks to be loosely coupled, simplifying communication without needing complex inter-task messaging mechanisms like queues or semaphores.

## Summary

This project implements a multi-tasking system where sensor data is continuously collected, processed, and transmitted while an LED is controlled in response to user input. The RTOS provides task scheduling, ensuring efficient use of CPU time, while shared global variables facilitate straightforward data sharing between tasks. The modular task-based design allows for easy expansion or modification to accommodate additional features or peripherals.

---

Save this file as `README.md` in the root directory of your GitHub repository. This comprehensive README provides clear documentation on the system components, individual task functionality, and their interdependencies, which should help others understand and contribute to the project.
4. **StartTask04**: Controls the LED and sends its status via UART.

## Tasks and Interactions

### StartDefaultTask
- **Purpose**: Reads an analog sensor value using ADC.
- **Operation**: Continuously polls the ADC in a loop and updates the global variable `adc_value`.
- **Interaction**: 
  - Supplies `adc_value` used by `Startledtask` for voltage calculations.
  - Runs every 500 ms, ensuring the other tasks have access to up-to-date sensor data.

### Startledtask
- **Purpose**: Converts `adc_value` to voltage and sends it over UART.
- **Operation**: Calculates voltage from `adc_value` and formats a message with the ADC value and voltage. Sends the message via UART.
- **Interaction**: 
  - Relies on `StartDefaultTask` for updated ADC data.
  - Runs every 1000 ms, providing consistent sensor data reporting over UART.

### Startbutontask
- **Purpose**: Detects button presses and toggles `led_status`.
- **Operation**: Reads button state using GPIO, applies debouncing, and toggles the `led_status` variable when a press is detected.
- **Interaction**: 
  - Modifies `led_status`, which `StartTask04` uses to control the LED.
  - Runs every 500 ms, allowing a responsive user interface.

### StartTask04
- **Purpose**: Controls the LED based on `led_status` and communicates LED state via UART.
- **Operation**: Checks `led_status` to toggle the LED on/off and sends the LED state over UART.
- **Interaction**: 
  - Depends on `led_status` updated by `Startbutontask`.
  - Runs every 500 ms, ensuring timely updates to the LED state and status reporting.

## Task Interactions Summary

- `StartDefaultTask` updates `adc_value` for `Startledtask`, which reports sensor data over UART.
- `Startbutontask` changes `led_status` in response to button presses, indirectly controlling `StartTask04`.
- `StartTask04` checks `led_status` to manage the LED and sends the current status via UART.

These tasks work together to create a reactive system where sensor readings are processed, user inputs control the LED, and system status is reported both visually (via the LED) and over UART.
