A simple 2D rendering engine that simulates several 2D objects moving over a planar background.

## Example
First set the absolute paths in `multi_objects.conf` and `example.scene`:

    roscd esim_ros
    gedit cfg/multi_objects.conf
    roscd imp_multi_objects_2d
    gedit scenes/example.scene

To run the example:

    roslaunch esim_ros esim.launch config:=cfg/multi_objects.conf

You should get a result similar to [this video](https://youtu.be/Yre8iS2LqPc).

To visualize the output of the simulator, you can open `rviz` (from a new terminal) as follows:

    roscd esim_visualization
    rviz -d cfg/esim.rviz

You can also open `rqt` for more visualizations, as follows:

    roscd esim_visualization
    rqt --perspective-file cfg/esim.perspective

## Format of a `scene` file

The configuration of this rendering engine can be set through a `scene` file (example [here](https://github.com/uzh-rpg/rpg_esim/blob/master/event_camera_simulator/imp/imp_multi_objects_2d/scenes/example.scene)).

A scene file with N layers has the following structure:

    width height tmax (s)
    <layer 1>
    <layer 2>
    ...
    <layer N>

where each layer line has the format:

    path_to_image median_filter_size gaussian_blur_sigma theta0 theta1 x0 x1 y0 y1 sx0 sx1 sy0 sy1

describing which image should be used for the layer, how much preprocessing should be applied to it, and the parameters of the affine transformation it will undergo between t=0 and t=tmax
