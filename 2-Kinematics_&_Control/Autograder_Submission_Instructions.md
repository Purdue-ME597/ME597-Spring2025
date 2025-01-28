# Autograde Submission Instructions

## Submission Format
1. You must submit your work in a .zip file. The autograder will not work in other formats, such as a github or Bitbucket submission (even though they may be offered as an option on Gradescope).
2. Your submission must follow the exact structure as specified in the deliverables section of the lab.
3. Your node names, message names, and information in the launch files must be exactly as specified in the instructions.
4. Only submit the specified files, do not include any extra files because they may make the submission size too large.
5. You only need to submit the latest folder, latest_build folder and COLCON_IGNORE in the main log folder. 
6. Remove build, install, and log files from inside your packages. There should only be log files from the log folder of your workspace.

## Common Issues
1. Cannot submit at all:
    - Too many files or too large of a submission size
        - Do not submit your build or install folders.
        - Delete any log, build or install folders found within your ROS packages
        - Only submit the 2 most recent log folders (called latest and latest_build) and COLCON_IGNORE. Delete the rest.
2. Wrong format
    - Submission must be a .zip
    - Submission must follow the folder format given in deliverables.
3. ros2 cannot find your launch file.
     - Make sure to add the launch file path to the setup.py file as shown in [ros2 docs Launch System](https://docs.ros.org/en/humble/Tutorials/Intermediate/Launch/Launch-system.html#creating-the-structure-to-hold-launch-files)
4. Node alias names not recognized
   - Make sure that the node name in your python script file matches the alias name assigned in your launch file per the alias names specified in the lab instructions.
   - If these do not match the autograder will not pick them up and will give you a zero for this portion.
