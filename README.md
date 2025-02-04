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



