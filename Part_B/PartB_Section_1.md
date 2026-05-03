# Section 1: Characteristics of the Production System

The production portfolio operates as a decentralized manufacturing cell. To ensure the successful assembly of the field nodes, the system is characterized by the following operational metrics and design principles:

**1. System Architecture: Pull System and CONWIP**

* **Inventory Management (Pull System):** The facility operates a strict Pull system, sorting incoming stock into two distinct assembly lines (Standard vs. Anchor) based on exact deployment demand. By utilizing a serialized tracking system, the facility prevents overproduction and manages limited component inventory efficiently.
* **Constant Work-in-Progress (CONWIP):** To prevent bottlenecks between the soldering stations and the 3D printing/potting zones, the system limits the number of active units on the bench at any given time, maintaining a smooth operational flow.

**2. Operations Performance Metrics**

* **Error Rate (Pre- and Post-Assembly Validation):** Error rates are aggressively minimized at two stages. First, Stock Quality is verified pre-assembly using Raspberry Pi testing rigs to check raw sensor and ESP32 I/O integrity. Second, Technical Performance Validation is conducted post-assembly using digital multimeters to confirm pin connections and solder joint reliability.
* **Efficiency and Cycle Time (Component Integration):** Efficiency is optimized at dedicated soldering stations where the general assembly of the ESP32 and LoRa modules occurs. Cycle times for individual sub-assemblies are tracked to ensure steady throughput.
* **Productivity / Throughput (Firmware & Network Verification):** Throughput is maintained by having dedicated computer workstations flash the ESP32 firmware. Productivity is verified by conducting live network testing—ensuring data successfully reaches the test site before the unit leaves the facility.
* **Cost Effectiveness & Turnaround Time (Final Assembly & Hardening):** Turnaround time is finalizing by efficiently executing environmental hardening (epoxy potting of capacitive sensors) and utilizing 3D-printed PLA inserts for structural support. This allows for rapid final integration into the enclosure, resulting in a cost-effective, deployment-ready product.                                                                                                                                                                                                                           


