# Fix build error (no such file or directory) when building a node with a custom service definition inside a package

Add ${PROJECT_NAME}_generate_messages_cpp to the add_dependencies(... line in your CMakeLists.txt

Example:
add_executable(image_snapshot src/image_snapshot.cpp)

target_link_libraries(image_snapshot ${catkin_LIBRARIES}) 

add_dependencies(image_snapshot ${${PROJECT_NAME}_exported_targets} ${catkin_EXPORTED_TARGETS} ${PROJECT_NAME}_generate_messages_cpp) 


Answer found here: https://www.theconstructsim.com/ros-qa-163-custom-message-fails-build-no-file-directory/

# Fix remote machine ssh problem with ROS
Remove ssh key from known_hosts
ssh-keygen -R <hostname>


SSH in passing in this argument (-oHostKeyAlgorithms='ssh-rsa')
ssh -oHostKeyAlgorithms='ssh-rsa' <username>@<hostname>

# Install all dependencies in a workspace via rosdep
rosdep install --from-paths src --ignore-src -r -y

# Launch ROS processes while inside a node using Python
Example:

Create a bash script that roslaunches or rosruns a new node.  In the launch case you will also need to create the appropriate launch file for this script to call.

Inside the python node use:

subprocess.Popen('bash <path_to_script_file>, shell=True)


NOTES (READ!!!):

Popen is used because it is not blocking though there are other calls you can use that may have other functionality.

Doing this is NOT recommended and goes against ROS design principles but I had to for a very specific case.  If possible you should always just have nodes spawning new subscribers, publishers, or services instead of spawning entire new nodes!


EXAMPLE:

Inside your python node ros_node.py:

subprocess.Popen('bash ~/catkin_ws/src/ros_node/scripts/script.sh ' + val1 + " " + val2, shell=True)


Script file:

#!/bin/bash
roslaunch ros_node ros_node.launch arg1:=$1 arg2:=$2

# Make ROS log output include the node name ROSCONSOLE_FORMAT
Put this in your .bashrc


export ROSCONSOLE_FORMAT='[${severity}] [${node}] [${file}] [${function}] [${line}]: ${message}'



The full list of possible options follows.

Token       Sample Output
severity    ERROR
message     hello world 0
time        1284058208.824620563
            (if no sim time is available it only shows the wall time)
            1284058208.824620563, 1234567890.123456789
            (if sim time is available it shows the wall time first and the sim time second)
walltime    1284058208.824620563
            (this feature was always available in Python, in C++ it is only available since ros_console version 1.12.6)
thread      0xcd63d0
logger      ros.roscpp_tutorials
file        /wg/bvu/jfaust/ros/stacks/ros_tutorials/roscpp_tutorials/talker/talker.cpp
line        92
function    main
node        /talker


# Reset your included build spaces in a given workspace
catkin config --extend <path_to_ros>

catkin config --extend /opt/ros/melodic


# Set logger level for the output of a node via a launch file
In the launch file add the following line
<node pkg="rosservice" type="rosservice" name="set_{node_name}_logger_level" args="call --wait {node_name}/set_logger_level 'ros' '{desired_level}'"/>

Example:
In this example I set image_undistort node to logger level 'error'
<node pkg="rosservice" type="rosservice" name="set_image_undistort_logger_level" args="call --wait /image_undistort/set_logger_level 'ros' 'error'"/>

# TF view_frames in ROS Noetic
In 20.04 ROS Noetic rosrun tf view_frames  is broken so we must adapt...

sudo apt install ros-noetic-tf2-tools 
rosrun tf2_tools view_frames.py 

# Unsupported GNU version build error.  Specifically with darknet_ros
catkin build -DCMAKE_BUILD_TYPE=Release -DCMAKE_C_COMPILER=/usr/bin/gcc-6 darknet_ros

catkin build -DCMAKE_BUILD_TYPE=Release -DCMAKE_C_COMPILER=/usr/bin/gcc-8 darknet_ros