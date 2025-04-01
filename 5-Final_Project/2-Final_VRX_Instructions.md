# ME 597- Final Project

## Final Project using VRX (Virtual Robot X)

### Introduction
The objective of the final project is to put into practice different concepts that have been acquired during the lab activites using the simulation environment and ROS2. This is an **individual** project.

This version of the final project will be done using the Virtual RobotX (VRX) Simulator linked [here](https://github.com/osrf/vrx). This is a marine robotics simulation environment used to simulate tasks with a robotic boat (WAM-V) as part of the RobotX competition. The environment is heavily documented [here](https://github.com/osrf/vrx/wiki). While developing, please make sure that you are looking at the most recent wiki page and *not* the VRX Classic pages, which are for the ROS1 version of the simulator.

Note that throughout this document, and the VRX documentation, the vehicle is referred to interchangeably as a USV (unmanned surface vehicle, broad term to describe a class of robots, like "ground vehicle") and WAM-V (brand-specific vehicle, like "Turtlebot").

### Setup
You will need to follow the tutorial linked [here](https://github.com/osrf/vrx/wiki/getting_started_tutorial) to install the VRX software. These steps are summarized below:.

1) Follow the instructions [here](https://github.com/osrf/vrx/wiki/preparing_system_tutorial) if installing on a host computer or [here](https://github.com/osrf/vrx/wiki/docker_install_tutorial) if you will run VRX inside a Docker container. 
    * Note: we recommend running VRX inside a Docker container, as it is faster to set up.
2) Install VRX as shown [here](https://github.com/osrf/vrx/wiki/installation_tutorial). 
3) Test the installation using [this](https://github.com/osrf/vrx/wiki/running_vrx_tutorial) brief tutorial.
4) Read the documentation provided [here](https://github.com/osrf/vrx/wiki/getting_around_tutorial), with a focus on [how to drive the WAM-V](https://github.com/osrf/vrx/wiki/teleop_tutorial) and [articulate the thrusters](https://github.com/osrf/vrx/wiki/thruster_articulation_tutorial). We request that you use the [default WAM-V configuration](https://github.com/osrf/vrx/wiki/default_wamv_tutorial), but the customization documentation may be of interest for other projects.

### Assignment
The task in this final project is to select tasks, which are graded out of different point values depending on difficulty, from the VRX 2023 competition. You can find information for these tasks in the [task_tutorials](https://github.com/osrf/vrx/wiki/vrx_2023-task_tutorials#individual-tasks) section of the wiki. The tasks are as summarized below and generally increase in diffculty as the task number increases:

1) [Stationkeeping](https://github.com/osrf/vrx/wiki/vrx_2023-stationkeeping_task): navigate to goal pose and hold 
2) [Wayfinding](https://github.com/osrf/vrx/wiki/vrx_2023-wayfinding_task): navigate through a series of published waypoints (position and orientation)
3) [Perception](https://github.com/osrf/vrx/wiki/vrx_2023-perception_task): identify a series of markers and report their locations
4) [Acoustic Perception](https://github.com/osrf/vrx/wiki/vrx_2023-acoustic_perception_task): navigate to a beacon broadcasting its position relative to the vehicle
5) [Wildlife Encounter and Avoid](https://github.com/osrf/vrx/wiki/vrx_2023-wildlife_task): track a set of animals and perform different actions based on which animal is being tracked
6) [Follow the Path](https://github.com/osrf/vrx/wiki/vrx_2023-follow_the_path_task): vehicle must traverse a channel marked by buoys and avoid collision with the buoys
7) [Acoustic Tracking](https://github.com/osrf/vrx/wiki/vrx_2023-acoustic_tracking_task): same as Task 4, but with a moving beacon
8) [Scan and Dock and Deliver](https://github.com/osrf/vrx/wiki/vrx_2023-scan_dock_deliver_task): vehicle must detect a dock, move into the correct gate (after determining the correct gate using a light sequence), and optionally launch a ball into one of the openings in the gate

More details regarding the tasks and scoring are available at [this link](https://robotx.org/programs/vrx-2023/) under the Competition Resources>2023 Task Descriptions section. More technical information is available at the same link under Competition Resources>2023 Technical Guide. You can ignore instructions about how to submit code for competition.

### Grading

Each task will be graded out of a different number of points depending on the difficulty of the task. There are two ways to recieve full credit for this project:

1) Select any **3** tasks of Task 1, Task 2, Task 3, Task 4:

| Task |        | Available Points |
| :--: | -------| :--------------: |
|  1   |        |       30         |
|  2   |        |       30         |
|  3   |        |       30         |
|  4   |        |       30         |

2) Select any **1** task of Task 5, Task 6, Task 7, Task 8, with extra credit awarded as follows:

| Task |        |        Available Points            |
| :--: | -------| :--------------------------------: |
|  5   |        |                90                  |
|  6   |        |                90                  |
|  7   |        | 100 (up to +10 extra credit)       |
|  8   |        | 120 (up to +30 extra credit)       |

For example, three variations that could all receive full credit are

* Task 1 + Task 2 + Task 3 = 90
* Task 1 + Task 3 + Task 4 = 90
* Task 5 = 90

### Guidelines
* You should follow the same submission conventions as in prior labs.
* Please include a `.txt` file listing the tasks you are implementing in your submission.
* For each of the tasks you are implementing, create a new package with the following conventions:
    * Named `task_N`, where N is the task number you are implementing, and containing solution node `task_N.py`
    * Launch file named `task_N_launch.py`, again replacing N with the task number you are implementing (this can be run separately from the world launch file)
    * Solution can be launched with `ros2 launch task_N task_N_launch.py`
* While the instructions for each task note that you can manually drive the WAM-V to complete the task, for this project, the tasks should be completed *AUTONOMOUSLY*. You may use manual control in testing and exploring the simulation environment.
* Your solutions will be evaluated in part based on the scores available through the VRX scoring plugins. This may include testing in other worlds other than the default.
* Please use the default vehicle configuration (H-thruster configuration). 

### Presentation
Finally, you will give a 5-7 slide presentation. This must contain a concise explanation of the algorithms employed to solve each of the tasks. In case you have not completed some of the tasks that you selected to reach 90 points, the slides should explain what you tried, and your explanations for why a particular task is not working. You have a maximum of 10 minutes for the presentation.

### Rubric
* 90-120 pts: Task completion, awarded as indicated in this document
* 10 pts: 5-7 Presentation Slides
* (Up to -10 points): Negative points- Missed final project presentation
* +10 points for attempting VRX

