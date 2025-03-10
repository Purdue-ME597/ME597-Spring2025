# Using OpenCV in ROS2
Below are some OpenCV commands you may find particularly useful for this lab. As always, you are welcome to look online for more information regarding using these commands. 

* [OpenCV Docs - Python Tutorials](https://docs.opencv.org/4.x/d6/d00/tutorial_py_root.html)
* [OpenCV Docs - Smoothing Images (Median Blurring)](https://docs.opencv.org/4.x/d4/d13/tutorial_py_filtering.html)
* [OpenCV Docs - Getting Started with Videos (Playing Video from file)](https://docs.opencv.org/4.x/dd/d43/tutorial_py_video_display.html)

## Creating a package that will use OpenCV
When you create your package, you must specify opencv as part of the dependencies:

```
cd ~/your_ws/src
ros2 pkg create --build-type ament_python pkg_name --dependencies rclpy image_transport cv_bridge sensor_msgs std_msgs opencv2
```

## Use the cv_bridge functions:

### Imports:
```
from sensor_msgs.msg import Image # To use a ROS2 image message
from cv_bridge import CvBridge    # Convert ROS2 <--> cv2 image type
import cv2                        # OpenCV

# rest of your imports
```

### Initialize:
```
class ImagePublisher(Node):
    def __init__(self):
        self.bridge = CvBridge()

        # rest of your code
```

### Convert a cv2 image to a ros2 image:
```ros2_img = self.bridge.cv2_to_imgmsg(cv2_img)```

### Convert a ros2 image to a cv2 image:
```cv2_img = self.bridge.imgmsg_to_cv2(ros2_img)```

## OpenCV Video Capture Object:

### Load the video as a video capture object:
```self.cap = cv2.VideoCapture('path_to_your_video.avi')```

### Get a frame:
```success, frame = self.cap.read()```

`frame` is now the cv2 image (a numpy array) we want to work with. 

`success` is a flag that equals true if the frame was successfully obtained. 

### Release the video capture object when you're done using it:
```self.cap.release()```

## Using cv2.imshow():
### Display an image frame:
```cv2.imshow('Window Name', frame)```

Note: Calling this when no window of name 'Window Name' is open will create a new window. Calling this when a window of name 'Window Name' already exists will simply update the frame.

### Destroy all windows:
```cv2.destroyAllWindows()```


### Wait x ms for a keypress and update cv objects:
```cv2.waitKey(x)``` 

### Below is a recommended code snippet to display a video feed in a node:
```
def __init__(self):
    self.show_video = True
    # rest of init code

def some_looping_or_callback_function(self):
    # rest of code to get 'frame'

    if self.show_video:                         # Custom flag to enable or disable video display below
        cv2.imshow('Window Name', frame)        # Display frame image in 'Window name'
        if cv2.waitKey(1) & 0xFF == ord('q'):   # Refresh window and check for keypress of `q` 
            self.show_video = False             # Set custom flag to False to disable video display 
            cv2.destroyAllWindows()             # Close all cv2.imshow windows
```


## ROS2 Tips

### Display ROS2 image topics in real time:
Use command `rqt` to open the rqt window. Then go to plugins -> Visualization -> Image View, to open the image topic viewing window. Then, select the topic you want to view from the dropdown. This is useful to view image topics, because ros2 topic list will not show the image.

