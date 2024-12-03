Rendering engine based on pure OpenGL code, that can simulate simple 3D scenes, including the possibility to simulate dynamic objects moving in the scene.

## Run
First set the absolute paths in `opengl.conf`:

    roscd esim_ros
    gedit cfg/opengl.conf

Run the simulator as follows:

    roslaunch esim_ros esim.launch config:=cfg/opengl.conf

At this point, the simulator should be running, however by default it does not show any visualization.

To visualize the output of the simulator, you can open `rviz` (from a new terminal) as follows:

    roscd esim_visualization
    rviz -d cfg/esim.rviz

You can also open `rqt` for more visualizations, as follows:

    roscd esim_visualization
    rqt --perspective-file cfg/esim.perspective

## Scenes

[Assimp](http://www.assimp.org/) is used to read a 3D object used as scene, for example an .obj file.
Use `--render_scene` to specify the path to some object.

### Blender

Theoretically, Blender-files are supported by Assimp, but are loaded somehow incorrectly.
Better export your scene as a .obj file. Make sure of the following things:

- Materials must have UV maps defined for textures (In Textures: Mapping: Coordinates select 'UV').
- Choose 'Z Up' and 'Y forward' when exporting as .obj
- Make sure 'include UVs' and 'write materials' are enabled when exporting.
- Make sure 'Path Mode' is set to 'Copy' (this will export and copy the texture files to the same folder as the .obj file)
