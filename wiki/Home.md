# Getting Started

[Installation instructions](https://github.com/uzh-rpg/rpg_esim/wiki/Installation)

## Tutorials

- [Simulating events from a video](https://github.com/uzh-rpg/rpg_esim/wiki/Simulating-events-from-a-video)

### Advanced

- [Writing your own publisher](https://github.com/uzh-rpg/rpg_esim/wiki/Writing-your-own-publisher)

# Available Rendering Engines

Below is a table summarizing the different rendering engines available to date, with their respective properties.

|                   | Dimension          | IMU | Moving Objects | Camera Distortion | Simulation speed |
|-------------------|--------------------|-----|----------------|-------------------|------------------|
| [MultiObjects2D](https://github.com/uzh-rpg/rpg_esim/wiki/Multi-Objects-2D-renderer)    | 2D                 | No  | Yes            | No                |  ***             |
| [PanoramicRenderer](https://github.com/uzh-rpg/rpg_esim/wiki/Panoramic-Renderer) | 2D (rotation only) | Yes | No             | Yes               | ****             |
|  [PlanarRenderer](https://github.com/uzh-rpg/rpg_esim/wiki/Planar-Renderer)   | 2.5D (planar)      | Yes | No             | Yes               | ****             |
| [OpenGLRenderer](https://github.com/uzh-rpg/rpg_esim/wiki/3D-OpenGL-Rendering-Engine)    | 3D                 | Yes | Yes            | No                | *****            |
| [UnrealCVRenderer](https://github.com/uzh-rpg/rpg_esim/wiki/Photorealistic-Rendering-Engine-based-on-Unreal-Engine)  | 3D                 | Yes | No             | No                | *                |
