# Thesis Summary~ish
# An In-Network Architecture for Smart 2-Wheelers Based on CANBus

The core goal of this thesis was to propose and validate a distributed sensor network architecture for smart two-wheeled vehicles, such as e-bikes, based on the **Controller Area Network (CAN) Bus** protocol. The primary objective is to enhance cyclist safety by adapting robust, automotive-grade communication technology to the micro-mobility sector. The system was designed to overcome the limitations of previous UART-based systems, offering superior noise immunity, longer cable runs, and hardware-level message prioritization.
## Introduction

- The work is motivated by the increasing popularity of e-bikes and other electrified two-wheeled vehicles in urban areas, which, despite their benefits, are more prone to accidents than automobiles with their integrated safety features.
- The project aims to address this safety gap by extending the safety paradigm of modern automotive platforms to micro-mobility.
- The system is composed of distributed processing nodes interconnected via a custom **CAN 2.0A** implementation.
## Main Contributions

- **CAN Bus Architecture:** Design and implementation of a distributed sensor network architecture for smart two-wheelers utilizing the CAN Bus protocol.
- **Custom CAN 2.0A Implementation:** Development of a custom 11-bit identifier structure to encode message **priority**, **type**, and **source zone**, enabling efficient message arbitration.
- **Sensor Integration:** Integration of multiple sensors, including **LiDAR** for rear vehicle detection, weight sensors for cargo monitoring, heart rate sensors, and cameras for object detection.
- **Intelligent Services:** Development of two intelligent services that adapt the level of motor assistance based on collected sensor data.
- **Validation:** Demonstration of clear advantages over previous UART-based systems in a real-world scenario within the Aveiro Tech City Living Lab (ATCLL).
## System Architecture

The system is fundamentally organized into two primary units: the **Sensing Unit** and the **Communications Unit**, interconnected by the CAN Bus.
- **Sensing Unit:** Designed for flexibility and modularity, it comprises distributed nodes:
    - **Rear Module:** Includes LiDAR for rear vehicle detection.
    - **Cluster Module:** Includes sensors like heart rate and environmental sensors.
    - **Motor Module:** Manages the motor assistance services.
- **Communications Unit:** Includes the **Central Processing Module** (Raspberry Pi) which listens to the CAN bus traffic for data processing, transmission (e.g., V2X), or actuation tasks.
- **CAN Bus Interconnection:** The CAN bus allows the Communications Unit to access all necessary data without explicit queries, reducing overhead and latency.
## Testing and Evaluation

The evaluation compared the new CAN-based system with the legacy UART-based system and tested the core features of the CAN protocol.

| Feature               | UART-based System                  | CAN-based System                             | Advantage                                    |
| :-------------------- | :--------------------------------- | :------------------------------------------- | :------------------------------------------- |
| **Architecture**      | Point-to-point, non-distributed    | Distributed, multi-master                    | Scalability, Modularity                      |
| **Noise Immunity**    | Low                                | Superior                                     | Reliability in harsh environments            |
| **Cable Length**      | Limited (e.g., 15m at 9.6 Kbps)    | Long (e.g., 40m at 1 Mbps)                   | Flexibility in vehicle design                |
| **Message Priority**  | Software-level                     | Hardware-level (Arbitration)                 | Guaranteed low latency for critical messages |
| **Latency (Example)** | High (e.g., 40ms for some sensors) | Low (e.g., 1.2ms for high-priority messages) | Real-time safety response                    |

- **Priority Testing:** The custom 11-bit ID structure successfully demonstrated hardware-level message prioritization, ensuring that high-priority messages (e.g., from the LiDAR) were transmitted with minimal latency even under bus saturation.
- **Real-World Validation:** The system's effectiveness and scalability were demonstrated in the Aveiro Tech City Living Lab, confirming that CAN Bus technology is a robust foundation for intelligent bicycle systems.
## Conclusion and Future Work

The **conclusion** affirms that the work successfully created a robust, distributed, and scalable communication framework for smart two-wheelers by adopting the CAN Bus protocol. The system demonstrated clear technical superiority over previous UART-based solutions, particularly in terms of reliability, noise immunity, and guaranteed message delivery for critical safety functions.
**Future Work** includes:
- **CAN-FD Implementation:** Transitioning to CAN-FD (Flexible Data-rate) to increase the data payload size and overall throughput.
- **Integration with V2X:** Integrating the CAN-based system with V2X communication technologies (e.g., ITS-G5) to enable the exchange of safety messages with other road users.
- **Standardization:** Further work on standardizing the CAN message structure for micro-mobility to promote interoperability across different manufacturers.
## Controller Area Network (CAN) in the Thesis

The **Controller Area Network (CAN)** Bus is the foundational technology of Tiago Rodrigues' thesis, serving as the robust, in-network communication backbone for the smart two-wheeler architecture. The thesis leverages CAN's key features, which were originally designed for the harsh, real-time environment of the automotive industry, to address the safety and reliability challenges of e-bikes.
### Why CAN?
The primary motivation for choosing CAN over the legacy **UART (Universal Asynchronous Receiver-Transmitter)** system was to achieve a truly **distributed, multi-master architecture**. Unlike UART's point-to-point nature, CAN allows all nodes to communicate simultaneously on a single bus. This is crucial for a system with multiple independent sensors and processing units.
The key advantages of CAN utilized in this work are:
1.  **Hardware-Level Message Prioritization:** CAN uses a non-destructive bitwise arbitration mechanism. The lower the numerical value of the message identifier (ID), the higher its priority. This is a critical feature for safety systems, as it guarantees that a high-priority message (e.g., a rear-collision warning from the LiDAR) will always gain immediate access to the bus, even when the bus is saturated.
2.  **Superior Noise Immunity:** CAN uses a differential signaling pair (CAN High and CAN Low), which makes it highly resistant to electromagnetic interference and noise. This is essential for a vehicle operating in an electrically noisy environment (e.g., near the motor or in urban settings).
3.  **Robustness and Error Handling:** The protocol includes built-in error detection and fault confinement mechanisms, ensuring data integrity and preventing a single faulty node from disrupting the entire network.
4.  **Scalability:** The bus structure allows for easy addition or removal of nodes without requiring changes to the existing network or software on other nodes.
### Custom CAN 2.0A Implementation
The thesis specifically implements the **CAN 2.0A** standard, which uses an **11-bit identifier**. To maximize the utility of this limited ID space, a custom structure was defined to encode three critical pieces of information within the 11 bits:

| Bit Range | Field | Purpose |
| :--- | :--- | :--- |
| **Bits 10-8** | **Priority** | Defines the message urgency (e.g., 000 for highest priority). This is the most significant field for bus arbitration. |
| **Bits 7-4** | **Message Type** | Identifies the content of the message (e.g., LiDAR data, Heart Rate, Motor Status). |
| **Bits 3-0** | **Source Zone** | Identifies the physical location of the sending node (e.g., Rear Module, Central Module). |

This custom structure ensures that the most critical safety messages are assigned the lowest numerical ID, thereby winning arbitration and achieving the lowest possible latency, a feature that was successfully validated in the testing phase.