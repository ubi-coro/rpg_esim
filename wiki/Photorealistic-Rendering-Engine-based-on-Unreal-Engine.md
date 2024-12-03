Photorealistic rendering engine based on [Unreal Engine](https://www.unrealengine.com), using the [UnrealCV](https://github.com/unrealcv/unrealcv) project.

## Preparation

You need first to set up an UnrealCV plugin, or simply download one from the [Model Zoo](http://docs.unrealcv.org/en/latest/reference/model_zoo.html). We recommend to start first with the `RealisticRendering` model. Download it using:
    
    cd ~/Downloads
    wget http://cs.jhu.edu/~qiuwch/release/unrealcv/RealisticRendering-Linux-0.3.10.zip

and unzip it:

    unzip RealisticRendering-Linux-0.3.10.zip

Before running the simulator, you need to launch the UnrealCV server as follows:

    ./RealisticRendering/LinuxNoEditor/playground.sh

Please make sure that the mouse input is disabled in the window that will pop up, by hitting the ` key while the window is in focus.

## Run

Once an UnrealCV server is running, you can run the simulator as follows:

    roslaunch esim_ros esim.launch config:=cfg/unrealcv.conf

To visualize the output of the simulator, you can open `rviz` (from a new terminal) as follows:

    roscd esim_visualization
    rviz -d cfg/esim.rviz

You can also open `rqt` for more visualizations, as follows:

    roscd esim_visualization
    rqt --perspective-file cfg/esim.perspective

Note that this rendering engine is very slow: rendering a one minute trajectory may take multiple hours.