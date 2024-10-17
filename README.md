# RTOS STM32 Task3 
# LED Control Program with RTOS

This project demonstrates multi-tasking LED control on an STM32 microcontroller, utilizing FreeRTOS for task scheduling. The program includes two independent tasks, each responsible for controlling different LEDs with specific timing and frequency requirements.

## Program Overview

The program is designed with two main tasks:
1. **greenTask1**: Controls a Green LED with a specific blink pattern and delay period.
2. **redTask02**: Controls a Red LED with a different pattern, alongside an Orange LED.

The tasks are managed using RTOS, which ensures that each task executes as per its priority and timing requirements, ensuring precise control and synchronization.

### Task Descriptions

#### greenTask1
- **Functionality**: 
  - Toggles the Green LED at 20 Hz (every 50 ms) for 4 seconds.
  - Turns off both Green and Blue LEDs after the toggling cycle.
  - Pauses for 6 seconds before the next cycle.
- **Priority**: Normal Priority, allowing it to run over redTask02 when both are ready.
- **Purpose**: Provides a longer blink pattern with a pause, allowing time for other tasks to execute during its idle period.

#### redTask02
- **Functionality**:
  - Activates an Orange LED and toggles a Red LED at 20 Hz (50 ms intervals) for 0.5 seconds.
  - Turns off both Red and Orange LEDs after the toggling cycle.
  - Pauses for 1.5 seconds before the next cycle.
- **Priority**: Idle Priority, meaning it only runs when greenTask1 is inactive.
- **Purpose**: Provides a quick blink pattern that fits within the idle time of greenTask1, ensuring efficient use of GPIO resources.

### Task Coordination

The program uses RTOS to handle task execution, enabling precise timing and non-overlapping LED control:
- **Non-Interference**: The Normal and Idle priorities prevent GPIO conflicts, allowing each task to control its LEDs independently.
- **Timing and Synchronization**: Each task’s delay aligns with the other’s active time, ensuring consistent and predictable LED patterns without overlapping resource usage.
