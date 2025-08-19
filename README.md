# ArduSim: Basic Package for running ArduPilot Simulations with ROS and Gazebo

ArduSim provides a minimal setup to run **ArduPilot SITL (Software In The Loop)** with **ROS** and **Gazebo**, allowing you to test, simulate, and validate robotic/autonomous solutions.

---

## 🚀 Main Features
- Integration of **ArduPilot SITL** with **ROS Noetic**.
- Support for **Gazebo simulation environments**.
- Includes setup steps for **MAVROS** to enable ROS-based mission planning/control.
- Easy testing framework to get started with ArduPilot + ROS.

---
## 📋 Prerequisites
Before starting, ensure you have the following installed on your system:

- **Ubuntu 20.04 LTS** (recommended, since ROS Noetic supports it)
- **ROS Noetic** (Desktop-Full version recommended)
- **Python 3.8+** (installed by default with Ubuntu 20.04)
---

## ⚙️ Basic Installation for Running ArduPilot SITL in Gazebo

### 1. Install ArduPilot and MAVProxy for running SITL

#### a. Clone the ArduPilot repository:
```bash
cd ~
sudo apt install git
git clone https://github.com/ArduPilot/ardupilot.git
```

#### b. Install the dependencies and reload the profile:
```
cd ardupilot
Tools/environment_install/install-prereqs-ubuntu.sh -y
. ~/.profile
```
#### c. Checkout Latest Copter Build
```
git checkout Copter-4.6.1
git submodule update --init --recursive
```

#### d. Run SITL once to set params:
```
cd ~/ardupilot/ArduCopter
sim_vehicle.py -w
```

### 2. Install Gazebo and ArduPilot Plugin
#### a. Setup your computer to accept software from http://packages.osrfoundation.org:
```
sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
sudo apt update
```

#### b. Install Gazebo 11 (recommended for Ubuntu 20.04):
```
sudo apt-get install gazebo11 libgazebo11-dev
```
for more detailed instructions for installing gazebo checkout http://gazebosim.org/tutorials?tut=install_ubuntu

***Note: You can skip this step if Gazebo is already installed.***

#### c. Install ArduPilot Gazebo Plugin
Clone the ```ardupilot_gazebo``` repo in the Home directory:
```
cd ~
git clone https://github.com/khancyr/ardupilot_gazebo.git

```
Build and install the Plugin:
```
cd ardupilot_gazebo
mkdir build
cd build
cmake ..
make -j4
sudo make install
echo 'source /usr/share/gazebo/setup.sh' >> ~/.bashrc
```
Set paths for models:
```
echo 'export GAZEBO_MODEL_PATH=~/ardupilot_gazebo/models' >> ~/.bashrc
. ~/.bashrc
```

#### d. Run the Simulator
In one Terminal, run Gazebo:
```
gazebo --verbose ~/ardupilot_gazebo/worlds/iris_arducopter_runway.world
```

In another Terminal, run SITL:
```
cd ~/ardupilot/ArduCopter/
sim_vehicle.py -v ArduCopter -f gazebo-iris --console
```

---

## Running ArduPilot Simulation with ROS and MAVROS

### Install MAVROS:

#### a. Install GeographicLib Datasets:
```
wget https://raw.githubusercontent.com/mavlink/mavros/master/mavros/scripts/install_geographiclib_datasets.sh
chmod a+x install_geographiclib_datasets.sh
./install_geographiclib_datasets.sh
```

#### b. Install MAVROS package:
```
sudo apt-get install ros-noetic-mavros ros-noetic-mavros-extras
```

#### c. Install RQT:
```
sudo apt-get install ros-noetic-rqt ros-noetic-rqt-common-plugins ros-noetic-rqt-robot-plugins
```
