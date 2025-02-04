**Lecture Notes 1: Introduction to IoT Systems, Microcontrollers, and Compilation Process**

### **1. Introduction to IoT Using Case Studies**

**What is an IoT System?**  
A network of interconnected "things" that **sense**, **process**, **act**, and **communicate** to achieve shared objectives.

**IoT Network Architectures:**

- **Peer-to-Peer:** Direct device communication.
- **Star Network:** Devices connect to a central hub.
- **Hierarchical Network (Cloud-Fog-Edge):** Cloud-based analytics combined with localized processing.

**Case Studies:**

- **Ride-Hailing Services (Uber/Ola):** GPS, accelerometers, cellular communication, cloud-based pricing and route optimization, edge-based location updates.
- **Smart Road Traffic Monitoring:** Traffic cameras, air quality sensors (PM2.5, NOx), LoRaWAN, cellular/Wi-Fi communication, edge-based object detection, cloud-based analysis.
- **Smart Water Meters:** Flow sensors, NB-IoT/LoRaWAN communication, edge-based anomaly detection, cloud-based billing and trends.
- **Sports Vests:** Heart rate monitors, GPS, accelerometers, Bluetooth/Wi-Fi communication, edge-based tracking, cloud-based performance analysis.

**Comparative Analysis:** Case studies are analyzed based on sensors, communication, edge/cloud processing, and key design considerations.

**IoT Design Considerations:**

- **Components:** Sensors/actuators, edge vs. cloud processing, communication networks, security, software stack.
- **Parameters:** Processing capability, communication requirements, real-time needs, power efficiency, storage.

**Key Takeaways:** IoT applications vary widely. Effective design balances efficiency, scalability, and privacy, with architecture tailored to system requirements.

---

### **2. Microcontrollers**

**Microcontroller (MCU) vs. Microprocessor (MPU):**

- **MCU:** Self-contained chip with CPU, memory, and peripherals for specific tasks; cost-effective and compact.
- **MPU:** CPU reliant on external components, suited for general-purpose computing.

**Why Use a Microcontroller?** Cost-efficient, power-efficient, compact, and suitable for real-time tasks.

**Why Not Use a Microprocessor?** Higher power consumption, external dependencies, overkill for simple tasks.

**When to Use a Microprocessor:** Complex tasks requiring high computational power or OS support.

**Introduction to Microcontrollers:** Integrated circuits for specific tasks, combining a processor, memory, and I/O peripherals.

**Case Studies:**

- **ESP32:** Low-cost microcontroller with Wi-Fi/Bluetooth, using FreeRTOS.
- **Arduino:** Open-source platform with boards like Arduino Uno; supports bare-metal programming and RTOS.
- **Particle Photon:** Wi-Fi-enabled microcontroller with built-in cloud support.

**Comparison:** ESP32, Arduino, and Photon are compared based on processor, voltage, memory, connectivity, GPIO, cloud support, and ease of use.

**SRAM vs. Flash Memory on ESP32:**

- **SRAM:** Temporary storage for data and variables; volatile but fast.
- **Flash:** Permanent storage for code and static data; non-volatile but slower.
- **Why Both Are Needed:** SRAM ensures fast real-time operations, while Flash provides reliable code storage.

---

### **3. Compilation Flow: From Sketch to Board**

**Steps in Compilation:**

1. **Writing the Sketch:** Program in Arduino IDE with `setup()` and `loop()`.
2. **Preprocessing:** IDE adds includes, combines files, and processes macros.
3. **Compilation:** Converts the sketch into machine code via GCC toolchain.
4. **Generating the Binary:** Produces a binary file.
5. **Uploading to the Board:** Binary is uploaded via a serial connection.
6. **Program Execution:** Microcontroller runs bootloader, then `setup()`, followed by `loop()`.

**Native vs. Cross-Compilation:**

- **Native:** Compilation on the same platform as execution.
- **Cross:** Compilation on one platform for execution on another.
- **Arduino Relevance:** Arduino development uses cross-compilation.

---

### **4. FreeRTOS in Compiled ESP32 Binaries**

**Introduction:** FreeRTOS provides multitasking for the ESP32 and is included in its SDK.

**Why FreeRTOS Is Included:** Offers task scheduling, timers, and inter-task communication.

**Binary Components:** Application code, FreeRTOS kernel, hardware abstraction layer, ESP32-specific libraries, external libraries.

**Bare-Metal Programming vs. FreeRTOS:** Bare-metal requires manual resource management, while FreeRTOS simplifies multitasking and scheduling.

**Confirmation of FreeRTOS:** Via build logs or runtime task lists.

**Advantages of FreeRTOS:** Multitasking, real-time scheduling, resource management, and system-level features.

---

### **5. PCBs**

**Why Move to PCB Design?** Reliability, compactness, signal integrity, mass production, power/heat management, custom enclosures, long-term reliability.

**ESP32-WROOM:** A pre-manufactured PCB module with the ESP32 microcontroller.

**Why Design a Custom PCB for ESP32-WROOM?**

- Integration of custom circuits.
- Optimized connectors and pins.
- Improved power management.
- Better mechanical fit.

**PCB-on-PCB:** ESP32-WROOM can be mounted on custom PCBs to simplify development.

---

### **6. SoCs vs. MCUs**

**What Is an SoC?** A chip integrating all components for a specific application.

**Microcontroller Characteristics:** Combines CPU, RAM, flash memory, and peripherals on one chip.

**Why Microcontrollers Qualify as SoCs:** Microcontrollers are a basic form of SoCs, optimized for simpler tasks.

**Key Differences:**

- **Microcontrollers:** Simpler, task-specific.
- **SoCs:** More powerful, feature-rich.

**Examples:** ESP32, STM32 (MCUs/SoCs), Raspberry Pi 4 (traditional SoC).

**Conclusion:** Microcontrollers are simplified SoCs, while SoCs provide advanced functionality.