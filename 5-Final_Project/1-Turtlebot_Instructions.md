# ME 597 - Final Project

## Final Project using Turtlebot (in Simulation)

### Introduction
The objective of the final project is to put into practice different concepts that have been acquired during the lab activities using the simulation environment Gazebo and ROS2. This is an **individual** project. 

The project will be done on a modified turtlebot3 house world to provide you with enough challenge and distinctive features to let you decide between different techniques to implement your solution. 

### Assigment 
The project gives a total of 100 points for what we call basic tasks. In addition to those tasks, we have defined a set of tasks (that build upon the basic tasks) that will be considered for a total of up to 30 points, giving a total of 130 points. A top level description of the tasks that you need to accomplish is included as follows:

* **Task 1 - Autonomous Mapping:** For this task you will need to use Turtlebot3 Waffle and the navigation capabilities used in Lab 4 to map a customized environment. You will need to implement an algorithm to make Turtlebot automatically cover as much area as possible within a limited amount of time. You can implement random bouncing, wall following, RRT, or any other algorithms for this autonomous mapping task. 
    * Notes:
        * Do not use the keyboard or fixed waypoints to control the turtlebot to map the space.
        * You may use any navigation code developed for Lab 3.
        * As in Lab 3, do not use preexisting packaged like Nav2 to implement navigation capabilities. 
        * This task will be graded based on the coverage rate (visited grid cells / all grid cells) of the map created by your algorithm.
        * Extra credit is available; your `task1.py` file will be run on a second, unknown world. 
        * You will have a maximum of 10 minutes to complete both worlds.
        * The code required for this task needs to go inside the `task1.py` file.
        * The given world can be spawned by running the `turtlebot3_house.launch.py` file. The `mapper.launch.py`is the main launch file for this task, which already includes a SLAM node and the `task1.py` node.
        
* **Task 2 - Navigation with Static Obstacles:** In this task, you will use an A∗ path planner in the same way that you did for Lab 3. You will be able to use the AMCL node for localization, and path planning and path following algorithms must be part of your solution. This task is almost identical to Lab 3, however we will spawn in static obstacles that you must also avoid. 
    * Notes:
        * The map to be used for path planning should be taken from Task 1.
        * We will spawn in random trash cans that are not mapped, your code should be able to navigate around the new, static obstacles. Trashcans will not be placed around a corner and will not block all accessible paths.
        * To grade this task, we will assign goal poses sequentially, then measure the time cost of all successful collision-free navigations to get a weighted score.
        * The code required for this task needs to go inside the `task2.py` file.
        * Extra credit is available for implementation of RRT* algorithm for local path replanning. This work should be implemented in a file called `task2_bonus.py`.
        * The given world can be spawned by running the `turtlebot3_house.launch.py` file. The `navigator.launch.py`is the main launch file for this task, which already includes a SLAM node and the `task2.py` node.

* **Task 3 - Search and Localize:** In this task, large colored balls will be randomly spawned throughout the house. You are tasked with creating a search algorithm that can find, identify, and give an x, y estimate of the balls in the world. In order to successfully complete this task, you will need to reuse your obstacle avoidance strategy from Task 2 while building in object detection and localization logic. 
    * Notes:
        * The ball colors will be easy to identify from the surroundings.
        * We will change the location of the balls when grading to avoid hardcoding solutions.
        * For full credit you are required to identify the location of three balls. Error in the localization will deduct points from the total.
        * Extra credit is available for the fastest solutions. The speed at which you can identify all three objects will be hindered by any error in your localization so that the "fastest solutions" have the best balance between speed and accuracy.
        * The code required for this task needs to go inside the `task3.py` file.
        * The given world can be spawned by running the `turtlebot3_house.launch.py` file. The `navigator.launch.py`is the main launch file for this task, which already includes a SLAM node and the `task3.py` node, depending on the provided input argument enabling dynamic obstacles.

### Guidelines
* To aid in completing this project, we have created a repository with the base files that you will need to solve all the tasks. You can clone these files from the following [link](https://github.com/naslab-projects/sim_ws/tree/main), available in the `src/turtlebot3_gazebo/src/lab4` directory. 
* Your solution Python script for each task should be self-sufficent without depending on other scripts (from the other tasks or otherwise).
* Please only depend on commonly used Python (ROS2) packages like `numpy`, `rclpy`, `math`, and `cv2`.
* Do not submit extra files for any tasks. Each task file should be able to function without relying on another task file. Any major problem while running your files will be subject to point deductions. 
* All 3 tasks (not including RRT* bonus task) should not take longer than 20 minutes to run in total.
* The repository has a package structure that you will use to implement your solutions. You must match the structure outlined below and match the names of the files. Files that you will write are indicated with an arrow. Any folders in the below tree that are not expanded should have the same content as from `sim_ws`.

```
turtlebot3_gazebo/
    ├── launch
    ├── maps
        ├── map.pgm                     <-- your files
        ├── map.yaml                    <-- your files
    ├── models
    ├── params
    ├── rviz
    ├── src
        ├── lab4
            ├── task1.py                <-- your files
            ├── task2.py                <-- your files
            ├── task2_bonus.py                <-- your files
            ├── task3.py                <-- your files

    ├── urdf
    ├── worlds
    |
    ├── CMakeLists.txt
    ├── README.md
    ├── package.xml
```
You will use

* `turtlebot3_house.launch.py` to launch the house world for all tasks

* `mapper.launch.py` for Task 1

* `navigator.launch.py` for Tasks 2 and 3 

### Grading
1) Task 1: Autonomous Mapping
    * Baseline solution: autonomous mapping strategy to create a map of the given world
    * Credit will be awarded on a linear scale from 0 to 36 based on coverage, where 0% earns 0 points and 85% earns 36 points.
    * For extra credit (up to +10), your `task1.py` file will be run on a second, unknown world. This world will not be provided to you before grading. Extra credit will be linearly assigned for coverage >60% on this world, where 60% earns 0 extra points and 100% earns 10 extra points.
2) Task 2
    * Baseline solution: create a path and drive the vehicle through an A∗ path to go to the goal pose (with stationary obstacles)
    * Every collision will deduct two points from your total score.
    *  For extra credit, the correct implementation of an RRT* local path replanner for navigation around static obstacles on the global path included in the bonus file will recieve 10 bonus points.
3) Task 3
    * Baseline solution: create a path and drive the vehicle through an A* path to go to the goal pose (with moving obstacles)
    * Full points will be given to the solutions getting to the goal pose with no collisions. 
    * Every collision will deduct 2 points from the final score.
    * For extra credit, we will rank full-credit solutions based on task completion time + penalty for localization error. For the top 50% of solutions:
        * The top 15% of solutions will be given 10 extra points
        * The next 15% of solutions will be given 5 extra points
        * The remaining 20% of solutions will be given 3 extra points
        * Localization error will add time to your completion time and will negatively impact your ranking.



### Deliverables

1) `map.pgm`, `map.yaml`, `task1.py`, `task2.py`, and `task3.py`.
2) A video for each of the tasks.
3) A brief presentation in pdf format showing your solution strategy.

### Presentation
Finally, you will give a 5-7 slide presentation. This must contain a concise explanation of the algorithms employed to solve each of the tasks. In case you have not completed some of the tasks that you selected to reach 90 points, the slides should explain what you tried, and your explanations for why a particular task is not working. You have a maximum of 10 minutes for the presentation.

### Rubric
* 36 pts: Task 1
* 18 pts: Task 2
* 36 pts: Task 3
* 10 pts: 5-7 Presentation Slides
* (Up to -10 points): Negative points- Missed final project presentation
* (up to -10 pts) - Negative points - Missed final project presentation.
* (up to 10 pts) - Extra Credit - Successful unknown world mapping for Task 1.
* (up to 10 pts) - Extra Credit - RRT* Local Path Replanning.
* (up to 10 pts) - Extra Credit - Top 50% fastest solutions for Task 3.
