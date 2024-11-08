# tiago_harmonic

![distro](https://img.shields.io/badge/Ubuntu%2024-Jammy%20Jellyfish-orange)
![distro](https://img.shields.io/badge/ROS2-Jazzy-blue)
[![jazzy](https://github.com/Tiago-Harmonic/tiago_harmonic/actions/workflows/jazzy_devel.yaml/badge.svg)](https://github.com/Tiago-Harmonic/tiago_harmonic/actions/workflows/jazzy_devel.yaml)

## Authors
* Juan Carlos Manzanares Serrano - juancarlos.serrano@urjc.es 
* Francisco Martín Rico - fmrico@gmail.com
* Juan Sebastían Cely Gutiérrez - juan.cely@urjc.es 
  
## Installation

You need to have previously installed ROS2. Please follow this [guide](https://docs.ros.org/en/jazzy/Installation.html) if you don't have it.

```bash
source /opt/ros/jazzy/setup.bash
```

Create workspace and clone the repository

```bash
mkdir ~/tiago_ws/src
cd ~/tiago_ws/src
git clone https://github.com/Tiago-Harmonic/tiago_harmonic.git -b jazzy
vcs import . < tiago_harmonic/dependencies.repos
```

Install dependencies and build workspace
```bash
cd ~/tiago_ws
sudo rosdep init
rosdep update
rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install 
```

Setup the workspace
```bash
source ~/tiago_ws/install/setup.bash
```

## Usage

```bash
ros2 launch tiago_gazebo tiago_gazebo.launch.py is_public_sim:=True world_name:=house # or empty
```

[![See in YouTube](https://img.youtube.com/vi/k_6EIEdKrc0/0.jpg)](https://www.youtube.com/watch?v=k_6EIEdKrc0)

