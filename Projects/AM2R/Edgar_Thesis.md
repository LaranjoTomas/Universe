# MSc Thesis – Edgar Sousa (2024)
# Sensing integrated in Bicycles, Including People and Cargo Bikes

The core goal of this thesis was to develop a solution applicable to Smart Bicycles that significantly improves Cycling Safety and contributes to Cooperative Perception services in Smart Cities. To achieve this, the work proposed and implemented a set of integrated ADAS (Advanced Driver Assistance System) services.

## Introduction

The work is motivated by the increasing global demand for healthier and more sustainable urban transport solutions, making e-bikes popular. The author notes that despite these advantages, **safety concerns remain high**. The project focuses on the design and implementation of an IoT integrated ADAS system for two-wheelers.

The system goals required the solution to be scalable, low-cost, non-interfering with bike ergonomics, and optimized for low energy and computational resource usage, while ensuring resistance to physical strain and outdoor elements.

**Main Contributions:**
*   Definition and development of a two-wheeler ADAS system integrating various sensors, including Lidar, IMU, GPS, heart rate, and camera.
*   Development of a **rear-vehicle detection** system based on Lidar and Machine Learning for Cooperative Perception and integration with ITS-G5 V2X scenarios.
*   Analysis and comparison of computer vision algorithms using a Fisheye Camera for VRU Detection (pedestrians and vehicles).
*   Development of an IMU-based **road quality assessment** system using supervised machine learning to identify Road Surface Anomalies (e.g., holes). This data contributes to the Aveiro Living Lab platform.

## State of the Art

The chapeter discusses Modern 2-Wheeler Infrastructure, noting that cycle tracks are the safest option as they are physically separated from motor vehicle traffic. A Smart Bicycle is defined generally as an e-bike integrating sensing, processing, and wireless. Historic studies on instrumented bikes show that **GPS** and **forward cameras** are the most common sensors utilized for data collection.

The thesis groups smart bicycle developments by their level of assistance — from passive (data collection) to active (ADAS with V2X). Most existing solutions still focus on low-level sensing or post-crash alerts, showing a lack of integrated real-time ADAS for two-wheelers.
*   **Warning Assistance** systems (collision avoidance) are reviewed. Different sensor technologies are assessed, including Sonar, Radar, Optical, and Lidar. In academic applications, **low-density Lidars** are found to be popular.
*   **Environmental Monitoring** systems are examined, which collect real-time data on environment or path quality. For road quality assessment, IMU-based or computer vision approaches are common.

## Architecture and Services

This chapter specifies the hardware and the four core services developed.

*   **General Architecture:** The system uses a local **MQTT Broker** to coordinate data transfer between decoupled services.
*   **Hardware Setup:** The architecture relies on two main computational units: the **Raspberry Pi 5 (RPI5)** acts as the main processing unit for complex algorithms (vision, Lidar analysis), while a proprietary **ESP-32-based PCB** (Bicycle Module) serves as a low-power, supporting unit for sensor data collection. They communicate via UART.
*   **Sensors:** Include GPS, IMU (gyroscope, accelerometer), luminosity (VEML7700), heart rate (MAX30102), a Fisheye Camera, and a low-density Lidar (LD-06).

**Implemented Services:**
1.  **Sensor Data Collection:** This service runs on the ESP-32 to offload time-sensitive sensor interactions, ensuring minimal latency and allowing the RPI5 to focus on complex tasks.
2.  **Collision Avoidance (Lidar):** This rear vehicle detection algorithm focuses on **front-bumper identification**. It uses clustering and filtering techniques based on three dynamically tuned hyperparameters: Alpha (cluster radius), Attenuation (radius increase per meter), and Min Points (minimum points required for cluster identification).
3.  **Computer Vision:** Uses a Fisheye Camera mounted at the front to detect VRUs and vehicles in reserved or mixed lanes. To maintain a wide FoV while allowing model compatibility, the system uses OpenCV to apply a **non-distortion matrix** to captured frames before feeding them to deep learning models like MobileNet V2 SSD or YOLO V8.
4.  **Pothole Detection (IMU):** This service analyzes accelerometer and gyroscope data to detect Road Surface Anomalies. It uses a **Support Vector Machine (SVM)** classifier, trained using supervised learning on extracted features (Max, Min, Speed, etc.) from segmented data.

## Tests and Evaluation

This chapter validates the prototype and algorithm performance using collected datasets. The prototype was housed in a resilient enclosure, solving overheating issues with an active cooler .

*   **Lidar Collision Avoidance Results:** Hyperparameter tuning resulted in an optimal algorithm for rear vehicle detection, achieving a classification **F1-score of 0.984** (Precision 0.97, Recall 0.998). The average frame latency was **44.87 ms**, remaining below the 100 ms threshold for the chosen Lidar. However, execution time showed variability (spikes) due to the non-RTOS nature of the RPI5.
*   **VRU Detection Results:**
    *   The **YOLO V8** model showed higher accuracy for vehicles (car precision 0.902, bicycle precision 0.656), but was computationally demanding, resulting in a very high average latency of **915.9 ms**, rendering it unsuitable for real-time application on the chosen hardware.
    *   The **MobileNet V2 SSD** model was more efficient but performed poorly in classifying cars and bicycles, though it achieved similar pedestrian precision (0.85). The model is deemed applicable only for the pedestrian detection scenario due to hardware constraints.
*   **Road Quality Results:** The SVM model was simplified to classify two classes: smooth and non-smooth. It achieved a strong **F1-score of 0.75 for identifying potholes**. A binary classification (smooth/non-smooth) raised overall accuracy to 0.84 but reduced the critical ability to distinguish anomaly types.
*   **Power Consumption:** The **Lidar service** had the highest power consumption (over 4.25 W/h). Overall, the power consumption of all services was relatively low and negligible compared to the e-bike motor (250W), leaving capacity for inclusion of more powerful processing units in the future.

## Conclusion and Future Work

The **conclusion** affirms that the integrated two-wheeler ADAS system provides a capable foundation for increasing user awareness and contributing to cooperative perception services. The successful implementation of the Lidar service (0.98 F1-score) and the Pothole detection algorithm (0.75 F1-score for potholes) were highlighted as key achievements.

**Future Work** includes:
*   Implementing **sensor fusion** by adding a vehicle tracking component to the Lidar system and corroborating it with the computer vision service.
*   Reducing the complexity of the Lidar algorithm to address execution time spikes.
*   Improving the Pothole detection service by addressing hardware placement and refining the supervised labeling process.
*   Integrating with Smart City infrastructure and adding **vehicular messaging (V2X)** using the multi-technology communication setup (Wi-Fi, 5G, ITS-G5) to exchange information.
*   Conducting studies on Human-Computer Interaction to determine the best way to convey warnings and recommendations to the cyclist.
