

# 🛰️ Xsens MTi-680 IMU/GNSS ROS 2 Driver (Humble)

## Overview

This repository provides a **ROS 2 driver** for the **Xsens MTi-680G**, MTi-8, MTi-630, and MTi-300, supporting **ROS 2 Humble** (also tested on Foxy and Jazzy). It includes:

* Full IMU and GNSS data output
* Real-time RTK positioning via **Ntrip Client**
* High-rate IMU output topics
* Filtered position, velocity, attitude (Euler/Quaternion)
* Easy launch with RViz visualization

> ✅ Based on Xsens Device API 2025.0 and tested on **Ubuntu 22.04.3 LTS**

---
## 🛠️ Device Configuration in MT Manager

### ✅ Output Configuration for ROS2 & RTK
Make sure to enable these:
- UTC Time ✔️
- Status Word ✔️
- Latitude and Longitude ✔️
- Velocity ✔️

📷 Example:
![Output Configuration](images/MTi-680_Output_Configuration.png)

---

### ⚙️ Device Settings for GNSS and High Baudrate
- Baudrate: 921600 (to avoid overflow)
- GNSS Platform: u-Blox ZED-F @ 230400
- Filter Settings: General_RTK
- Set your GNSS Lever Arm precisely

📷 Example:
![Device Settings](images/MTi-680_Device_Settings.png)

## 🚀 Installation

### 1. Clone the ROS2 Branch

```bash
git clone --branch ros2 https://github.com/Pakgard007/Xsens-MTi-ROS-Driver-and-Ntrip-Client_ROS2_humble
```

### 2. Install Dependencies

```bash
sudo apt install ros-humble-nmea-msgs
sudo apt install ros-humble-mavros-msgs
```

### 3. Build the Workspace

```bash
cd ~/ros2_ws/src
# Place `xsens_mti_ros2_driver` and `ntrip` folders here
cd ~/ros2_ws
colcon build
source install/setup.bash
```

To avoid sourcing every time, add this to your `~/.bashrc`:

```bash
source ~/ros2_ws/install/setup.bash
```

---

## ⚙️ Device Configuration

Before using, open **MT Manager** and enable the following Output Configurations:

| Required Fields          |
| ------------------------ |
| ✅ UTC Time               |
| ✅ SampleTimeFine         |
| ✅ Status Word            |
| ✅ Latitude and Longitude |

Then click **Apply** to save settings.

---

## 📡 RTK Support with NTRIP

### Setup

1. Open `src/ntrip/launch/ntrip_launch.py`
2. Edit with your **NTRIP credentials, server, port, and mountpoint**

### Launch

```bash
# Terminal 1: Start the IMU/GNSS driver
ros2 launch xsens_mti_ros2_driver xsens_mti_node.launch.py

# Or with RViz display
ros2 launch xsens_mti_ros2_driver display.launch.py

# Terminal 2: Start the NTRIP client
ros2 launch ntrip ntrip_launch.py
```

---

## ✅ Verify RTK Status

Check:

```bash
ros2 topic echo /rtcm          # Should show HEX RTCM messages
ros2 topic echo /status        # Look for Fix Type = 1 (Floating) or 2 (Fixed)
```

---

## 🧭 Available ROS Topics

| Topic                      | Message Type                      | Description                      |
| -------------------------- | --------------------------------- | -------------------------------- |
| `/filter/euler`            | `geometry_msgs/Vector3Stamped`    | Euler angles (roll, pitch, yaw)  |
| `/filter/quaternion`       | `geometry_msgs/QuaternionStamped` | Orientation in quaternion        |
| `/filter/velocity`         | `geometry_msgs/Vector3Stamped`    | Filtered velocity                |
| `/gnss_pose`               | `geometry_msgs/PoseStamped`       | GNSS position + orientation      |
| `/imu/data`                | `sensor_msgs/Imu`                 | Combined quaternion, gyro, accel |
| `/imu/acceleration_hr`     | `geometry_msgs/Vector3Stamped`    | High-rate acceleration           |
| `/imu/angular_velocity_hr` | `geometry_msgs/Vector3Stamped`    | High-rate angular velocity       |
| `/nmea`                    | `nmea_msgs/Sentence`              | GPGGA string (for NTRIP)         |
| `/status`                  | `xsens_mti_driver/XsStatusWord`   | Device status and RTK Fix        |

👉 For full list, refer to the official [MTi Family Reference Manual](https://www.xsens.com/mti)

---

## ✨ Features

* GNSS + IMU Integration (MTi-680G)
* RTK via NTRIP Caster
* Full 3D orientation and position support
* High-frequency IMU data
* ROS2-compliant packaging and launch files

---

## 🧰 Supported Devices

* ✅ MTi-680G
* ✅ MTi-8
* ✅ MTi-630
* ✅ MTi-300

---


