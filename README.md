# Portfolio C - Embedded Firmware Projects

This repository contains embedded firmware projects developed in C, demonstrating design patterns and architectures applied to embedded systems. Suggestions and contributions are always welcome.

## Objective

Demonstrate practical experience in embedded firmware development:
- Low-level drivers for peripherals
- Design patterns applied to embedded systems
- Decoupled and portable architectures
- Maintainable and scalable code

## Repository Structure

Each project is contained in its own directory as a Git submodule, allowing independent development and versioning.

## Projects

### 1. Multi-Driver System: TCA8418 Keypad + TCA9554 I/O Expander

**Directory:** [`hardware_proxy_pattern/`](./hardware_proxy_pattern)

System implementing Hardware Proxy pattern for managing multiple I2C peripherals on a shared bus. Includes driver for keypad matrix (TCA8418), I/O expander (TCA9554), keyboard manager with event queue, and BSP for ESP32-S3.

**Components:**
- **TCA8418 Driver:** Keypad scan controller (matrix up to 8x10, polling or interrupt detection, FIFO buffer 10 events)
- **TCA9554 Driver:** 8-bit I/O expander (individual pin control, input/output configuration, configurable polarity, multi-instance)
- **Keyboard Manager:** Asynchronous event management via FreeRTOS queue
- **BSP:** Hardware abstraction for portability (shared I2C, GPIO, interrupts)

**Technical features:**
- Hardware Proxy Pattern in both drivers
- Shared I2C bus (400 kHz Fast Mode)
- 4-layer architecture: Hardware → BSP → Driver → Manager/App
- FIFO queue for keyboard events
- Mutex synchronization (FreeRTOS)

**Use cases:** Industrial control panel, IoT devices with local interface, keypad access systems, GPIO expansion without MCU change.

---

### 2. Pressure Monitoring System - Observer Pattern

**Directory:** [`observer_pattern/`](./observer_pattern)

Monitoring system implementing Observer pattern to decouple sensor data acquisition from processing. Multiple observers can subscribe to pressure changes without knowing each other.

**Components:**
- **Pressure Sensor Driver:** Mock driver for testing without hardware
- **Pressure Manager:** Observer pattern Subject, manages observer list (up to 3), converts raw values to engineering units
- **Observers:** UI observer (updates screen), Alarm observer (monitors thresholds), extensible to logging, network, etc.

**Technical features:**
- Observer Pattern (Subject-Observer)
- Context injection for private data per observer
- Dynamic attach/detach of observers
- FreeRTOS task for periodic reading (100ms)
- Automatic broadcast notification

**Use cases:** Monitoring systems with multiple consumers, dashboard with multiple views, alarm system, data logging, remote transmission.

---

## Implemented Design Patterns

- [x] Hardware Proxy Pattern
- [x] Observer Pattern
- [ ] State Machine Pattern *(coming soon)*
- [ ] Factory Pattern *(coming soon)*

## Technology Stack

- **Language:** C (C99/C11)
- **Architecture:** ARM Cortex-M, ESP32-S3
- **RTOS:** FreeRTOS
- **Protocols:** I2C, UART, SPI
- **Tools:** GCC, Make, Git, ESP-IDF

## Usage

Each project includes its own README with:
- Detailed API documentation
- Code examples
- Architecture diagrams
- Usage instructions

**Download:**

For each repository:
```bash
git submodule init
git submodule update
```

Or clone the entire project with submodules:
```bash
git clone --recursive git@github.com:grivera90/portfolio_c.git
```

## Deployment

Uses [Stable Mainline](https://www.bitsnbites.eu/a-stable-mainline-branching-model-for-git/) as Git branching model.

## Future Updates

Upcoming projects:
- Communication protocols (MODBUS, CAN)
- State machines (FSM)
- Memory managers
- Drivers for sensors and actuators
- Additional RTOS examples

## Author

**Gonzalo Rivera**

- GitHub: [https://github.com/grivera90](https://github.com/grivera90)
- LinkedIn: [https://www.linkedin.com/in/gonzalo-rivera-7072a262/](https://www.linkedin.com/in/gonzalo-rivera-7072a262/)
- Email: gonzaloriveras90@gmail.com

## License

© 2025 grivera. All rights reserved.

---

⭐ If you find this repository useful, give it a star.