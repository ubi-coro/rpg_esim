Rendering engine that simulates a 3D camera motion in a planar scene (one plane with texture attached to it).

## Run
First set the absolute paths in `example.conf`:

    roscd esim_ros
    gedit cfg/example.conf 

Run the simulator as follows:

    roslaunch esim_ros esim.launch config:=cfg/example.conf

To visualize the output of the simulator, you can open `rviz` (from a new terminal) as follows:

    roscd esim_visualization
    rviz -d cfg/esim.rviz

You can also open `rqt` for more visualizations, as follows:

    roscd esim_visualization
    rqt --perspective-file cfg/esim.perspective