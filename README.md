# ESIM: an Open Event Camera Simulator

[<img src="http://rpg.ifi.uzh.ch/esim/img/youtube_preview.png" alt="ESIM: an Open Event Camera Simulator">](https://youtu.be/ytKOIX_2clo)

This is the code for the 2018 CoRL paper **ESIM: an Open Event Camera Simulator** by [Henri Rebecq](http://henri.rebecq.fr), [Daniel Gehrig](https://danielgehrig18.github.io/) and [Davide Scaramuzza](http://rpg.ifi.uzh.ch/people_scaramuzza.html):

```bibtex
@Article{Rebecq18corl,
  author        = {Henri Rebecq and Daniel Gehrig and Davide Scaramuzza},
  title         = {{ESIM}: an Open Event Camera Simulator},
  journal       = {Conf. on Robotics Learning (CoRL)},
  year          = 2018,
  month         = oct
}
```

> [!NOTE]
> 
> If you use any of this code, please cite this publication.

## Python Bindings

Python bindings for the event camera simulator can be found [here](https://github.com/uzh-rpg/rpg_vid2e). 
We now also support GPU support for fully parallel event generation!


## Features

- Accurate event simulation, guaranteed by the tight integration between the rendering engine and the event simulator
- Inertial Measurement Unit (IMU) simulation
- Support for multi-camera systems
- Ground truth camera poses, IMU biases, angular/linear velocities, depth maps, and optic flow maps
- Support for camera distortion (only planar and panoramic renderers)
- Different C+/C- contrast thresholds
- Basic noise simulation for event cameras (based on additive Gaussian noise on the contrast threshold)
- Motion blur simulation
- Publish to ROS and/or save data to rosbag

# Getting Started

* [Installation instructions](./wiki/Installation.md)
* [Installation (ROS-Melodic)](./wiki/Installation-(ROS-Melodic).md)
* [Installation with DistroBox](./wiki/INSTALLING_ESIM.md)

## Tutorials

- [Simulating events from a video](./wiki/Simulating-events-from-a-video.md)

### Advanced

- [Writing your own publisher](./wiki/Writing-your-own-publisher.md)

# Available Rendering Engines

Below is a table summarizing the different rendering engines available to date, with their respective properties.

| | Dimension | IMU | Moving Objects | Camera Distortion | Simulation speed |
|-|-|-|-|-|-|
| [MultiObjects2D](./wiki/Multi-Objects-2D-renderer) | 2D | No | Yes | No | ![60%](https://progress-bar.xyz/60) |
| [PanoramicRenderer](./wiki/Panoramic-Renderer) | 2D (rotation only) | Yes | No | Yes | ![80%](https://progress-bar.xyz/80) |
| [PlanarRenderer](./wiki/Planar-Renderer) | 2.5D (planar) | Yes | No | Yes | ![80%](https://progress-bar.xyz/80) |
| [OpenGLRenderer](./wiki/3D-OpenGL-Rendering-Engine) | 3D | Yes | Yes | No | ![100%](https://progress-bar.xyz/100) |
| [UnrealCVRenderer](./wiki/Photorealistic-Rendering-Engine-based-on-Unreal-Engine) | 3D | Yes | No | No | ![20%](https://progress-bar.xyz/20) |
