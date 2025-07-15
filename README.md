# 🧭 Task-1: Autonomous Navigation using TurtleBot3 in a Custom Gazebo World

This repository contains the ROS 2 (Humble) packages and configuration files for performing **autonomous navigation** using **TurtleBot3** in a selected **Gazebo world**. The workflow includes mapping the environment using teleoperation and navigating autonomously from point A to point B using the Navigation2 stack.

---
###Demo VIdeo - https://youtu.be/HhZbk4RJ7oM 
## ✅ Task Overview

**Objective**:  
Create a map of a selected environment using TurtleBot3 and perform autonomous navigation from a user-defined start point (A) to a goal point (B) using RViz and the Navigation2 stack.

---

## 🛠️ System Requirements

- **ROS 2 Humble** (on Ubuntu 22.04)
- **Gazebo (Fortress or default with Humble)**
- **TurtleBot3 Packages**
- **RViz2**
- **teleop_keyboard**

---
### Install Required Dependencies

```bash
sudo apt update
sudo apt install ros-humble-navigation2 ros-humble-nav2-bringup \
                 ros-humble-slam-toolbox ros-humble-turtlebot3* 
```
## 📦 Package Setup

### 1. Clone and Build Repositories

This will create a task1 folder which can be used as your workspace
```bash
git clone https://github.com/YashSharma-code/Software-Trials-Task_1.git
cd task_1
colcon build
source install/setup.bash
```
## 🗺️ Mapping the Environment

### Launch Gazebo with the Selected World:

In terminal 1(Make sure to source your workspace):
```bash
export GAZEBO_MODEL_PATH:=task_1/models #or your custom models folder in case of custom world
ros2 launch turtlebot3_gazebo turtlebot3_custom_world.launch.py
```
This will launch the Custom_hospital.world

If you want to launch another world just copy the .world file to task_1/src/turtlebot3_simulations/turtlebot3_gazebo/worlds

& then

change Custom_hospital.world to <your-world> file in task_1/src/turtlebot3_simulations/turtlebot3_gazebo/launch/turtlebot3_custom_world.launch.py in the following 
```
world = os.path.join(
        get_package_share_directory('turtlebot3_gazebo'),
        'worlds',
        'Custom_hospital.world'
    )
```
If the following line:

export GAZEBO_MODEL_PATH:=task_1/models #or your custom models folder in case of custom world

does not work, you can manually copy paste your models to $HOME/.gazebo/models (In this repo the models are located in task_1/models)

### Launch SLAM Toolbox:
In terminal 2:
```bash

ros2 launch slam_toolbox online_async_launch.py use_sim_time:=True

```

### Teleoperate the Robot:
In terminal 3:
```bash
ros2 run turtlebot3_teleop teleop_keyboard
```

### Launch rviz2 to visualize your current map
In terminal 4:
```bash
rviz2
```
### Save the Map:

After covering the environment:
In terminal 5:
```bash
ros2 run nav2_map_server map_saver_cli -f ~/map/my_map
```

---

## 🤖 Autonomous Navigation

### Launch Nav2 Stack:

```bash
ros2 launch turtlebot3_navigation2 navigation2.launch.py map:=$HOME/map/my_map.yaml use_sim_time:=true
```

### Launch RViz (if not already):

```bash
ros2 run rviz2 rviz2
```

- Use **2D Pose Estimate** to set initial pose
- Use **2D Nav Goal** to define destination

---

## 📎 Notes

- Ensure your map is of good quality with clear walls and corners.
- Set the robot pose carefully for accurate localization.
- If navigation fails, check for:
  - Incomplete map
  - Incorrect initial pose
  - Obstacles in the path
  - Transform errors (check with `ros2 run tf2_tools view_frames`)

---

## 🔗 Credits

- [TurtleBot3 by ROBOTIS](https://github.com/ROBOTIS-GIT/turtlebot3)
- [Gazebo World Dataset](https://github.com/mlherd/Dataset-of-Gazebo-Worlds-Models-and-Maps)

---
