# Knee Angle Measurement System

A real-time knee biomechanics monitoring system for gait analysis and rehabilitation assessment. Uses dual IMU sensors positioned on the thigh and shank to measure knee flexion/extension angles continuously via wireless Bluetooth connectivity.

## Project Overview

This system provides objective, quantitative measurement of knee joint angles for:
- **Gait Analysis** - Monitor walking patterns and knee motion during ambulation
- **Rehabilitation Tracking** - Assess range of motion recovery post-surgery or injury
- **Movement Assessment** - Evaluate functional knee mobility in clinical or research settings
- **Real-time Feedback** - Live visualization for patient/clinician monitoring

### Hardware Design

The measurement device consists of:
- **Dual BNO055 IMU Sensors** - One mounted on the thigh, one on the shank
- **ESP32 Microcontroller** - Processes sensor data and transmits via Bluetooth
- **Sensor Fusion Algorithm** - Utilizes built-in 9-axis fusion for accurate orientation tracking

### Measurement Principle

The system calculates knee angle using **relative orientation measurement**:
1. Each BNO055 sensor independently determines its 3D orientation using sensor fusion (accelerometer, gyroscope, magnetometer)
2. The pitch angle (sagittal plane rotation) from each sensor is extracted
3. **Knee Angle = |Thigh Pitch - Shank Pitch - Zero Offset|**
4. Absolute value ensures positive readings during both flexion and extension
5. Zero/tare function allows setting any position as the reference angle

This approach provides accurate flexion/extension measurement while being robust to overall body orientation.

## Features

- 📊 **Live Angle Display** - Real-time knee angle visualization
- 📈 **Data History** - Continuous plotting of angle measurements over time
- ⚙️ **Zero/Tare Function** - Set current position as 0° reference for relative measurements
- 🔋 **Calibration Status** - Visual indicator of sensor calibration quality (affects accuracy)
- 💾 **Data Export** - Save session data to CSV for offline analysis
- 📱 **Wireless Connection** - Bluetooth connectivity for untethered movement

## System Setup

### Running the Dashboard

The easiest way to start the system:

```bash
./start.sh
```

This script automatically:
1. Installs required software dependencies
2. Starts the data server
3. Opens the dashboard in your browser at http://localhost:3000

### Device Connection

1. Power on the ESP32 device
2. Ensure Bluetooth is enabled on your computer
3. The system will automatically detect and connect to "ESP32_Knee_Tracker"
4. Connection status is displayed on the dashboard

### Using the System

**Before Measurement:**
1. Ensure sensors show "Calibrated" status (may require moving the device through full range of motion)
2. Position sensors securely on thigh and shank segments
3. Place leg in desired reference position (e.g., full extension)
4. Click "Zero" button to set current angle as 0°

**During Measurement:**
- Live angle updates at 20Hz (20 readings per second)
- Graph displays continuous angle history
- Export data at any time using "Download CSV" button

**Commands:**
- **Zero (z)** - Set current position as reference angle
- **Save Calibration (c)** - Store sensor calibration to device memory

## Technical Specifications

### Measurement Capabilities
- **Sampling Rate**: 20 Hz
- **Angular Range**: 0-180° (flexion/extension)
- **Resolution**: 0.1° precision
- **Latency**: <50ms from movement to display

### Sensor Details
- **Model**: BNO055 Absolute Orientation Sensor
- **Fusion Algorithm**: Built-in 9-axis sensor fusion (accelerometer + gyroscope + magnetometer)
- **I2C Communication**: Dual independent I2C buses to prevent address conflicts
- **Calibration**: Automatic calibration with persistent storage

### Data Output Format
The device transmits data via Bluetooth Serial:
```
Knee_Angle:45.3,Calib_Status:3
```
- `Knee_Angle`: Calculated knee flexion angle (degrees)
- `Calib_Status`: Sensor calibration quality (0-3, where 3 = fully calibrated)

## Potential Applications

- Pre/post-operative range of motion assessment
- Physical therapy progress monitoring
- Sports biomechanics research
- Prosthetic and orthotic fitting evaluation
- Home-based rehabilitation with remote monitoring

## Data Export

Exported CSV files include timestamp, knee angle, and calibration status for offline analysis in MATLAB, Excel, or statistical software.

## Project Structure

```
knee-dashboard/
├── app/                      # Dashboard interface
├── backend/                  # Data server and device communication
├── knee_measuring_device.ino # ESP32 firmware (Arduino code)
├── start.sh                  # Quick start script
└── README.md                 # This file
```

## Troubleshooting

**Connection Issues:**
- Ensure Bluetooth is enabled on your computer
- Check that ESP32 is powered and showing "ESP32_Knee_Tracker" in Bluetooth devices
- Restart both the device and dashboard if connection fails

**Calibration Issues:**
- Move the device in a figure-8 pattern through full range of motion
- Ensure no magnetic interference from metal objects
- Calibration status should reach 3 before measurements

**For technical details**, see [backend/README.md](backend/README.md) or refer to hardware setup in `knee_measuring_device.ino`.

---

**Biomedical Engineering Project** - Knee Angle Measurement for Gait Analysis and Rehabilitation
