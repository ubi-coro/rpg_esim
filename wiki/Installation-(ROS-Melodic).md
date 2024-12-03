We recommend to use ROS Kinetic with this simulator. However, ROS Kinetic is not supported on Ubuntu distributions > 16.04 LTS. In this case we recommend to upgrade ROS to Melodic, because we experienced library conflicts and similar bugs when the Ubuntu version is not in sync with what's recommended by ROS.

This page collects information on how to install the simulator on a newer ROS version (Melodic in combination with GCC 7.x).

## Catkin Configuration
```
catkin config --extend /opt/ros/melodic --cmake-args -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_FLAGS=-Wno-int-in-bool-context
```
## (Optional) Hector Trajectory Server

**Note**: this step is optional. ESIM does not need the hector trajectory server. It is only used to display the camera trajectory in RVIZ for visualization.

The hector trajectory server package is not available for ros melodic. But we can manually compile it together with the other dependencies. Download the packages from [their Github Repository](https://github.com/tu-darmstadt-ros-pkg/hector_slam) and put them in the source directory, next to rpg_esim (`sim_ws/src`). The packages are detected `catkin` during a regular compilation process.

Required Packages:
 - hector_map_tools
 - hector_nav_msgs
 - hector_trajectory_server
