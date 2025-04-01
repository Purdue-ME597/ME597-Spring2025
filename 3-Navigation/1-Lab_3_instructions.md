# ME 597- Lab 3

## Autonomous Navigation using TurtleBot4

For the third lab, your primary task will be learning and implementing Simultaneous Localization And Mapping (SLAM) and Navigation techniques using the TurtleBot4.

### Introduction
Mobile robots are used in different environments to perform a wide range of tasks such as cleaning, patrolling, and rescue operations. In order to perform these tasks, the robots must move freely and autonomously through either static or dynamic environments. This process is navigation and it allows a robot to move through the current environment while performing tasks according to predefined rules. Examples of these rules include obstacle avoidance, cost minimization, and driving comfort.

In order to navigate, a mobile robot must gather information about the environment it is in. A common way to generate a map is with simultaneous localization and mapping (SLAM). The map can either be generated before a robot starts navigation (graph-based algorithms) or can be generated as the robot navigates an unknown environment (sampling-based algorithms). In this lab, we will be using an graph-based algorithm.

Combined mapping and navigation allow robots to move through a predefined path, to adjust its trajectory due to changes in the environment, or to correct motion online due to measurement errors. It is also important to consider extreme cases in which the robot gets lost or crashes into something, by implementing recovery strategies that bring the robot back to a safe known state. 

#### Words of advice:
* Whenever you're lost or have a doubt, Google it! Self-help will take you a long way in this course. A list of dependable and trustworthy resources (websites) is [here]([(https://github.com/Purdue-ME597/ME597_Private/blob/main/0-Setup/Resources/References.md)]).
* Students who make mistakes AND attempt to correct it will learn way more than those who finish the tasks without any errors/bugs.

### Instructions
In this lab, your primary task will be to implement SLAM and navigation capabilities in simulation and hardware. This includes 4 sub-tasks:

Task_4:
1) Mapping
2) Localization
3) Planning
4) Execution 

This will be a three-week lab, with the following tasks each week:
* Week 1: Mapping (hardware), mapping (simulation), A*
* Week 2: Navigation (simulation)
* Weeks 3/4: Navigation (hardware)


Please note that all hardware tasks should be done with your partner. All other tasks must be done individually. Solutions may be shared for partnered work, but individual work must still be unique.

**Please do not use preexisting navigation packages like Nav2.**

### Week 5
This week, you will begin by generating a map using SLAM and implementing an A* algorithm for path planning.

#### Part A: Mapping (Hardware) (1 hr) (Partnered)
* `task_4`- Generating map data

NOTE: The robot used in this class has a namespace of `/robot`, so when you are launching a file, remember to include the namespace argument, e.g. `ros2 launch task_4 gen_sync_map_launch.py namespace:=/robot`

Using [this](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/generate_map.html) as reference:
1. Create a ROS 2 `ament_python` package called `task_4`, with two sub-folders: `maps` and `launch`.
2. Write a launch file called `gen_sync_map_launch.py` which performs the following:
    * Launch the `slam.launch.py` file (which belongs to `turtlebot4_navigation` pkg) from within this launch file to start SLAM.
    * Launch the `view_robot.launch.py` file (which belongs to `turtlebot4_viz` pkg) from within this launch file to view the map in Rviz2.
3. Complete the relevant tag details in the `package.xml` file, and build and run the ROS 2 node.

In a separate terminal, run the `teleop_twist_keyboard` node to teleoperate the TurtleBot4:
`ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args --remap cmd_vel:=/robot/cmd_vel`

4. Also verify with these commands:
    * $`ros2 node list`
    * $`ros2 topic list`
    * $`ros2 node info <node_name>`
    * $`ros2 topic echo <topic_name>`
5. Once you're satisfied with clarity of the generated map, manually [save the map](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/generate_map.html#save-the-map) as `classroom_map`, before killing `slam.launch.py`.

NOTE: Again, if the saving fails, that would be caused by lack of namespace, so use this command instead: `ros2 run nav2_map_server map_saver_cli -f "map_name" --ros-args -p map_subscribe_transient_local:=true -r __ns:=/robot`.


#### Part B: Mapping (Software) (10 min) (Individual)

Now we will map in simulation.

1. Follow the instructions [here](https://github.com/naslab-projects/sim_ws.git) to use the simulation environment. Note the specific launch instructions in the `src/turtlebot3_gazebo` subdirectory.
2. Manually map the `turtlebot3_house` world:
   * Launch the `mapper.launch.py` file (which belongs to `turtlebot3_gazebo` pkg). 
   * Run the `teleop_twist_keyboard` node (which belongs to `teleop_twist_keyboard` [pkg](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/driving.html)) from within this launch file to teleoperate the TurtleBot4.
3. Once you're satisfied with clarity of the generated map, manually [save the map](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/generate_map.html#save-the-map) as `sync_classroom_map`, before killing `mapper.launch.py`. 
4. Copy and paste the map files to `turtlebot3_gazebo/maps`


#### Part C: Path Finding Algorithms (1 hr) (Individual)

From the prior part, you should have 2 map files, one containing an image of the map(`*.pgm`)and another one containing meta-data of the map(`*.yaml`). Note: We have also provided 2 map files on Brightspace that you may use this week instead.

You will need these two files in order to implement your own A* algorithm. Go to the course site on Brightspace, and download the Jupyter Notebook. 

You will need to upload this file into [Google Colab](https://colab.research.google.com/) to work with it. Go through the jupyter notebook and implement the A* algorithm for the map obtained during previous tasks with the navigation stack.

Take this opportunity to learn how the BFS, Dijkstra, and A* algorithms are implemented. 

### Week 6: Navigation, Part I
In this lab, you will perform navigation in simulation, in preparation for hardware implementation next week.

* `task_4`- Autonomous navigation in a known environment

We will continue developing with the `task_4` `ament_python` ROS2 pkg developed in Week 5, Part B.

The task is to 
1. get the goal pose, 
2. use the A* algorithm previously created, and 
3. implement a path-follower to drive the vehicle through the path. 

The primary focus of this task should be the logic of the path planning and path follower. It is assumed that the path is free of obstacles.
 
#### Tasks: Simulation (2 hr) (Individual)
In this part of the lab, we will work with the robot in simulation. This allows you to test functionality before working with the robot in a field environment.

1. Within the `task_4` package, create a Python node called `auto_navigator.py`. We have provided a skeleton code to help you get started. The `auto_navigator.py` node must do the following:
        
    1. Implement the path planning and path following logic to move the TurtleBot4 from position A to B.
    2. Maintain a record of the initial, goal, and current TB4 pose.
    3. Perform the A* path planning logic and a path follower logic which moves the TB4 through the generated path.
        
        *The path follower logic must make sure the TB4 passes through the waypoints with an acceptable error threshold for the heading and speed vectors.*

**Do not use preexisting packages like Nav2 to implement this node.**

Please give this node the alias `auto_navigator.py` in your `setup.py` file to start up the auto-navigator node. You will use this node both in simulation and in hardware (the launch files will be different).

Compared to other nodes, `auto_navigator.py` is predicted to have a relatively high number of lines of code. We recommend breaking your logic into subfunctions and following good coding styles.

**Bonus Points**
1. We will give you a long path to test your robot, we will assign bonus points to those who are able to increase the performance of your A* algorithm computation speed. This is measured for calculating the path from the robot spawn to the back of the room in the top right corner in RVIZ.

2. There are various methods you can do to improve the A* performance, one way is to replace the queue handling with implementing the heapq library in python.
   * 50-90 seconds = 5 points
   * 20-50 seconds = 10 points
   * 5 - 20 seconds = 15 points
   * < 5 seconds = 20 points
   * >90 seconds = 0 points
After completing this node, 
**Testing your code (assignmen check off points)**
2. Follow the instructions [here](https://github.com/naslab-projects/sim_ws.git) to use the simulation environment. Note the specific launch instructions in the `src/turtlebot3_gazebo` subdirectory.
3. Launch the `navigator.launch.py` file (which belongs to `turtlebot3_gazebo` pkg). 
3. Test the `auto_navigator.py` by pick a [target pose in RVIz](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/navigation.html). Ensure it generates a path to the goal and navigates towards it.
4. Record a video showing your autonomous navigation going to a desired position.
4. Record the `/scan` and `/cmd_vel` topics using rosbag (see Deliverables section at end of document).

### Weeks 7/8: Navigation, Part II

Last week, you implemented Turtlebot navigation capabilities in simulation. This week, you will implement it on the physical robot and demonstrate navigation capabilities in hardware.

#### Tasks: Hardware (2 hr) (Partnered)

You will use the [`auto_navigator.py`](/3-Navigation/Resources/auto_navigator.py) node from last week (Week 6, Part A, Step 1). However, this week, you will add it to a launch file to be used to navigate on hardware with the physical robot. 
Before writing the launch file create the folder `rviz` in your task_4 ros package and put [`robot.rviz`](/3-Navigation/Resources/robot.rviz) inside. Include the [`view_robot.launch`](/3-Navigation/Resources/view_robot.launch.py) file in your launch folder as well. Make sure to add 
the following in your `setup.py` under data_files.
```python
(os.path.join('share', package_name, 'rviz'), glob(os.path.join('rviz', '*'))),
(os.path.join('share', package_name, 'maps'), glob(os.path.join('maps', '*')))
```


1.  Write a launch file called `turtlebot4_navigator.launch.py` which performs the following:
          
    1. Launch the `localization.launch.py` file (which belongs to `turtlebot4_navigation` pkg).
        * This is for localization.
        * You must also pass the relative path for the `map` launch argument, which should be `src/task_4/maps/classroom_map.yaml`. You may also pass the absolute path to your map.
        * You must also pass `/robot` for the `namespace` launch argument.
        * For debugging purposes, you can try launching this file on its own with: `ros2 launch turtlebot4_navigation localization.launch.py map:=classroom_map.yaml namespace:='/robot`
   
    
    2. Launch the `view_robot.launch.py` file (which belongs to `task_4` pkg).
        * This allows you to view the map in Rviz2.
        * You should pass the map path as well for the `map` launch argument.
        * You should pass `/robot` for the `namespace` launch argument.
    
    3. Run the `auto_navigator` node (which belongs to `task_4` pkg).
        * This starts your path planning and path following algorithm for the TurtleBot4.
        * You will need to add `/robot` to the following topics: `/cmd_vel`,`/amcl_pose`
    4. [Optional] If you are struggling to get your launch file to work please use this instead: 
        - [`turtlebot4_navigator.launch.py`](/3-Navigation/Resources/turtlebot4_navigator.launch.py) (can be found in `3-Navigation/Resources`)

2. Complete the relevant tag details in the `package.xml` file.

3.  Build and run the ROS 2 launch file you built (or the provided launch file from the Resources folder).
    * Observe the robot's path through the environment.
    * Also verify with these commands:
        * $`ros2 node list`
        * $`ros2 topic list`
        * $`ros2 node info <node_name>`
        * $`ros2 topic echo <topic_name>`
4.  To get your hardware working, set the robot down in the map. Launch the file to start the amcl and Rviz. Set your initial pose to match your physical robot position. You may notice the lidar scan does not match up perfectly with the walls of the map, in this case adjust your iniital pose in Rviz or move the physical robot slightly until they align (this does not need to be perfect but should be close). At this point, you can set your goal position in Rviz and watch your little robot go!
5. Once the robot can navigate successfully, record a video of the robot navigating. You will upload this to Gradescope later.

Note: it is assumed the path is free of obstacles. However, if your robot keeps colliding into the walls, you might need to increment the safe margin with respect to the walls or improve your map accuracy. This map is challenging and will require a delicate balance of map inflation, robot speed, and initial pose estimate accuracy to traverse the space.

### Deliverables
Source code
* One ROS 2 package:
    * `task_4`- Generating map data + TB4 path planning and following

ros2 bag files (Week 6)
* You will 'record' the data passing through your topics during simulation via `ros2 bag`:
    * in any directory outside from your workspace directory, run:
        * $`mkdir bag_files`
        * $`cd bag_files`
        * $`ros2 bag record -o task_4 /scan /cmd_vel`
    * NOTE: your topic must be alive for ros2 to record it
    * Use this as [reference](https://docs.ros.org/en/galactic/Tutorials/Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.html)

ros2 log files
* Upload `<ws_ros2>/log/` too

Video footage
* Recording of your robot autonomously navigating the simulation space. Please include the set up you do in RVIZ as part of the video.
* Recording of your robot autonomously navigating the physical space. Please include the set up you do in RVIZ as part of the video.

Your folder structure should be as such:

```
XX
|
|__bag_files
|       |__task_4
|            |__metadata.yaml
|            |__task_4_0.db3
|            
|__task_4
|     |__launch
|     |__maps
|     |__...
|
|__log
|
|__navigation_astar_f24.ipynb
```

Where XX must be replaced with your roll-number.

### Rubric
Deviating from the names provided in the lab sheet will result in penalties.
* 45 pts: `task_4` pkg
* 05 pts: Week 5, Part A, map files
* 10 pts: Autonomous Navigation Simulation Video
* 25 pts: Week 6, Part C, completed and commented `*.ipynb` NB
* 05 pts: Week 6 `ros2 bag` files
* 10 pts: Weeks 7/8, Physical Turtlebot Video recording
* 20 pts bonus: Improved calculation time of A* algorithm.
