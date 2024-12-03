In this tutorial, we will show how to use ESIM to simulate events from a given video.

[[https://github.com/uzh-rpg/rpg_esim/blob/master/event_camera_simulator/img/example_video_to_events.gif|alt=Example conversion from video to events]]

Left: simulated APS frames (including motion blur)
Right: simulated events with ESIM.

### WARNING

Simulating events from a video **will not work for any video**. The input video needs to have a sufficiently high framerate: the pixel displacement between successive images needs to be small (i.e. a few pixels at most).
Otherwise, the simulated events will be of poor quality.

### Preliminaries (basic knowledge of ROS)

To follow this tutorial, it will be helpful to have some basic understanding of ROS (i.e. know what ROS nodes, messages and topics are). If you do know ROS yet, we recommend to skim through the [ROS beginner tutorials](http://wiki.ros.org/ROS/Tutorials), in particular [Understanding ROS Nodes](http://wiki.ros.org/ROS/Tutorials/UnderstandingNodes) and [Understanding ROS topics](http://wiki.ros.org/ROS/Tutorials/UnderstandingTopics).

### Overview

An overview of the conversion method is shown in the diagram below. First, we will extract the individual frames from the input video in a folder. Second, we will run ESIM, providing the image folder as an input to simulate events.
Finally, we will see how to visualize the simulated events using the `dvs_renderer` node.

[[https://github.com/uzh-rpg/rpg_esim/blob/master/event_camera_simulator/img/overview_video_to_events.png|alt=Overview of the methodology to convert a video to events]]

### (Optional) Download an example video

As an example input video, we will use this [nice video](https://youtu.be/THA_5cqAfCQ) from National Geographic, which shows a running cheetah recorded at 1200 frames per second with a Phantom camera.

- Create a working folder:
```
mkdir -p /tmp/cheetah_example
cd /tmp/cheetah_example
```
- Download the video from YouTube using `youtube-dl` (`pip install youtube-dl`):
```
youtube-dl https://youtu.be/THA_5cqAfCQ -o cheetah
```

- Cut the part of the video that is relevant. In our case, we will cut the slow motion part from `2'07` to `2'47''`:
```
ffmpeg -i cheetah.mkv -ss 00:02:07 -t 00:00:40 -async 1 -strict -2 cheetah_cut.mkv
```

- Optionally, resize the video to a smaller size for faster processing. Here, we will resize the video to a width of `640`:
```
ffmpeg -i cheetah_cut.mkv -vf scale=640:-1 -crf 0 cheetah_sd.mkv
```

### Pre-process the video for ESIM

- Export the video to a sequence of individual frames in the `frames` folder:
```
mkdir frames
ffmpeg -i cheetah_sd.mkv frames/frames_%010d.png
```

- Finally, create an `images.csv` file in the `frames` folder, which will be necessary for ESIM. Each line of this file contains the image timestamp and path in the format: `timestamp_in_nanoseconds,frames_xxxxx.png`:
```
roscd esim_ros
python scripts/generate_stamps_file.py -i /tmp/cheetah_example/frames -r 1200.0
```
where `-i` specifies the path to the folder containing the frames, and `-r` specifies the input video framerate (`1200 fps` in our case).

### Simulate events with ESIM

```
rosrun esim_ros esim_node \
 --data_source=2 \
 --path_to_output_bag=/tmp/out.bag \
 --path_to_data_folder=/tmp/cheetah_example/frames \
 --ros_publisher_frame_rate=60 \
 --exposure_time_ms=10.0 \
 --use_log_image=1 \
 --log_eps=0.1 \
 --contrast_threshold_pos=0.15 \
 --contrast_threshold_neg=0.15
```

If you get an error message: `Could not find ROS master`, open a new terminal, and start a `roscore` as follows: `roscore`.

#### Explanation of the parameters

  - `--data_source=2`: tells ESIM to read from a folder containing images. ESIM will look for an `images.csv` file describing the timestamps of the images to read.
  - `--path_to_data_folder`: path to the folder where the frames and the `images.csv` file are located.
  - `--path_to_output_bag`: the generated data (events and frames) will be saved into a rosbag at this location
  - `--ros_publisher_frame_rate`: frame rate of the simulated APS (frame) sensor (in frames per second).
  - `--exposure_time_ms`: exposure time of the simulated APS (frame) sensor (in milliseconds).
  - `--use_log_image=1`: tells ESIM to operate in the log-intensity domain. This means each input image will be converted to log, as follows: `L = ln(I / 255 + eps)`, where `eps` is set via the `--log_eps` parameter.
  - `--contrast_threshold_pos` and `--contrast_threshold_neg`: values of the positive (resp. negative) contrast threshold. A lower value means a higher sensitivity (more events). A higher value means a lower sensitivity (less events).

### Visualizing the simulated event data

After running ESIM as shown in the previous section, all the simulated data is saved in a rosbag (at `/tmp/out.bag` in our example).
To visualize the events, you can open a new terminal, and start the `dvs_renderer` node as follows:
```
rosrun dvs_renderer dvs_renderer events:=/cam0/events
```

Then, you can play the saved rosbag, in a loop (`-l`), and slowed down 10x (`-r 0.1`):

`rosbag play /tmp/out.bag -l -r 0.1`.

You can now visualize the simulated events by opening `rqt_image_view` (in another terminal): `rqt_image_view /dvs_rendering`.

Note: you can also visualize the simulated APS frames (including motion blur) by selecting the tab `/cam0/image_corrupted` from the list at the top of `rqt_image_view`.

