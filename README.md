# tiago_harmonic

![distro](https://img.shields.io/badge/Ubuntu%2024-Jammy%20Jellyfish-orange)
![distro](https://img.shields.io/badge/ROS2-Jazzy-blue)
[![jazzy](https://github.com/Tiago-Harmonic/tiago_harmonic/actions/workflows/jazzy_devel.yaml/badge.svg)](https://github.com/Tiago-Harmonic/tiago_harmonic/actions/workflows/jazzy_devel.yaml)

## Authors
* Juan Carlos Manzanares Serrano - juancarlos.serrano@urjc.es 
* Francisco Martín Rico - fmrico@gmail.com
* Juan Sebastían Cely Gutiérrez - juan.cely@urjc.es
  
[![See in YouTube](https://img.youtube.com/vi/k_6EIEdKrc0/0.jpg)](https://www.youtube.com/watch?v=k_6EIEdKrc0)
  
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

### Start Tiago, simulated in Gazebo Harmonic
```bash
ros2 launch tiago_gazebo tiago_gazebo.launch.py is_public_sim:=True world_name:=house # or empty
```

### Teleoperate the mobile base

Velocity commands are expected on `/key_vel`:
```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -r /cmd_vel:=/key_vel
```

### Run SLAM mapping, save map, and navigate in 2D

Start SLAM:
```bash
ros2 launch tiago_2dnav tiago_nav_bringup.launch.py slam:=True is_public_sim:=True
```

Then teleoperate the mobile base to collect map data.

Create a new map directory and save the new map under name `our_map` into the `pal_maps` package:
```bash
mkdir ~/tiago_ws/src/pal_maps/maps/our_map/
ros2 run nav2_map_server map_saver_cli -f ~/tiago_ws/src/pal_maps/maps/our_map/map
```

For offline navigation in the saved map, make sure that SLAM is closed and load the saved map with `tiago_nav_bringup`:
```bash
ros2 launch tiago_2dnav tiago_nav_bringup.launch.py is_public_sim:=True world_name:=our_map
```

If the new map does not properly load: build and source the workspace. But that should not be needed with `--symlink-install`.

Then:
- use `Estimate 2D pose` in RViz to help AMCL to initialize, and perform some teleoperated movements until AMCL particles have converged.
- use `2D nav goal` in RViz to define a target to navigate to. Make sure that teleoperation does not publish contradictory commands meanwhile.

### Run MoveIt GUI for arm path planning

```bash
ros2 launch tiago_moveit_config moveit_rviz.launch.py
```

Then use the color handles and click on `Plan and Execute` to trigger trajectory planning and motion.

### Command MoveIt from a Python script

Ues the MoveItPy boilerplate to send cartesian-space goals or joint-space goals to MoveIt, as well as for gripper open/close commands:
```bash
cd ~/tiago_ws/src
git clone https://github.com/Tiago-Harmonic/tiago_moveitpy
cd ~/tiago_ws
rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install
```

The node must be launched through the launchfile so that the planning pipeline is loaded to the parameters:
```bash
ros2 launch tiago_moveitpy plan.launch.py use_sim_time:=True
```

Comment [any of the method calls](https://github.com/Tiago-Harmonic/tiago_moveitpy/blob/c35e6f4dab48b20f07f2a700244dce1f5fd460a0/tiago_moveitpy/pick.py#L108-L110) to disable either cartesian-space, joint-space, or gripper motions.

