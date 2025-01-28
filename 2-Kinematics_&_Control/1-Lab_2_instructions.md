# ME 597- Lab 2

## Kinematics and Control using TurtleBot4

For the second lab, your primary task will be learning how to control the kinematic motion of the TurtleBot4, using the linux terminal and employing the PID algorithm. The PID control algorithm is an imperative tool every roboticist must have handy and is applicable for many scenarios.


#### Words of advice:
* Whenever you're lost or have a doubt, Google it! Self-help will take you a long way in this course. A list of dependable and trustworthy resources (websites) is [here](../0-Setup/Resources/References.md).
* Students who make mistakes AND attempt to correct it will learn way more than those who finish the tasks without any errors/bugs.

# Instructions
This week you will implement a PID controller in simulation. You will use what you've learned from Lab 1 to create a single node that subscribes to lidar data at `/scan`, calculates the PID control output and publishes the velocity command to `/cmd_vel`. The system diagram will look like this:

<img src="Resources/images/PIDDistanceControllerDiagram.png" alt="PIDDistanceControllerDiagram.png" width="1000"/>

## Week 3
### Reading (20 min)
First, we need to prepare for the simulator:  
1. Setup TurtleBot4 packages - find the instructions [here](Resources/TurtleBot4_installation_guide.md)
2. Familiarize yourself with the TurtleBot4 Simulator

Next, we will take a look at the physical Turtlebot4, used next week (Week 5): 

3. Take a look at the specs of TurtleBot4 lite: [Turtlebot4Lite](https://turtlebot.github.io/turtlebot4-user-manual/overview/features.html#turtlebot-4-lite)
4. See how the TurtleBot4 will communicate with your PC. We will use the Discovery Server configuration to facilitate multiple robots on the same network and allow use with a Virtual Machine: [Turtlebot4 Networking](https://turtlebot.github.io/turtlebot4-user-manual/setup/networking.html) 
5. Familiarize yourself with the suite of sensors available on the TurtleBot4 from [Turtlebot4 Sensors](https://turtlebot.github.io/turtlebot4-user-manual/software/sensors.html).
 (You won't have to install or run any of these packages, they are pre-installed and are run automatically by the robot upstart service)

PLEASE NOTE:

6. You may only access the physical Turtlebot4s at lab time during your section each week! To prepare for next week, make sure your PID code works in simulation.
7. Although implementation on the physical robot is a grouped task, the simulator PID controller is still an individual assignment and your submitted code should be unique to you. For the physical robot PID controller, a team may use either one of the partners' code.

### Tasks (2 hr) 

`task_3`- PID distance controller using lidar data to allow the TurtleBot4 to stop 'x' meters from an obstacle directly in front of it.
  
#### Part A: Creating the Package and Node
1. Create a ROS 2 `ament_python` package called `task_3`
2. Create a simple Python node within this pkg, called `pid_controller.py`
3. Create a launch file called ```pid_control_launch.py``` that launches your pid controller node, place it in task_3/launch/ and configure setup.py to access that launch file. 
4. Setup up the following alias in setup.py
    - pid_speed_controller := PID Controller Node
#### Part B: Subscribe to the `/scan` topic
5. Create a subscriber that reads the `/scan` topic. 
    - Set the rate to 10Hz to define the execution speed of your node. You can tune this parameter - however, setting it too high will cause unnecessary load.
6. Get the forward-facing distance reading from lidar.
    - Check out the `sensor_msgs/LaserScan.msg` [message definition](http://docs.ros.org/en/api/sensor_msgs/html/msg/LaserScan.html). 
    - hint: you will want to use the `float32[] ranges` attribute, which is a list of range values, and for the Turtlebot3 these correspond to:
      
      <img src='Resources/images/TB3_laserscan.png' alt='TB3_laserscan.png' width=1000/>

#### Part C: Calculate the PID Controller
7. In the callback function, use forward distance measured from the lidar sensor as input to a PID controller to control the velocity.
    - Set the target distance to 0.35 m.
    - Tune your `Kp`, `Ki`, and `Kd` values as you wish.
    - You will be graded on meeting a specified peak time and then higher grades will be given to lower percent overshoot. (Specific grade cutoffs will be posted later).
    - **Note for full credit you must use all three gains (i.e a P, PI, or PD controller will not recieve credit)**
   


#### Part D: Publish to the `/cmd_vel` topic
8. In the same node file, create a publisher that writes to the `/cmd_vel` topic to move the robot. [Reference](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/driving.html#command-velocity)
    - The upper bound for `linear.x` must be 0.15 m/s and the lower bound for `linear.x` must be -0.15 m/s.
    - See the `geometry_msgs/Twist` [message definition](http://docs.ros.org/en/melodic/api/geometry_msgs/html/msg/Twist.html)


#### Part E: Verification
9. Complete the relevant tag details in the `package.xml` file, build and run the ROS 2 node along with gazebo.

    - Launch the world and spawn a turtlebot3 using:
      ```
      ros2 launch turtlebot3_gazebo turtlebot3_house_norviz.launch.py
      ```
    - Don't forget to source your sim_ws first:
      ```
      source ~/path/to/sim_ws/install/local_setup.bash
      ```

10. You may also verify with these commands:
    * $`ros2 node list`
    * $`ros2 topic list`
    * $`ros2 node info <node_name>`
    * $`ros2 topic echo <topic_name>`


### Week 4

#### Part A: Connect to Robot via ROS2 (Group)

1. With your lab partner, set up communication with the Turtlebot4 via ROS2 on your PC. Follow the instructions here: [5-Turtlebot4_use.md](Resources/5-Turtlebot4_use.md). If you have issues, ask for help.

2. Now that you have connected to the robot with your PC via ROS2 (not ssh), you are able to subscribe and publish to the robot's topics. Run this in your terminal to manually publish to the robot's `/cmd_vel`:

    ```ros2 topic pub /robot/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 0.1, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.0}}"```

#### Part B: PID Implementation on Physical Robot (Group)
3. The Turtlebot3 and Turtlebot4 are very similar, however, there are tw`o small changes you will need to make to convert your code from being used on the Turtlebot3 to the Turtlebot4.
    1. The `/robot` namespace: You need to modify your publisher and subscriber initializations to use this namespace for their topics.
      
        What is a namespace? In general, a namespace simply acts as a prefix for nodes, parameters, topics, and other interfaces in the system. Namespaces are often used to differentiate multiple robot's data on the same network. For example, when you do `ros2 topic list` you may have noticed that all the topics are in the form `/robot/<topicname>`. In this case, they are all in the 'robot' namespace
    
        Although namespaces are typically optional, in the current version of the Turtlebot4 software, namespaces are required for it to function properly (for reasons you can ignore). Your robot's namespace was arbitrarily set to 'robot'. Any other string would have worked just as well ('my_robot', 'robot1', 'turtle', etc). Since we chose 'robot', all of the topics have the prefix `/robot`, and you must reflect that in your publisher and subscriber.
    1. Laserscan angles: In your turtlebot3 simulator implementation, you used a specific index of the 'ranges' attribute - `ranges[index]`. The forward facing index is different for the Turtlebot3 and the Turtlebot4. For the Turtlebot4, you can either determine it from these resources: [RPLidar Frame](https://github.com/allenh1/rplidar_ros/blob/ros2/rplidar_A1.png) & [Laserscan message definition](http://docs.ros.org/en/api/sensor_msgs/html/msg/LaserScan.html); or trial and error.
3. Run your PID node on your PC to control the Turtlebot4.
4. Record a video of your PID code running on the robot and submit it on Gradescope.

#### Part C: Inverse Kinematics Derivation (Individual)
5. Inverse Kinematics: This is a written assignment. You may find this assignment on Brightspace. You will submit it on Gradescope.

### Deliverables
Each of the 3 deliverables will have a separate submission:
#### 1. Kinematics Derivation
* Solution to the inverse kinematics derivation problem. May be handwritten. There will be a separate submission for this on Gradescope.

#### 2. Source code
Please Note: All individuals must have unique PID source code for the simulation assignment.

Your ROS 2 package:
  * `task_3`- PID controller to help the TurtleBot4 stop 0.35 m from an obstacle

ros2 bag files:
* You will 'record' the data passing through your topics via `ros2 bag`:
  * in any directory outside from your workspace directory, run:
    * $`mkdir bag_files`
    * $`cd bag_files`
    * $`ros2 bag record -o task_3 /scan /cmd_vel`
  * NOTE: your topic must be alive for ros2 to record it
  * Use this as [reference](https://docs.ros.org/en/galactic/Tutorials/Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.html)

ros2 log files:
* Upload `<ws_ros2>/log/` too

Your folder structure should be as such:

```
42
|
|__bag_files
|     |__task_3
|         |__metadata.yaml
|         |__task_3_0.db3
|            
|__task_3
|     |__launch
|     |__resource
|     |__...
|     
|__log
```

Where 42 must be replaced with your roll-number.

#### 3. Video of Physical Robot Implementation
Partners may submit the same video, however, both partners must submit a video. You do not need to make a source code submission for the physical robot implementation. There is no late submission for this assignment.

### Rubric
Deviating from the names provided in the lab sheet will result in penalties.
* 60 pts: Week 3, `task_3` pkg: All three gains must be non-zero for credit. Grading is scaled based on achievable OS% and meeting peak time criteria.
* 25 pts: Week 4, Inverse Kinematics solution
* 15 pts: Week 4, Video of Successful Physical Robot PID (Group - but both individuals need to submit the video)
