# Raspberry Pi - AM2R Unit

**Role:** Central Sensor Fusion & V2X Gateway  
**Hostname:** `NAP-770`

> [!INFO] Credentials
> **User:** `nap`
> **Pass:** `openlab`
> **Local IP:** `10.0.22.186`

## Logging in

### 1. Local Network (Lab/Home)
```bash
ssh nap@10.0.22.186
# Enter password: openlab
```
### 2. Remote Access (Tailscale)

```bash
sudo tailscale up

ssh nap@10.0.22.186

#password: openlab
```

## Environment

- **mediamtx:** This is a high-performance Real-Time Media Server.
    - **Purpose:** Likely for streaming the **Rear Camera** or **LiDAR visualization** to your laptop/phone with low latency.
- **can_rcv.py:** A Python script for the CAN Bus.
    - **Purpose:** This is your "Sniffer." It listens to the wires and prints out what the sensors are saying.
- **Wiring_Pi / bcm2835:** Low-level C libraries.
    - **Purpose:** Used to control the GPIO pins directly (blinking LEDs, reading digital signals) at high speed.
- **...**

## Thing to try

### Run can_rcv.py
**Result:** Output is a Dictionary in python of the sensors names with the corresponding IDs. Each sensor has a different priority 
```python
CAN IDS {'weight': 352079698, 'ultrasonic': 284838243, 'lidar': 150921297, 'camera': 217060194, 'gyro': 285141093, 'accelero': 284169062, 'latitude': 285143911, 'longitude': 285143908, 'heart': 150929505, 'speed': 285143400, 'heading': 301008233, 'lidar_detect': 285139056, 'lux': 150929248}
```

### Listen on the specific topic

```bash
mosquitto_hub -h 127.0.0.1 -p 1883 -t 'rpi/#' -v
```

