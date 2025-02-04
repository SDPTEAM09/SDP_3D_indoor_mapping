# SDP_3D_Indoor_Mapping

## Overview
SDP 3D Indoor Mapping is a project designed to enable indoor environment mapping using ROS (Robot Operating System) with the Melodic distribution. This guide provides the steps for installing ROS Melodic on Ubuntu and setting up the necessary environment.

## ROS Melodic Installation Guide

### 1. Setup Your Sources List
Run the following command to add the ROS package repository to your system:
```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

### 2. Set Up Your Keys
Ensure you have `curl` installed and then add the ROS key:
```bash
sudo apt install curl # Install curl if not already installed
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

### 3. Update and Install ROS Melodic
```bash
sudo apt update
sudo apt install ros-melodic-desktop-full
```

### 4. Verify Available Packages
To check for available ROS Melodic packages, run:
```bash
apt search ros-melodic
```

### 5. Environment Setup
To automatically source the ROS setup file on every terminal session, add the following to your shell configuration:

#### For Bash Users:
```bash
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

#### For Zsh Users:
```bash
echo "source /opt/ros/melodic/setup.zsh" >> ~/.zshrc
source ~/.zshrc
```

Alternatively, source the setup file manually when needed:
```bash
source /opt/ros/melodic/setup.bash
```

----------------------------------------------------------------------

## Livox ROS Driver 2 Installation Guide

### 1. Preparation
#### 1.1 OS Requirements
- Ubuntu 18.04 for ROS Melodic

### 2. Install Required Dependencies
```bash
sudo apt update
sudo apt install -y python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential cmake git
```

### 3. Install Livox SDK2
```bash
git clone https://github.com/Livox-SDK/Livox-SDK2.git
cd Livox-SDK2
mkdir build
cd build
cmake .. && make -j
sudo make install
```
The generated shared library and static library will be installed to `/usr/local/lib`. The header files will be installed to `/usr/local/include`.

To remove Livox SDK2:
```bash
sudo rm -rf /usr/local/lib/liblivox_lidar_sdk_*
sudo rm -rf /usr/local/include/livox_lidar_*
```

### 4. Clone Livox ROS Driver 2 Source Code
```bash
git clone https://github.com/Livox-SDK/livox_ros_driver2.git ws_livox/src/livox_ros_driver2
```
Ensure that the repository is cloned inside the `[work_space]/src/` folder.

### 5. Build Livox ROS Driver 2
```bash
source /opt/ros/melodic/setup.sh
cd ws_livox
./build.sh ROS1
```

### 6. Run Livox ROS Driver 2
```bash
source devel/setup.sh
roslaunch livox_ros_driver2 [launch file]
```
Example:
```bash
roslaunch livox_ros_driver2 msg_MID360.launch
```

### 7. Verify Setup
Check that the LiDAR is publishing point cloud data and is visible in RViz.

### 8. Configuration Instructions
#### 8.1 Launch File Configuration
Launch files are located in:
```bash
ws_livox/src/livox_ros_driver2/launch_ROS1
```
Common launch files:
- `rviz_HAP.launch`: Connect to HAP LiDAR, publish `PointCloud2` data, and auto-load RViz.
- `msg_HAP.launch`: Connect to HAP LiDAR, publish Livox customized point cloud data.
- `rviz_MID360.launch`: Connect to MID360 LiDAR, publish `PointCloud2` data, and auto-load RViz.
- `msg_MID360.launch`: Connect to MID360 LiDAR, publish Livox customized point cloud data.
- `rviz_mixed.launch`: Connect to HAP and MID360 LiDARs, publish `PointCloud2` data, and auto-load RViz.
- `msg_mixed.launch`: Connect to HAP and MID360 LiDARs, publish Livox customized point cloud data.

#### 8.2 Internal Parameter Configuration
Parameters are set within the launch files:
- **publish_freq**: Frequency of point cloud publication. Default: `10.0 Hz`. Max: `100.0 Hz`.
- **multi_topic**: `0` (single topic for all LiDARs), `1` (each LiDAR has its own topic).
- **xfer_format**: `0` (Livox `PointCloud2` format), `1` (Livox custom format), `2` (PCL `PointCloud2` format).

### 9. LiDAR Configuration
LiDAR settings such as IP, port, and data format are specified in JSON config files:
- Located in `config/`
- Example: `config/HAP_config.json`
- Contains `lidar_summary_info`, `lidar_net_info`, and `host_net_info`

Example configuration:
```json
{
  "HAP": {
    "lidar_ipaddr": "192.168.1.100",
    "lidar_net_info": {
      "cmd_data_port": 56000,
      "point_data_port": 57000
    },
    "host_net_info": {
      "cmd_data_ip": "192.168.1.5",
      "point_data_ip": "192.168.1.5"
    }
  }
}
```

### 10. Troubleshooting
#### 10.1 No Point Cloud Display in RViz
Ensure `Global Options -> Fixed Frame` is set to `livox_frame` in RViz.

#### 10.2 `liblivox_sdk_shared.so` Not Found
Add `/usr/local/lib` to `LD_LIBRARY_PATH`:
```bash
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/lib
```
To make it persistent:
```bash
echo 'export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/lib' >> ~/.bashrc
source ~/.bashrc
```

---
This guide provides the essential steps to install and run **Livox ROS Driver 2** on **Ubuntu 18.04 with ROS Melodic**. Ensure that all dependencies are met and configurations are correctly set up for optimal performance.


-------------------------------------------------------------------

## FAST-LIO Installation Guide

### 1. Preparation
#### 1.1 OS Requirements
- Ubuntu 18.04 for ROS Melodic

### 2. Install Required Dependencies
```bash
sudo apt update
sudo apt install -y ros-melodic-pcl-ros ros-melodic-tf ros-melodic-message-filters ros-melodic-geometry-msgs ros-melodic-roscpp
```

### 3. Clone FAST-LIO Repository
```bash
cd ~/catkin_ws/src
git clone https://github.com/hku-mars/FAST_LIO.git
cd FAST_LIO
git submodule update --init
```

### 4. Modify FAST-LIO for Livox ROS Driver 2
Update references in FAST-LIO to use Livox ROS Driver 2:
```bash
gedit ~/catkin_ws/src/FAST_LIO/CMakeLists.txt
gedit ~/catkin_ws/src/FAST_LIO/src/preprocess.h
gedit ~/catkin_ws/src/FAST_LIO/src/laserMapping.cpp
```
Replace `livox_ros_driver` with `livox_ros_driver2`.

### 5. Build FAST-LIO
```bash
cd ~/catkin_ws
catkin_make
source devel/setup.bash
```

### 6. Run FAST-LIO
Ensure that Livox ROS Driver 2 is sourced before running FAST-LIO:
```bash
echo "source ~/ws_livox/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
Run FAST-LIO for Livox Mid-360:
```bash
roslaunch fast_lio mapping_mid360.launch
```


---
This guide provides the essential steps to install and run **FAST-LIO** with **Livox Mid-360** on **Ubuntu 18.04 with ROS Melodic**. Ensure all dependencies are installed and configurations are set correctly for optimal performance.


