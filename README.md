# RTOS STM32 Task3
# LED Control Program with RTOS

This project demonstrates how to control multiple LEDs on a microcontroller using two independent tasks managed by a real-time operating system (RTOS). Each task handles specific LEDs and operates based on timing requirements using FreeRTOS.

## Program Overview

The program initializes two main tasks:
1. **greenTask1**: Controls the Green LED and toggles it for a set period before turning it off and pausing.
2. **redTask02**: Controls the Red LED with similar functionality and timing variations.

Both tasks run independently, managed by the RTOS scheduler, which ensures that each task can execute without interference based on their priority levels.

### Features
- Independent control of multiple LEDs using RTOS threads.
- Precise timing and frequency control for LED toggling.
- Efficient use of microcontroller GPIO resources with non-conflicting timing schedules.

## Task Descriptions

### greenTask1
- **Functionality**: Toggles the Green LED at 20 Hz for 4 seconds and then turns it off.
- **Timing**: Waits for 6 seconds before the next cycle, allowing ample time for other tasks to execute.
- **Priority**: Normal priority, ensuring it executes before redTask02 when scheduled by the RTOS.

### redTask02
- **Functionality**: Toggles the Red LED at 20 Hz for 0.5 seconds while turning on the Orange LED.
- **Timing**: Pauses for 1.5 seconds between cycles.
- **Priority**: Idle priority, ensuring it only runs when higher-priority tasks are idle.

## Getting Started

### Prerequisites
- STM32CubeIDE or any other compatible IDE for STM32 development.
- FreeRTOS configured on the STM32 microcontroller.

### Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/led-control-rtos.git
