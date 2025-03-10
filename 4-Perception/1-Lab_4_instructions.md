# ME 597- Lab 4

## Perception using TurtleBot4

In our final Lab, we will be learning about computer vision and how we can leverage it for low level control.

One of the key challenges in mobile robots is the perception problem, but what is perception? 
Perception consists of making sense of the unstructured real world, in other words converting the real world into a more manageable version of it so that robots can work more efficiently with it. 
Remember that robots have a limited computation power, thus selecting what is important becomes a challenge, and it is mostly driven by the application.
You have seen a popular sensor used for robot perception in previous laboratories- LIDARs; however, you have not used one of the most important ones: the camera.
In this laboratory, you will learn to use several libraries for image processing such as OpenCV. 
Although it does not have native support to inteface directly with ROS 2, `cvbridge` allows us to convert CV image objects into ROS 2 messages that can be exchanged between nodes in a ROS 2 application.
In this lab you will gain experience with a foundational computer vision library, OpenCV, and implement your computer vision algorithm to autonomously control a robot.

#### Words of advice:
* Whenever you're lost or have a doubt, Google it! Self-help will take you a long way in this course. A list of dependable and trustworthy resources (websites) is [here](../0-Setup/Resources/References.md).
* Students who make mistakes AND attempt to correct it will learn way more than those who finish the tasks without any errors/bugs.


# Instructions
For the third lab, your primary task will be learning image processing techniques from a camera feed, and using that to control a robot.
As part of this laboratory, you will utilize some algorithms for the robot to detect a moving object using the camera and to follow the object. 
This will allow the system to have a position reference system on top of which we can perform some control tasks. Specifically, you will complete 4 sub-tasks: 

task_5:
1. Image Publisher from recorded video
2. Image Subscriber and image processing
3. Object Detection Algorithm

Task_6:

4. Object Tracking Robot Control Algorithm


## Week 9
### Reading (10 min)
* Install OpenCV and cv_bridge from source according to instructions here: [OpenCV_installation.md](Resources/OpenCV_installation.md)
* Have a very quick read through and familiarize yourself with the basics of OpenCV: [OpenCV Docs - Python Tutorials](https://docs.opencv.org/4.x/d6/d00/tutorial_py_root.html)


### Tasks (2 hr)
`task_5`- Image data publisher and image processing subscriber
* Refer to the 'Playing Video from file' section from [OpenCV Docs - Getting Started with Videos](https://docs.opencv.org/4.x/dd/d43/tutorial_py_video_display.html)
* Use this resource for useful commands: [OpenCV_ROS2.md](Resources/OpenCV_ROS2.md)

#### Part A: Image Publisher from Recorded Video
1. Create a ROS 2 `ament_python` package called `task_5`
    * Specify cv_bridge dependencies like so: `ros2 pkg create --build-type ament_python YOUR_PACKAGE_NAME --dependencies rclpy image_transport cv_bridge sensor_msgs std_msgs opencv2` 
1. Write a simple publisher Python node, within this pkg, called `image_publisher.py`.
1. The `image_publisher.py` node must perform the following:
    * Load a recorded video file provided by your TAs, titled 'lab3_video.avi'. Use avi video format.
    * Save the lab3_video.avi to your `src/task_5/resource` folder
    * Publish the raw color image without any processing, using `Image` msg type via the `/video_data` topic.
1. In your setup.py, give your node the alias `image_publisher`.

#### Part B: Image Subscriber and Object Detection Algorithm
1. Write a subscriber Python node, within this pkg, called `object_detector.py`
1. The `object_detector.py` node must perform the following:
    * Subscribe to the `/video_data` topic.
    * Print the x and y position of the centroid of a detected object, relative to the top left corner of the frame (e.g.- top left is (0,0), bottom right is (w,h)), in pixels.
    * Print the width and height of the detected object, in pixels.
    * Publish these values to a topic called `/bbox` of type `/vision_msgs/BoundingBox2D`. Reference the [message definition source code](https://github.com/ros-perception/vision_msgs/blob/ros2/vision_msgs/msg/BoundingBox2D.msg)
       * If it's not available, install with `sudo apt install ros-humble-vision-msgs` 
       * You may also do: `ros2 interface show vision_msgs/msg/BoundingBox2D` to see the definition.
       * 'size_x', 'size_y' should be the width and height. 
    * Draw a rectangle (called a bounding box) around the detected object.
    * Display the edited video feed with bounding box in a new cv2 window.
1. To do this:
    * Note: You must come up with your own logic to recognize the object, and you are free to browse the internet for inspiration
    * HINT: One method is to mask the background, draw a contour around the isolated object, then get its center.
1. Write a `launch` file called `object_detector_launch.py` in Python to start up the image publisher and object detector nodes.
1. In your setup.py, give your node the alias `object_detector`.
1. Complete the relevant tag details in the `package.xml` file, build and run the ROS 2 nodes
1. Also verify with these commands:
    * $`ros2 node list`
    * $`ros2 topic list`
    * $`ros2 node info <node_name>`
    * $`ros2 topic echo <topic_name>`
## Week 10
Happy spring break! There is no lab this week, feel free to finish up your object detection algorithm and image subscriber from task_5 in order to prepare for next week's assignment.
## Week 11
This week you will control your robot in simulation to follow a red ball.
### Test the simulator (10 min):
1. Re-install sim_ws by following the updated instructions in [2-ROS_2_installation_guide.md](../0-Setup/2-ROS_2_installation_guide.md). You may delete your old sim_ws. For the time being, Mac users will need to use lab computers for robot simulation.
2. First, try launching the simulator with a red ball.
`ros2 launch turtlebot3_gazebo task_6.launch.py`
3. Next, try controlling the red ball. Use the wasd keys.
`ros2 run sim_utils red_ball_controller`

### Tasks (2 hr)

#### Part C: Track Red Ball
1. Create a ROS 2 `ament_python` package called `task_6`
2. Create an object tracker node for a red ball, called `red_ball_tracker.py`.
3. This node should subscribe to the robot's camera data, detect the ball, and publish wheel velocities to follow the ball.
4. The robot should have the following behavior:
    1. Ball is not visible: stop (baseline) or enter a search mode (extra credit - Part D).
    2. Ball is visible and far away: move towards the ball.
    3. Ball is visible and very close: move away from the ball.
    4. Turn to keep the ball in the center of the camera frame.
5. You should use a PID controller to control robot motion. Extra credit is available for more sophisticated controllers (see Part D).
6. In your setup.py, give your node the alias `red_ball_tracker`.

Hint: You should be able to reuse much of your work in Part B for the object detection component of this task.

Note: This part of the lab will be manually graded, so you must submit a video demonstrating the robot in action, in case there are any issues with running your code.

#### Part D (Extra Credit)
1. Implement more sophisticated logic for better tracking or searching performance. Better performance will earn up to 20 points extra credit.
    * Include a .txt file with a very brief description of any logic you included that you would like to be evaluated for extra credit, titled `extra_credit.txt`. 
    * Include 'extra_credit.txt' in both the video submission and source code submission. 
    * For extra credit, the search logic must go beyond rotating in place, and control logic must be more sophisticated than a PID controller (or any variant of P, PI, PD, etc.).

If you have completed all the tasks until here, you have done an amazing job! Pat yourself on the back because you have what it takes to become a robotics engineer.

### Deliverables
Video of Track Red Ball
* Demonstrate all 4 behavior requirements.
* Include 'extra_credit.txt' in zip file.

Source code
* Two ROS 2 packages:
    * `task_5`- Image data publisher and object detector 
    * `task_6`- Object tracker

ros2 log files
* Upload `<ws_ros2>/log/` too

Note: You do not need to upload bag files for this lab

Your folder structure should be as such:

```
XX
|
|__task_5
|     |__launch
|     |__...
|     
|__task_6
|     |__launch
|     |__...
|
|__'extra_credit.txt'
|
|__log
```

Where XX must be replaced with your roll-number.

### Rubric
Deviating from the names provided in the lab sheet will result in penalties.
* 5  pts: Correct node aliases.
* 20 pts: Week 9, Part A, `task_5` pkg
* 35 pts: Week 9, Part B, `task_5` pkg
* 40 pts: Week 11, Part C, `task_6` pkg
* +20 pts: Week 11, Part D, `task_6` pkg - Extra Credit
