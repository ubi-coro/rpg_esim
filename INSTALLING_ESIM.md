# Installing ESIM on Ubuntu 24.04 Using DistroBox

This guide explains how to set up the Event-based Simulator (ESIM) on Ubuntu 24.04 using a DistroBox container running Ubuntu 16.04.

## Install `distrobox`

First, install `distrobox` and `podman`:

```sh
sudo apt -y install distrobox podman
```

## Set Up Ubuntu 16.04 in DistroBox

1. Create a DistroBox container with Ubuntu 16.04:

```sh
distrobox create --name ubuntu-16-04 -i ubuntu:16.04
```

2. Enter the container:

```sh
distrobox enter ubuntu-16-04
```

## Install ROS Kinetic

For a detailed explanation of all the steps, visit https://wiki.ros.org/kinetic/Installation/Ubuntu

Follow these steps to install ROS Kinetic inside the container:

1. Add the ROS repository:

```sh
sudo apt -y install lsb-release curl
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

2. Update and install ROS Kinetic:

```sh
sudo apt-get update
sudo apt-get install ros-kinetic-desktop-full
```

3. Set up the ROS environment:

```sh
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

4. Install ROS tools and dependencies:

```sh
sudo apt -y install \
    python-rosdep python-rosinstall \
    python-rosinstall-generator \
    python-wstool build-essential python-rosdep
sudo rosdep init
rosdep update
```

5. Install `catkin` tools:

```sh
sudo apt-get install ros-kinetic-catkin python-catkin-tools
```

## Install ESIM

1. Create a Catkin Workspace for the simulator:

```sh
mkdir -p ~/sim_ws/src && cd ~/sim_ws
catkin init
catkin config --extend /opt/ros/kinetic --cmake-args -DCMAKE_BUILD_TYPE=Release
```

2. Install `vcstool` for managing dependencies:

```sh
sudo apt-get install python-vcstool
```

3. Clone the `rpg_esim` repository:

```sh
cd src/
git clone https://github.com/uzh-rpg/rpg_esim.git
```

4. Convert SSH-based URLs in the `dependencies.yaml` file to HTTPS:

```sh
sed -E 's#git@([^:]+):([^/]+)/([^\.]+)\.git#https://\1/\2/\3.git#g' rpg_esim/dependencies.yaml > rpg_esim/http_dependencies.yaml
```

5. Import dependencies using `vcstool`:

```sh
vcs-import < rpg_esim/http_dependencies.yaml
```

6. Install the following required packages

```sh
sudo apt-get install ros-kinetic-pcl-ros
sudo apt-get install libproj-dev

sudo apt-get install libglfw3 libglfw3-dev

sudo apt-get install libglm-dev

sudo apt-get install ros-kinetic-hector-trajectory-server
```

7. Disable unused packages to streamline the build process:

```sh
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
```

8. Build the `esim_ros` node:

```sh
catkin build esim_ros
```

9. Create a script to initialize the ESIM workspace:

```sh
echo "source ~/sim_ws/devel/setup.bash" >> ~/setup_event_sim.sh
chmod +x ~/setup_event_sim.sh
```

From now on, **every time you start a new terminal**, run `distrobox enter ubuntu-16-04` and then `source ~/setup_event_sim.sh` to initialize the simulator workspace.

## Next Steps

Congratulations! The simulator is now set up. For examples and usage instructions, refer to the following pages:

* https://github.com/uzh-rpg/rpg_esim/wiki/Multi-Objects-2D-renderer
* https://github.com/uzh-rpg/rpg_esim/wiki/Simulating-events-from-a-video
* https://github.com/uzh-rpg/rpg_esim/wiki/Planar-Renderer
