# ROS2 Workspaces
## What does it mean to "source" or "configure" my ros2 workspace? 

You may notice that we have to run commands like:
`source ~/opt/ros/humble/setup.bash` or `source ~/your_ros2_ws/install/local_setup.bash`, for ros2 and your ros2 packages to work. Why? What do they do?
## Bash Environment
Before we get into workspaces and configuring them, first, we will go over your bash environment. When you open a terminal, the commands you type are bash commands. For example, `source` simply executes the contents of the file you specify, in the shell. It can be used to execute scripts that modify the current bash environment. The bash environment is a set of environment variables that are available in the current bash shell and are passed to any child processes created in that shell.
* If you want to take a look at your environment variables, use command `printenv` in a terminal. Some notable ones are `USERNAME`: your username, `PWD` : your present working directory (not password!), and `PATH`: specifies a set of directories at which executables can be found, such as python. When you execute a command, the bash shell will search through the PATH directories for an executable matching that command name.

Each time you open a new terminal, the environment is reset back to default settings. For example, if you open a terminal and create an environment variable using:

`export my_environment_variable='XXXXXXXXXXXX'` 

You will be able to print it with: `echo $my_environment_variable`

It will also appear with `printenv`. However, it is only set for that environment, AKA, that terminal. If you open a new terminal, it will not be there. Why is this important? This is important because you must add ROS2 environment variables from a workspace to your current environment.


## Activate the ROS2 workspace with `source /opt/ros/humble/setup.bash`

 When you install ROS2 and open a new terminal, ROS2 is not ready to use. If you enter a command like `ros2 topic list` , you will get an error `ros2: command not found`. This is because the path containing the ros2 executable is not in the `PATH` environment variable yet. To configure the environment for ros2, you must source the ros2 workspace with `source /opt/ros/humble/setup.bash`. This command runs the setup.bash file, which configures the environment, such as adding ros2 to the PATH environment variable. You can see this by: `echo $PATH`. This setup script does more to configure the environment, but this is just one example.

Now that the ros2 workspace has been sourced, all of the base ros2 commands and functionalities are available. We have essentially "activated" ros2 (I'm using this term informally). However, we want to use our custom ros2 packages that we have in our workspaces. Simply source-ing the base ros2 environment won't allow us to use our packages. We now have to also source our individual workspaces so that we can use the packages inside.



For context, by the end of this course, you will have these workspaces (labs, simulator, final project): 

```
 me597
    ├── lab1_ws
    ├── lab2_ws
    ├── lab3_ws
    ├── lab4_ws
    ├── sim_ws
    └── final_ws  
```
* Note: A workspace is simply a directory containing ros2 packages. However, workspaces follow a standard structure. Read about it further down. 

In lab1, we just need the packages in lab1_ws to complete assignments. So we only have to source ros2 and lab1_ws. 

However, in the rest of the labs, we need to use the simulation package in sim_ws and our source code in labX_ws. We also want to use only the necessary workspaces for the current project. For example, we may not want to activate the lab1_ws and lab2_ws when we are completing lab3. So, how do we selectively activate each workspace, so that we activate all the packages we need, and nothing extra?

## Source Multiple Workspaces

The answer is simple: we only source the workspaces we need.

1. First, for anything using ros2, we must source the ros2 workspace:
`source ~/opt/ros/humble/setup.bash` 
    * Note: These steps must be done for every terminal you open. Below we will discuss how to make it partially automated & faster.

2. Next, you must source any workspace you want to use:
`source ~/path/to/your/ros2_ws/install/local_setup.bash`

    ```
    ros2_ws
        ├── build
        ├── install
        |   ...
        |   ├── local_setup.bash
        |   └── setup.bash
        ├── log
        └── src  
    ```
    * What is the difference between `local_setup.bash` and `setup.bash`? `local_setup.bash` sources just this workspace. `setup.bash` sources this workspace and any other parent workspaces (underlays) that had been sourced at the time of `colcon build` -ing. Both can be used, but it is good to understand the differences. 

## Workspace Architecture

```
 ros2_ws
    ├── build   <-- intermediate build files from colcon
    ├── install <-- executables and setup files for packages
    ├── log     <-- log files for build and runtime processes
    └── src     <-- source code (your ros2 packages)
```
General ROS2 workspace structure.

```
src/
  ├── my_python_package/     <-- your ROS 2 Python package
  │   ├── launch/            <-- launch files for starting nodes
  │   │   └── my_launch.py
  │   ├── my_python_package/ <-- your Python source code (module)
  │   │   ├── __init__.py    <-- makes this directory a Python package
  │   │   ├── some_module.py
  │   │   └── another_module.py
  │   ├── include/           <-- used for C++ headers if needed (rare for Python packages)
  │   │   └── ...
  │   ├── package.xml        <-- defines package metadata, dependencies, required build tools
  │   ├── setup.py           <-- script used to build and install the Python package
  |   └── ...                <-- any additional files (e.g., setup.cfg)
  |
  └── my_cmake_package/      <-- your ROS 2 C++ package
      ├── include/           <-- header files for C++ code
      │   └── ...
      ├── msg/               <-- holds custom message definitions
      │   └── MyMessage.msg
      ├── srv/               <-- holds custom service definitions
      │   └── MyService.srv
      ├── action/            <-- holds custom action definitions
      │   └── MyAction.action
      ├── src/               <-- C++ source code
      │   └── my_node.cpp
      ├── CMakeLists.txt     <-- defines build process, dependencies, and executables for C++ package
      ├── package.xml        <-- contains package information, dependencies, and build tools for C++ package
      └── ...                <-- any additional files (e.g., configuration files, launch files, etc.)

```
General ROS2 package structure for python and c++ packages. Note: python packages cannot be used to define custom interfaces (msg, srv, action).

## ROS2 Overlays and Underlays
The concept of sourcing ROS2 and multiple workspaces uses an important ROS2 concept called overlays. From reading above you already know the concept. In ROS2, each workspace you source is a layer. The bottom workspace layer will almost always be the base ROS2 environment. When you add another layer by sourcing another workspace (e.g. sim_ws), that workspace is an overlay. It is called this, because it will add its own environment configuration on top of the bottom layer, and overwrite any environment variables that they share. Finally, when you again add another layer (e.g. lab3_ws), that layer is now an overlay of the first two layers. Conversely, the sim_ws is now an underlay of the lab3_ws. It is important to source these in the correct order, so that the top layers (last to be sourced) add onto and overwrite the bottom ones.

```
Your environment:

lab3_ws  ========= Overlay =========== <-- source last
sim_ws   ========= Underlay ========== 
ROS2     ============================= <-- source first
bash env ============================= <-- from ~/.bashrc
```

## Automating Environment Configuration

Remember how each time a new terminal is opened, the environment variables are reset back to default? How can we set environment variables for all new terminals we open? The answer is the ~/.bashrc file. This file is run by every new terminal that is opened. If you want a specific command to be run in every new terminal, you can add that command to the end of your ~/.bashrc file.

For example, it is common to add `source /opt/ros/humble/setup.bash` to the ~/.bashrc file. You may have already been instructed to do this during your ros2 install process. This configures the ros2 environment in each new terminal. If you work with multiple versions of ros2, it is a good idea to remove or change this so they potentially don't interfere with each other, however, we won't run into that issue in this course. It is not recommended to add workspace source commands `source ~/your_ws/install/setup.bash` to the ~/.bashrc because there are times when you do not want every local workspace to be sourced. Such as when running `colcon build`.

Instead, you can automate the local source command to make your life easier, such as creating a separate script that sources a set of the workspaces you're working with. E.g.-
```
# New file lab2.bash
source ~/sim_ws/install/local_setup.bash
source ~/lab2_ws/install/local_setup.bash
```
or create an alias to run the source command so you don't have to enter the full command: e.g., Adding the following line to ~/.bashrc
```
alias sim_setup='source ~/sim_ws/install/local_setup.bash'
```
    
Warning: Be careful when modifying the `.bashrc` file. Incorrect modifications to this file could affect behavior of the terminal across all sessions.
