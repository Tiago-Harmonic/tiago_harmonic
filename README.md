# tiago_harmonic

Athors:
* Juan Carlos Manzanares Serrano - juancarlos.serrano@urjc.es 
* Francisco Martín Rico - fmrico@gmail.com
* Juan Sebastían Cely Gutiérrez - juan.cely@urjc.es 
  
Usage:

```
$ mkdir -p tiago_ws/src
$ cd tiago_ws/src
$ git clone https://github.com/Tiago-Harmonic/tiago_harmonic.git
$ vcs import . < tiago_harmonic/dependencies.repos
$ cd ..
$ colcon build --symlink-install
$ source install/setup.bash 
$ ros2 launch tiago_gazebo tiago_gazebo.launch.py is_public_sim:=True world_name:=house # or empty
```
[![See in YouTube](https://img.youtube.com/vi/k_6EIEdKrc0/0.jpg)](https://www.youtube.com/watch?v=k_6EIEdKrc0)

