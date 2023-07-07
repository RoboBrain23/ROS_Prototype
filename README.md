# ROS_Prototype
This repository contains the code of description, mapping, localization, and navigation for a prototype using C++ and Python. gmapping package is used to create the map and save it to be used in navigation later in navigation using the SLAM algorithm. Sending goals is implemented in two different ways, GUI in Rviz and C++ node 


### Getting Started

- `cd catkin_ws/src`
-  Clone this repo here: `git clone "https://github.com//ROS_Prototype"`
- `cd ..` (Go back to catkin_ws/)
- `catkin_make`<br />
Open a terminal and run `gedit .bashrc` and add this line in it <br />
```
source {add_location_to_devel_folder}/setup.bash
```


### Gazebo Simulation
To launch the urdf `prototype description` model in Gazebo <br />
open a new terminal and write: <br />
```
roslaunch slam gazebo.launch
```
To start moving robot in gazebo <br />
In a new terminal: <br />
```
rosrun teleop_twist_keyboard teleop_twist_keyboard.py
```
To start to view the simulated robot model and take information from the robot's camera, <br />
In a new terminal: <br />
```
rviz rviz
```


### Mapping
Run these commands in two different terminal tabs
```
roslaunch slam gazebo.launch
rosrun teleop_twist_keyboard teleop_twist_keyboard.py
```
Run SLAM, in a new terminal: <br />
```
roslaunch slam gmapping.launch
```
In a new terminal,   
Run RVIZ : <br />
```
cd catkin_ws/src/slam/rviz
rviz -d map.rviz
```

To create a map of your environment, drive the robot around the environment you want to map.




https://github.com/RoboBrain23/ROS_Prototype/assets/61033121/90344239-2f06-4d90-94c6-ecef7554287e


Once you completed mapping the whole environment,
save the map:
```
rosrun map_server map_saver ~/catkin_ws/src/slam/maps/name_of_map
 ```

### Autonomous Navigation
Autonomous Navigation can be done using the following -- giving the final destination:
#### 1- Graphical User Interface (GUI).
launch the urdf model in Gazebo <br />
In a new terminal: <br />
```
roslaunch slam gazebo.launch
```
Run RVIZ :<br />
```
cd catkin_ws/src/slam/rviz 
rviz -d navigate.rviz
```
Using the amcl package for localization, `Run amcl.launch`. The amcl algorithm implements Monte Carlo localization for state estimation.
We will use amcl with a map built in the previous step. In the second part, we will create our map using the gmapping package and then use the resulting map for localization. <br />
```
roslaunch slam amcl.launch map:='name_of_map'
```
The move_base package implements an action that will attempt to reach it with a prototype base. This node links a global and local planner to accomplish its global navigation task. The move_base node also maintains two costmaps, one for the global planner and another for a local planner that are used to accomplish navigation tasks. `Run move_base.launch`.<br />
```
roslaunch slam move_base.launch 
```





https://github.com/RoboBrain23/ROS_Prototype/assets/87612394/e297e60c-6b4b-4e42-81ea-c96f2de2c297







* Red arrows indicate the probable location.
* Set a goal for the robot in RVIZ (Click "2D Nav goal" and pinpoint the desired location and direction on the map).
* Green line indicates the path planned.
* It avoids obstacles "according to the pre-saved map" and also `Dynamic obstacles` that are added when it navigates.

#### 2- Sending the goal location coordinates through the move_robot node.

launch the urdf model in Gazebo <br />
On a new terminal: <br />
```
roslaunch slam gazebo.launch
```
Run RVIZ :<br />
```
cd catkin_ws/src/slam/rviz 
rviz -d navigate.rviz
```
Run `amcl.launch`, giving the name of the map you have created and saved: <br />
```
roslaunch slam amcl.launch map:='name_of_map'
```
Run move_base.launch :<br />
```
roslaunch slam move_base.launch 
```
Run the move_robot node that sends the goal to navigation-stack 
```
rosrun move_robot move_robot
```


https://github.com/RoboBrain23/ROS_Prototype/assets/61033121/cc21e13b-86a1-4dcc-ace9-d8664a51e0c8




* The desired rooms/final goals location are predefined through the user and mapped to i.e. letters A, B, C, and D.
* Each letter indicates the coordinate of the room.
* While ROS System is run in this mode, the user is asked to the next destination.
