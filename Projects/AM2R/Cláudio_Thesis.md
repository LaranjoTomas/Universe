# Thesis Summary~ish

# Communication Approach for Bicycles Integrated in the Road

The core goal of this thesis was to design and evaluate a vehicular communication system solution for bicycles. This system connects bikes to the internet and other vehicles using a multi-technology approach (ITS-G5, 5G, and WiFi) to enhance Cycling Safety and the cycling experience. The system relies on Fuzzy Logic for intelligent data management.
## Introduction

- The work focuses on integrating e-bikes into the intelligent transportation ecosystem using **V2X (Vehicle-to-Everything)** communication.
- Motivation: cyclists are vulnerable road users (VRUs); improving their awareness and communication with vehicles and infrastructure can reduce accidents.
- Advances in IoT and smart infrastructure create opportunities for connected cycling systems.

## Main Contributions

- **Multi-technology communication** using **Wi-Fi, ITS-G5, and 5G** for reliable connectivity.
- **Fuzzy logic–based data management** for smarter decisions about data priority and transmission timing.
- **V2X services**:
    - Send **VAM (Vehicle Awareness Messages)** — inform nearby entities of bike position and movement.
    - Send **DENM (Decentralized Environmental Notification Messages)** — warn of hazards (e.g., potholes, obstacles, pedestrians).

## Background Concepts

- Covers ideas like **smart cities**, **micromobility**, and **vehicular networks**.
- Discusses the rise of e-bikes and their importance in “**15-minute city**” urban design — encouraging short, sustainable trips.
- Reviews current systems and highlights the need for better cyclist integration into intelligent transport networks.

**Communication Technologies:** The system explores both short-range and long-range wireless technologies
-  **Short-range:** ITS-G5 (low latency, high reliability, V2X communication, requires IEEE 802.11p)
-  **Long-range:** LTE and 5G (moderate latency, wider coverage, suitable for urban environments)
-  **Local:** Wi-Fi (high data throughput, low cost, suitable for bulk data transfers)
-  **Sensors:** The system uses common sensors such as GPS, Accelerometer/Gyroscope, and Networking technologies, which are vital for safety and monitoring
-  **Related Work Context:** Most previous solutions for connected cycling relied on user devices (smartphones)
-  The system developed here aims to integrate all necessary functionality directly onto the bike, eliminating the need for user input or personal devices.

The system utilizes common sensors such as GPS, Accelerometer/Gyroscope, and Networking technologies. The related work section concludes that while prior solutions often relied on user devices (smartphones) , this project aimed to integrate all necessary functionality directly onto the bike itself, eliminating the need for user input or personal devices.
## System Architecture

This chapter describes how the system was designed, based on the requirements for low latency, reliability, scalability, and adherence to standards.

• **System Components:** The architecture centers around a main device that acts as the "brain," connecting various sensors, communications services, and V2X services
• **Data Persistence Service:** This module stores sensor data in queues, preventing data loss
• It manages the pending queues and ensures confirmed delivery before discarding batches. Its decisions on data management and technology selection are based on fuzzy logic
• **Data Stream Service:** This service ensures key, sensitive data is sent immediately, bypassing queue delays (which can introduce latency)
• If immediate transmission fails, the data is rerouted to the Data Persistence service

- The system runs on a central onboard computer (Raspberry Pi).
- Equipped with sensors:
    - **GPS**, **accelerometer**, **gyroscope**, **light**, **heart rate**, **camera**, **LiDAR**.
- Communication handled by **Wi-Fi**, **ITS-G5**, and **5G**.
- Services:
    - **Data persistence** (collects and queues data).
    - **Data stream** (sends critical data immediately).
    - **V2X modules** for VAM/DENM.
    - **Network management** (chooses the best available network dynamically).

<img src="claudio_architecture_thesis.png" style="display: block; margin: auto;" />


## Data Management & V2X Services
### Information Aggregator

- Central **MQTT broker** connecting all modules via topic-based communication.
- Keeps modules decoupled and secure.
<img src="claudio_informationAggregator_thesis.png" style="display: block; margin: auto;" />
### Data Persistence

- Handles all sensor data with multiple strategies:
    - **Priority-based sending**
    - **TTL (time-to-live)**
    - **TTL + priority**
    - **Fuzzy logic** (best performing)
- Fuzzy logic dynamically decides when and how to send data based on urgency, network quality, and message lifetime.

<img src="claudio_dataPersistence_thesis.png" style="display: block; margin: auto;" />
### Network Management

- Continuously scans networks and adjusts routes to always use the **best connection**.
- Informs other modules of current connectivity type.
### Data Stream

- Sends sensitive or high-priority data instantly — bypasses queues for low latency.
### V2X Services

- **VRU Awareness (VAM)**: broadcasts presence, speed, heading of the bike to nearby vehicles.
- **VRU Clustering**: groups nearby VRUs into clusters to reduce redundant transmissions.
- **Event Detection + DENM**: detects hazards (from sensors/camera) and issues alerts in real time.
- Messages follow **ETSI standards** for interoperability.

## V2X Message Generators

- **VRU Basic Service** broadcasts the bike's presence (using VAMs). It features **clustering capabilities** to group nearby VRUs, allowing the leader to transmit for the whole group, thereby reducing the volume of messages. VRUs transition between states (IDLE, ACTIVE-STANDALONE, ACTIVE-CLUSTER-LEADER, PASSIVE) to manage cluster membership.
- **Event Detection and DENM Alert System** detects hazards using camera and sensor data, generating Decentralized Environmental Notification Messages (DENMs) in real-time.
- **Network Management Module:** Continuously evaluates accessible connections (Wi-Fi, ITS-G5, cellular) based on performance indicators (latency, signal strength) and dynamically adjusts routing settings to direct traffic through the optimal network interface.

## Chapter 5: Results

This chapter presents the analysis of tests conducted in both laboratory and real-world outdoor scenarios, using a hardware prototype equipped with a cellular modem and wireless card to enable 5G and ITS-G5. The test setup also included safety services, such as Rear 2D Lidar vehicle detection, Front camera detection (using YOLO/TF Lite), and Road surface anomaly detection (using SVM).

**Data Persistence Results (Fuzzy vs. Non-Fuzzy):**
- The **fuzzy logic approach significantly improved performance**. In laboratory tests, the fuzzy system reduced the average waiting time for a pending batch to **5.37 seconds**, compared to **15.64 seconds** for the non-fuzzy approach.
- The mean time for non-expired batches dropped to **4.95 seconds** using fuzzy logic.
- **Outdoor Tests** confirmed consistency; the fuzzy system maintained superior performance in managing pending batches compared to the non-fuzzy system.
- **Data Stream Tests:** The service demonstrated reliable and low-latency transmission for sensitive data. The mean transmission time for 100 messages was approximately **15.85 ms**.
- **V2X Systems Tests:** The system demonstrated timely reaction capabilities.
- **VAM Messages:** Mean generation time was very fast (around **0.507 ms**).
- **DENM Messages:** Mean generation time was **0.845 ms**, showing consistent, low-variability performance vital for hazard alerting.

## Chapter 6: Conclusion and Future Work

The **conclusion** states that the work successfully created a robust, multi-technology communication framework for cycling safety. The key success factor was the implementation of Fuzzy Logic, which provided a sophisticated and responsive strategy for prioritizing and scheduling data transfers, significantly reducing waiting times compared to traditional binary systems.

**Data Management Enhancement:** Improving the fuzzy logic system through the integration of **Machine Learning algorithms** or other adaptive control systems.
**V2X Expansion:** Increasing shared information by including messages like **Collective Perception Messages (CPMs)** to boost situational awareness in dense urban areas.
**Predictive Systems**: Integrating capabilities to analyze historical data and current trends to forecast hazards and traffic congestion, allowing for proactive warnings and route optimization.