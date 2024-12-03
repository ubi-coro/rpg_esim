Rendering engine that simulate camera rotation provided a panoramic map of the scene in equirectangular format (such as [this one](https://github.com/uzh-rpg/rpg_esim/blob/master/event_camera_simulator/imp/imp_panorama_renderer/textures/bicycle_parking.jpg) for example).

## Run
First set the absolute paths in `panorama.conf`:

    roscd esim_ros
    gedit cfg/panorama.conf

Run the simulator as follows (do not forget to set the absolute paths in `panorama.conf`):

    roslaunch esim_ros esim.launch config:=cfg/panorama.conf

To visualize the output of the simulator, you can open `rviz` (from a new terminal) as follows:

    roscd esim_visualization
    rviz -d cfg/esim.rviz

You can also open `rqt` for more visualizations, as follows:

    roscd esim_visualization
    rqt --perspective-file cfg/esim.perspective
