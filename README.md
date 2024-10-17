# RTOS STM32 Task2

This project utilizes an RTOS (Real-Time Operating System) to manage multiple tasks running concurrently on an embedded system. The main functionality includes reading sensor data, controlling an LED based on button input, and communicating system status over UART.

## Table of Contents

- [Overview](#overview)
- [Tasks and Interactions](#tasks-and-interactions)
  - [StartDefaultTask](#startdefaulttask)
  - [Startledtask](#startledtask)
  - [Startbutontask](#startbutontask)
  - [StartTask04](#starttask04)
- [Task Interactions Summary](#task-interactions-summary)

## Overview

The program contains four primary tasks:
1. **StartDefaultTask**: Reads sensor data from an ADC.
2. **Startledtask**: Calculates voltage from the sensor data and sends it via UART.
3. **Startbutontask**: Monitors a button and toggles LED status.
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

---

Save this content as `README.md` in the project’s root directory on GitHub. This structure will make it easy for others to understand the purpose and interactions of each task in your project.
