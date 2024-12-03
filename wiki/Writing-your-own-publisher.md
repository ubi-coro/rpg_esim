This page explains how to write your own publisher for ESIM.
This will enable you to control exactly how you want the simulated data to be saved on disk (events, images, camera poses, etc.).

As a simple example, we will write our own publisher, which will simply write all the simulated events into a text file (and ignore all the other measurements).

Here are the steps to follow:

- Write your own `TextFilePublisher` C++ class implementing the [`Publisher`](https://github.com/uzh-rpg/rpg_esim/blob/master/event_camera_simulator/esim_visualization/include/esim/visualization/publisher_interface.hpp#L8) interface by creating the two files below.

Header `esim_visualization/include/esim/visualization/text_file_publisher.hpp`:

```cpp
#pragma once

#include <esim/common/types.hpp>
#include <esim/visualization/publisher_interface.hpp>
#include <fstream>

namespace event_camera_simulator {

class TextFilePublisher : public Publisher
{
public:
  TextFilePublisher(const std::string& path_to_events_text_file, size_t num_cameras);
  ~TextFilePublisher();

  virtual void eventsCallback(const EventsVector& events) override; // will be called when new events are available

  static Publisher::Ptr createFromGflags(size_t num_cameras);


private:
  std::ofstream events_text_file_;
};

} // namespace event_camera_simulator
```

C++ file (`esim/visualization/src/text_file_publisher.cpp`):

```cpp
#include <esim/visualization/text_file_publisher.hpp>
#include <esim/common/utils.hpp>

#include <gflags/gflags.h>
#include <glog/logging.h>

DEFINE_string(path_to_events_text_file, "",
"Path to the text file in which to write the events.");

namespace event_camera_simulator {

TextFilePublisher::TextFilePublisher(const std::string& path_to_events_text_file, size_t num_cameras)
{
  CHECK_EQ(num_cameras, 1) << "TextFilePublisher implemented for one camera only";
  events_text_file_.open(path_to_events_text_file);
}

TextFilePublisher::~TextFilePublisher()
{
    events_text_file_.close();
}

Publisher::Ptr TextFilePublisher::createFromGflags(size_t num_cameras)
{
  if(FLAGS_path_to_events_text_file == "")
  {
    LOG(WARNING) << "Empty events text file path: will not write events to a text file.";
    return nullptr;
  }

  return std::make_shared<TextFilePublisher>(FLAGS_path_to_events_text_file,
                                             num_cameras);
}

void TextFilePublisher::eventsCallback(const EventsVector& events)
{
    CHECK_EQ(events.size(), 1);
    for(const Event& e : events[0])
    {
        events_text_file_ << e.t << " " << e.x << " " << e.y << " " << e.pol << std::endl;
    }
}

} // namespace event_camera_simulator
```

- Add these two files to the [CMakeLists.txt](https://github.com/uzh-rpg/rpg_esim/blob/master/event_camera_simulator/esim_visualization/CMakeLists.txt) in the `esim_visualization` package:

```
set(HEADERS
    ...
    include/esim/visualization/text_file_publisher.hpp
)

set(SOURCES
    ...
    src/text_file_publisher.cpp
)
```

- Register the new publisher in [this file](https://github.com/uzh-rpg/rpg_esim/blob/master/event_camera_simulator/esim_ros/src/esim_node.cpp), e.g. like this:
```cpp
  Publisher::Ptr my_publisher = TextFilePublisher::createFromGflags(data_provider_->numCameras());
  if(my_publisher) sim->addPublisher(my_publisher);
```

Do not forget to include the new header file at the top of `esim_node.cpp`:
```cpp
#include <esim/visualization/text_file_publisher.hpp>
```

- Now, recompile ESIM: `catkin build esim_ros`.

- You can now run ESIM, setting the path to the event file as a flag:
```rosrun esim_ros esim_node --flagfile=cfg/example.conf --path_to_events_text_file=/tmp/events.txt```

The events will now be written to the text file at: `/tmp/events.txt`!

### Next steps

Now that you know how to write your own publisher, try to implement the other callback functions (e.g. `image_callback()`, etc.) to save more measurements to disk!