When designing an IoT device, these have to be kept in mind:

- **Form Factor:** Processor cannot be too big, IoT devices are meant to be fairly small
- **Power Optimization:** Very important as IoT devices need to last long.
- **Memory Capacity:** Important.

#### Microcontroller Unit (MCU)

> Processor + Memory + I/O Devices are printed on the motherboard.

System on a chip (`SoC`) architecture makes the microcontroller very small and lightweight.

> **Microprocessor vs Microcontroller:**
> - Microcontrollers are designed to control specific operations in embedded systems, while microprocessors are the critical unit of a computer system that performs arithmetic and logic operations.
> - Microcontrollers are cheaper and easier to develop than microprocessors, but microprocessors have more processing power and memory

**Why Use a Microcontroller?** Cost-efficient, power-efficient, compact, and suitable for real-time tasks.

**Why Not Use a Microprocessor?** Higher power consumption, external dependencies, overkill for simple tasks.

**When to Use a Microprocessor:** Complex tasks requiring high computational power or OS support.

**Case Studies:**

- **ESP32:** Low-cost microcontroller with Wi-Fi/Bluetooth, using FreeRTOS.
- **Arduino:** Open-source platform with boards like Arduino Uno; supports bare-metal programming and RTOS.
- **Particle Photon:** Wi-Fi-enabled microcontroller with built-in cloud support.

**SRAM vs. Flash Memory on ESP32:**

- **SRAM:** Temporary storage for data and variables; volatile but fast.
- **Flash:** Permanent storage for code and static data; non-volatile but slower.
- **Why Both Are Needed:** SRAM ensures fast real-time operations, while Flash provides reliable code storage.


