---
:grey_exclamation: **NOTE:**
Please read [this guide](https://github.com/uzh-rpg/rpg_esim/wiki/Installation-(ROS-Melodic)) if you have to use ROS Melodic.

---

## Installation

If it is not installed on your system yet, please install [ROS](http://www.ros.org/). Installation instructions can be found [here](http://wiki.ros.org/kinetic/Installation/Ubuntu).
We tested ESIM with Ubuntu 16.04 and ROS Kinetic. If you have to use ROS Melodic, some specific instructions can be found [here](https://github.com/uzh-rpg/rpg_esim/wiki/Installation-(ROS-Melodic)).

We recommend to create a new catkin workspace specifically for the simulator as follows:
    
    mkdir -p ~/sim_ws/src && cd ~/sim_ws
    catkin init
    catkin config --extend /opt/ros/kinetic --cmake-args -DCMAKE_BUILD_TYPE=Release

Install `vcstools` if you do not have it already:

    sudo apt-get install python-vcstool

Clone this repository and run `vcstools`:

    cd src/
    git clone git@github.com:uzh-rpg/rpg_esim.git
    vcs-import < rpg_esim/dependencies.yaml

Install `pcl_ros`:

    sudo apt-get install ros-kinetic-pcl-ros
    sudo apt-get install libproj-dev

Install `glfw`:

    sudo apt-get install libglfw3 libglfw3-dev

Install `glm`:

    sudo apt-get install libglm-dev

Optionally install the trajectory server:

    sudo apt-get install ros-kinetic-hector-trajectory-server

Disable the packages that are not needed:

    cd ze_oss
    touch imp_3rdparty_cuda_toolkit/CATKIN_IGNORE \
          imp_app_pangolin_example/CATKIN_IGNORE \
          imp_benchmark_aligned_allocator/CATKIN_IGNORE \
          imp_bridge_pangolin/CATKIN_IGNORE \
          imp_cu_core/CATKIN_IGNORE \
          imp_cu_correspondence/CATKIN_IGNORE \
          imp_cu_imgproc/CATKIN_IGNORE \
          imp_ros_rof_denoising/CATKIN_IGNORE \
          imp_tools_cmd/CATKIN_IGNORE \
          ze_data_provider/CATKIN_IGNORE \
          ze_geometry/CATKIN_IGNORE \
          ze_imu/CATKIN_IGNORE \
          ze_trajectory_analysis/CATKIN_IGNORE


Build the `event_camera_simulator` node:

    catkin build esim_ros

Make an alias for your workspace so you can source it easily next time.

    echo "source ~/sim_ws/devel/setup.bash" >> ~/setupeventsim.sh
    chmod +x ~/setupeventsim.sh

In your `.bashrc` file, add the following line:

    alias ssim='source ~/setupeventsim.sh'

From now on, typing `ssim` in a terminal will initialize the simulator workspace (you need to run `bash` first if you want to try this right after editing the `.bashrc` file.

## Testing

Congratulations! At this point you should be all set to try the simulator. Among the [multiple rendering engines available](https://github.com/uzh-rpg/rpg_esim/wiki), we recommend that you start with the [Planar Rendering Engine](https://github.com/uzh-rpg/rpg_esim/wiki/Planar-Renderer) to check that everything is up and running.
