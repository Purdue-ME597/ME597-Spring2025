# Installing OpenCV and cv_bridge in ROS2

## Information
[OpenCV (Open Source Computer Vision Library)](https://opencv.org/) is an open-source computer vision library, useful for image and video processing. This library provides tools such as object detection algorithms, facial recognition algorithms, and more. It also provides basic functionality for tasks such as reading images or videos from files, displaying video streams, and applying transformations such as filters or edge detection.

This tooling and functionality makes OpenCV a fundamental resource for robotics perception tasks. The only drawback we need to work around is the differing formats between OpenCV images and ROS2 images. When we use OpenCV, it must be in OpenCV image format, but when we publish and subscribe to image topics, we must use the ROS2 image format. To convert an image from one format to the other, we will use the package `cv_bridge`.

<img src="images/OpenCVROS.png" alt="opencv_ros" width=350/>

`cv_bridge` belongs to a repository `vision_opencv` which contains:

* `cv_bridge`: Bridge between ROS 2 image messages and OpenCV image representation
* `image_geometry`: Collection of methods for dealing with image and pixel geometry
* `opencv_tests`: Integration tests to use the capability of the packages with opencv
* `vision_opencv`: Meta-package to install both cv_bridge and image_geometry


# Follow these instructions to install OpenCV and cv_bridge for ROS 2

In a terminal, install these packages:

* `sudo apt install python3-numpy`
* `sudo apt install libboost-python-dev`
* `sudo apt install python3-opencv`

Fetch the latest code and build:
* `cd <YOUR_ROS2_WORKSPACE>/src`
* `git clone https://github.com/ros-perception/vision_opencv.git -b rolling`
* `cd ..`
* `colcon build --symlink-install`

### Using a package installed from source
Downloading the source code and manually building it is called installing from source. Which is what you just did in the second portion above. For OpenCV to continue to work with ROS 2, this package (`vision_opencv`) must NOT be deleted and you must build and/or source this workspace before attempting to work with `cv_bridge` again.

If you deleted the `<YOUR_ROS2_WORKSPACE>/build` and/or `<YOUR_ROS2_WORKSPACE>/install` folder, you must:
* $`cd <YOUR_ROS2_WORKSPACE>`
* $`colcon build --symlink-install`
* $`source install/setup.bash`

