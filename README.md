# JHU05
Custom Husky and UR5 configuration for JHU05 project

For Husky instructions and tutorials, please see http://wiki.ros.org/Robots/Husky

For this installation, local (user laptop for example) pre-requisites can be installed with:
```
sudo apt-get install ros-indigo-ur-modern-driver -y
sudo apt-get install ros-indigo-moveit-planners* -y
sudo apt-get install ros-indigo-moveit-ros-planning* -y
sudo apt-get install ros-indigo-moveit-ros-move-group -y
sudo apt-get install ros-indigo-moveit-ros-control-interface -y
```
On the system with a user interface (either the robot or laptop, user computer, etc) you will also need to install:
```
sudo apt-get install ros-indigo-moveit-visual-tools
sudo apt-get install ros-indigo-moveit-ros-visualization
```

To launch the planner on a local laptop, ensure ROS_IP (your user interface's IP) and the Husky's IP are exported properly (you may have to edit your /etc/hosts file to add the Husky's ):
```
export ROS_IP=192.168.1.X
export ROS_MASTER_URI=http://cpr-a200-0394:11311
```
On the robot, the UR5 driver should be brought up manually, as the UR5 arm can be powered on and off separately from the rest of the robot:
```
roslaunch jhu05_moveit ur5.launch
```

At this point, it’s useful to ensure that the user computer/laptop is able to successfully communicate with the husky. In the same terminal windows as used above to export ROS_IP, ensure you have a catkin_ws folder, and clone this Github folder. We also install pre-requisite husky packages. Lastly we source it, and launch an rviz viewer, which should show the correct orientation of the arm and gripper, along with accessories on the Husky top plate.
```
cd ~/catkin_ws/src
git clone https://github.com/ibaranov-cp/JHU05.git
cd ..
sudo apt-get install ros-indigo-husky-viz
rosdep install --from-paths src --ignore-src --rosdistro=indigo -y
catkin_make
source devel/setup.bash
roslaunch husky_viz view_robot.launch
```

Once the driver is up, the planner can be started on the robot, or on the laptop (as long as exported properly, etc) with:
```
roslaunch jhu05_moveit move_group.launch
```
Once both of the above steps are done, the user interface may be launched on the user laptop with:
```
roslaunch jhu05_moveit moveit_rviz.launch
```
Be sure to try out different planners, as some are significantly faster and more efficient than others.

![](http://i.imgur.com/56OeyHT.png)

Note: It is currently very difficult to accurately portray gripper position, so gripper model is fixed as fully open, for collision planning purposes. Avoiding collisions in general is a tricky process, and should be fined tuned by the user for their application.

To open and close the gripper, a simple sample controller can be launched with the following command. Be aware that the gripper may need to be reset and/or activated before use.
```
rosrun robotiq_c_model_control CModelSimpleController.py
```
