# ROS2 Galactic Installation

[home](README.md)

---
## Introduction
How to flash download `ROS2` Galactic on an `Ubuntu 20.04`, more information can be found here, but if you run the following setup commands it will do the same. https://docs.ros.org/en/galactic/Installation/Ubuntu-Install-Debians.html

---

## Steps
Download `ROS2 Galactic`
```bash
apt-cache policy | grep universe
sudo apt update 
sudo apt install -y software-properties-common
sudo apt install -y curl gnupg lsb-release
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
sudo apt update
sudo apt install -y ros-galactic-desktop
```

Download `colcon` compiler (replace catkin)
```bash
# Guide is from https://colcon.readthedocs.io/en/released/user/installation.html
sudo sh -c 'echo "deb [arch=amd64,arm64] http://repo.ros2.org/ubuntu/main `lsb_release -cs` main" > /etc/apt/sources.list.d/ros2-latest.list'
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -

sudo apt update
sudo apt install python3-colcon-common-extensions
```