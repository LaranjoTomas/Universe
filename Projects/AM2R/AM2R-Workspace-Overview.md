
## CAN

> [!info] Summary
> CAN bus resources and Arduino examples, including MCP2515/TJA1051 transceivers and J1939 support.

- j1939_decoder.dbc: DBC for J1939 message definitions.
- README.md: Overview/usage notes for CAN sketches.
- bikeDisplayFirmware/: Display firmware sketch consuming CAN data.
- examples/: Sender/receiver Arduino sketches for MCP2515 and TJA1051.
- jCOM-J1939-USB-C-Source-Code/: C sources for J1939 USB-C interface (`CANInterface.*`, `COM1939.*`, `config.h`, `main.c`).
- MCP2515_CANReceiver/, MCP2515_CANSender/: Focused MCP2515 receive/send sketches.
- TJA1051_CANReceiver/, TJA1051_CANSender/: Focused TJA1051 receive/send sketches.
- images/: Placeholder for diagrams/screenshots.

## Sketches

> [!info] Summary
> Main Arduino sketches and test suites for sensors, CAN, and services.

- BCycleIoTModule_v1.ino: Core IoT module sketch.
- backCombo/, bikeDisplayFirmware/: Additional Arduino sketches.
- CAN/: CAN-focused sketches (TWAIController, MCP2515 receiver/sender, etc.).
- esp/: ESP sketches (Gyroscope, lightmobie, mc60) and README.
- exampleMotor/: Motor control example.
- functionalTests/: CAN bandwidth logging/visualization, priority tests, latency scripts.
- heartRateSensor/, LiDAR/, lightSensor/, loadCellSensor/, motorVelocity/: Individual sensor drivers/tests.
- Services/: CSV datasets + plotting scripts and service sketches (heart rate, weight).
- ultraSonicSensor/, weightSensor/: Additional sensor sketches.

## esp

> [!info] Summary
> ESP-focused Arduino sketches and notes.

- Gyroscope.ino: IMU/Gyro handling on ESP.
- lightmobie.ino: Lighting-related sketch.
- mc60.ino: Cellular/GNSS module related sketch.
- README.md: ESP notes and setup.
- .gitkeep: Keeps folder tracked in VCS.

## rpi

> [!info] Summary
> Raspberry Pi code/assets for camera, LiDAR, CAN integration, ML services, and utilities.

- CAM/: Camera calibration data (NPZ/NumPy), COCO labels, TFLite models; Python scripts for capture, filtering, calibration, inference.
- LiDAR/: C++ sources for LiDAR drivers, processing, networking/serial interfaces.
- LiDAR training/: C++ evaluation and sample data folders (True/False).
- LiDAR_Can/: LiDAR via CAN integrations, tests/binaries, demo assets.
- pothole/: Python service for pothole detection with pre-trained models (.joblib) and training script.
- utility/: Helper scripts (JSON parsing, run-on-boot) + README.
- .gitkeep: Keeps folder structure tracked.

## pcbESP32

> [!info] Summary
> PCB design files for ESP32 boards, including EAGLE/KiCad and production CAM outputs.

- multiPCB/: EAGLE project (.brd/.sch/.epf), KiCad rules, backups.
  - CAMOutputs/: Assembly (PnP), drill, Gerber files for manufacturing.
- multiPCB_S3/: ESP32-S3 variant design files with backups and schematics/board layouts.
- README.md: Notes about the PCB designs.

## logicAnalyser

> [!info] Summary
> Logic analyzer setup/capture definitions for UART and CAN streams.

- hr&luxUART.dsl: Heart rate + lux over UART capture.
- hrCAN.dsl: Heart rate over CAN capture.
- lidarCAN.dsl: LiDAR over CAN capture.
- lidarUART.dsl: LiDAR over UART capture.
- luxCAN.dsl: Lux sensor over CAN capture.

## cargoBike

> [!info] Summary
> Cargo bike sensor sketches (load cell and ultrasonic).

- loadCellSensor/: Load cell Arduino sketch.
- sensors/: Load cell + ultrasonic sensor sketches.
