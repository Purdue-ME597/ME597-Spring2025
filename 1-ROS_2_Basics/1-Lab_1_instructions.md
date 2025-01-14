# ME 597- Lab 1

## ROS 2 Basics

For the first lab, your primary task will be learning what are the different ways ROS 2 nodes can be defined to talk to each other, using the linux terminal. It is suggested to progress through the lab sheets with this recommended pace, but feel free to go faster if you feel so.

#### Words of advice:
* Whenever you're lost or have a doubt, Google it! Self-help will take you a long way in this course. A list of dependable and trustworthy resources (websites) is [here](../0-Setup/Resources/References.md).
* Students who make mistakes AND attempt to correct it will learn way more than those who finish the tasks without any errors/bugs.

# Instructions

## Week 1
Understanding ROS 2 nodes, topics, services, parameters and actions. This week will be primarily following official ROS2 tutorials and documentation. You aren't expected to be an expert on every concept at the end of the week, rather, you should have a general understanding and reference these materials again later.
### Tasks (2 hr)
#### Isolate ROS2 Communications
Before we start anything, make sure your `ROS_DOMAIN_ID` is set to your unique roll number:
  1. Open a terminal and do `nano ~/.bashrc` to begin editing the `.bashrc` file.
  2. Add this line to the end of the file, replacing XX with your unique roll number: `export ROS_DOMAIN_ID=XX` 
  
  3. Verify this worked by opening a new terminal and enter: `echo $ROS_DOMAIN_ID` 
      * It should print your roll number. If nothing is printed, make sure you did this step in a new terminal and double check the changes were correct in your `.bashrc`.
      * Close out of both terminals.

Additionally, `echo $ROS_LOCALHOST_ONLY` should be 1. If it is not, do `echo "export ROS_LOCALHOST_ONLY=1" >> ~/.bashrc`

These steps are important. If you do not do this, then you may broadcast your ROS2 communications to everyone on the network who has not done this and your communications will interfere.

#### ROS2 Basics Tutorials
* You will follow and work your way through this [tutorials list](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools.html). Stop at "[Understanding actions](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Actions/Understanding-ROS2-Actions.html)" (inclusive)
* If you have time, you may continue reading `1-ROS_2_fundamentals_1.md` and `3-ROS_2_fundamentals_2.md`. These are additional readings that will provide foundational understanding of how ROS 2 works.

The only deliverables for the first week were the Lab 0 deliverables; however, you may advance to next week's assignments.

## Week 2-1
This week you will begin writing your own code to create your first ROS2 python package. You will use the concepts of nodes and topics, which you learned last week, to create a simple subscriber and a simple publisher. You will create a launch file to run these nodes. 

### Reading (20 min)
* Understanding the ROS 2 debugging tools- [rqt_console](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Using-Rqt-Console/Using-Rqt-Console.html) and [ros2 bag](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.html)
* Understanding ROS 2 launch files
  * Learn the concept: [Launching Nodes](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Launching-Multiple-Nodes/Launching-Multiple-Nodes.html)
  * Learn how to implement the launch files: [Launch File Formats](https://docs.ros.org/en/humble/How-To-Guides/Launch-file-different-formats.html).

* Read through these 2 resources on how to create a ROS 2 workspace and Python package: [Creating a Package](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html) & [Writing a Publisher & Subscriber](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Publisher-And-Subscriber.html).
* Now you're ready to write your first ROS 2 package!

### Tasks (1 hr 40 min)

Refer to the readings you completed above to complete the tasks:

`task_1`- ROS 2 `ament_python` package with custom publisher, subscriber and launch script
#### Part A: Create a Package
  1. Create a ROS 2 `ament_python` package called `task_1`
  
#### Part B: Basic Publisher

  2. Write a simple ROS 2 publisher node in Python, within this pkg, that publishes how long this node has been active, in seconds. Name this topic `my_first_topic` and choose an appropriate msg data type from `std_msgs.msgs`: [std_msgs api](https://docs.ros2.org/galactic/api/std_msgs/index-msg.html).

  3. Pick a frequency between 1 Hz and 20 Hz. Give this script an appropriate name and place it in `task_1/task_1/`

#### Part C: Basic Subscriber
  4. Write a simple ROS 2 subscriber node in Python, within this pkg, that prints out 2x the quantity of the data received. Make sure to print both the original and doubled value in the same message. Please only log the subscriber data for submission. Give this script an appropriate name and place it in `task_1/task_1/`
  5. Run the publisher and subscriber nodes, in different terminals, to verify everything works as intended.
  6. Also verify with these commands:
      * $`ros2 node list`
      * $`ros2 topic list`
      * $`ros2 node info <node_name>`
      * $`ros2 topic echo <topic_name>`
#### Part D: Launch File
  7. Write a launch file, that starts both the publisher and subscriber node.  Hint: it's just 4 lines of code. Use these: [Creating Launch Files](https://docs.ros.org/en/humble/Tutorials/Intermediate/Launch/Creating-Launch-Files.html) & [Launch System](https://docs.ros.org/en/humble/Tutorials/Intermediate/Launch/Launch-system.html) (follow the Python launch file method). Name this file `pub_sub_launch.py` and place it in `task_1/launch/`. You can create this directory using the `mkdir` command. 
      - In your ```setup.py```, make sure the following aliases are used for your nodes.
          - talker := Publisher node
          - listener := Subscriber node 
      - Here's an example of my_node.py in the my_py_pkg ROS package being given the alias "my_node". Here a link to the official source: [Customizing package.xml](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html#customize-package-xml)
        ```python
        from setuptools import setup

        package_name = 'my_py_pkg'

        setup(
        ...
        entry_points={
            'console_scripts': [
                    'my_node = my_py_pkg.my_node:main'
            ],
          },
        )
        ```
  8. Run the launch file to verify everything works as intended.
      * Tip: Lots of bash (including ros2) commands you can tab-complete, this can be helpful for running launch files. Just make sure you've configured your setup.py correctly, built, and sourced your workspace.

#### Part E: Submission
  9. Comment your code, save your work, record your ros2 bag files, and submit before the start of the next Lab session.


## Week 2-2
This week, you will:
  * Write a ROS 2 `ament_cmake` package for your own custom interfaces
  * Write a ROS 2 `ament_python` package with:
    * A simple publisher and subscriber node using your custom `msg` 
    * A simple service server and service client node using your custom `srv`

### Reading (20 min)

Creating a simple ROS 2 service server and service client package.
  * Learn how to write a custom msg or srv file here: [Custom ROS2 Interfaces](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Custom-ROS2-Interfaces.html)

### Tasks (1 hr 40 min)
Refer to the readings you completed above to complete the tasks:

 `task_2`- ROS 2 `ament_python` package with custom service server and service client node exchanging via a custom message and service interface

#### Part A: Creating custom `msg` and `srv` interfaces
  1. Create a new ROS 2 `ament_cmake` package called `task_2_interfaces`
      * Hint: you may have to install catkin_pkg, empy, and/or lark: `pip install catkin_pkg empy==3.3.4 lark`
  2. Write a custom `.msg` file called `JointData.msg` under the `task_2_interfaces/msg` folder. You can create this directory using the `mkdir` command
      * The fields of the `JointData.msg` will be:
        * `geometry_msgs/Point32 center`
        * `float32 vel`    
  3. Write a custom `.srv` file called `JointState.srv` under the `task_2_interfaces/srv` folder. You can create this directory using the `mkdir` command
      * The request fields of the `JointState.srv` will be:
        * `float32 x`
        * `float32 y`
        * `float32 z`
      * The response fields of the `JointState.srv` will be:
        * `bool valid`
#### Part B: Configure and build your cmake package
  4. You will use the resource above to figure out how to complete the `CMakeLists.txt` file and the `package.xml` to successfully `colcon build` this package

#### Part C: Creating publisher/subscriber nodes using your custom `msg`
  5. Create a new ROS 2 `ament_python` package called `task_2`
  6. Write a simple publisher & subscriber node that communicate via a topic called `joint_topic` and the topic type will be `JointData`
      * This publisher node simply needs to publish any valid data and the subscriber node should simply log this published data
      * Note that `JointData.msg` was defined by you in `task_2_interfaces/msg`, use this as your msg type
#### Part D: Creating service server/client nodes using your custom `srv`
  7. Write a simple service server & service client node that communicate via a service called `joint_service` and the service type will be `JointState`
      * Note that `JointState.srv` was defined by you in `task_2_interfaces/srv`, use this as your srv type
      * The response of the service must be `True` if the sum of the request field is positive or zero
      * The response of the service must be `False` if the sum of the request field is negative
#### Part E: Configure and build your python package
  8. You will use the resource above to figure out how to complete the `setup.py` file and the `package.xml` to successfully `colcon build` this package    

#### Part F: Verify it works
  9. Run the publisher & subscriber nodes and service & client nodes, all in different terminals, to verify everything works as intended.
      * Also verify with these commands:
        * $`ros2 node list`
        * $`ros2 topic list`
        * $`ros2 node info <node_name>`
        * $`ros2 topic echo <topic_name>`
        * $`ros2 service list`
        * $`ros2 interface show task_2_interfaces/msg/JointData`

#### Part G: Launch File
  10. Write a launch file, that starts the Service and Publisher node. Name this file `service_launch.py` and place it in `task_2/launch/`. You can create this directory using the `mkdir` command. 
      - In your ```setup.py``` make sure the following alias are used for your nodes.
          - talker := Publisher Node
          - service := Service Node
          - client := Client Node  
  11. Run the launch file and the client node to verify everything works as intended.


#### Part H: Submission
  12. Comment your code, save your work, record your ros2 bag files, and submit before the start of the next Lab session.

You should be very proud if you have completed all the tasks until here! At this point you should have a good understanding of all the ROS2 fundamentals!

## Deliverables

### Source Code
Three ROS 2 packages:
1. `task_1`- ROS 2 `ament_python` package with custom publisher, subscriber and launch script
1. `task_2`- ROS 2 `ament_python` package with custom publisher & subscriber and service & client node exchanging a custom message and service interface
1. `task_2_interfaces`- ROS 2 package with a custom message and service interface

### ROS2 Bag Files
You will 'record' the data passing through your topics via `ros2 bag`:
1. in any directory outside from your workspace directory, run:
    * $`mkdir bag_files`
    * $`cd bag_files`
    * $`ros2 bag record -o task_1 <your_topic_name>`
    * $`ros2 bag record -o task_2 -a`

NOTE: your topic must be alive for ros2 to record it. 

Use this as reference: [ROS2 Bags](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.html)

### ros2 log files
Upload `<ws_ros2>/log/` too

Your folder structure should be as such:

```
42
|
|__bag_files
|       |__task_1
|       |     |__metadata.yaml
|       |     |__task_1_0.db3
|       |    
|       |__task_2
|             |__metadata.yaml
|             |__task_2_0.db3
|            
|__task_1
|     |__launch
|     |__resource
|     |__...
|     
|__task_2
|     |__resource
|     |__...
|     
|__task_2_interfaces
|     |__msg
|     |__...
|
|__log
```

Where 42 must be replaced with your roll-number.

### Rubric
Deviating from the names provided in the lab sheet will result in penalties.
* 25 pts: Week 2-1 `task_1` pkg
* 25 pts: Week 2-1 `ros2 bag` file
* 40 pts: Week 2-2 `task_2` pkg and `task_2_interfaces` pkg
* 10 pts: Week 2-2 `ros2 bag` file
