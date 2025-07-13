
# 🧭 Task-1: Autonomous Navigation using TurtleBot3 in a Custom Gazebo World

This repository contains the ROS 2 (Humble) packages and configuration files for performing **autonomous navigation** using **TurtleBot3** in a selected **Gazebo world**. The workflow includes mapping the environment using teleoperation and navigating autonomously from point A to point B using the Navigation2 stack.

---

## ✅ Task Overview

**Objective**:  
Create a map of a selected environment using TurtleBot3 and perform autonomous navigation from a user-defined start point (A) to a goal point (B) using RViz and the Navigation2 stack.

---

## 🛠️ System Requirements

- **ROS 2 Humble** (on Ubuntu 22.04)
- **Gazebo (Fortress or default with Humble)**
- **TurtleBot3 Packages**
- **RViz2**
- **teleop_twist_keyboard**

---

## 📦 Package Setup

### 1. Clone Repositories

```bash
cd ~/ros2_ws/src
# Clone Gazebo worlds
git clone https://github.com/mlherd/Dataset-of-Gazebo-Worlds-Models-and-Maps.git
# Clone TurtleBot3 packages
git clone https://github.com/ROBOTIS-GIT/turtlebot3.git
```

---

### 2. Install Required Dependencies

```bash
sudo apt update
sudo apt install ros-humble-navigation2 ros-humble-nav2-bringup \
                 ros-humble-slam-toolbox ros-humble-turtlebot3* \
                 ros-humble-teleop-twist-keyboard
```

### 3. Set Environment Variables

Add to your `~/.bashrc`:

```bash
export TURTLEBOT3_MODEL=waffle_pi
source ~/.bashrc
```

---

## 🌍 Select and Modify Gazebo World

- Navigate to `Dataset-of-Gazebo-Worlds-Models-and-Maps/worlds/`
- Choose **any of the first five** `.world` files.
- Open in any editor and **remove non-static elements** (like moving people, vehicles, etc.)

---

## 🗺️ Mapping the Environment

### Launch Gazebo with the Selected World:

```bash
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py world:='absolute/path/to/your.world'
```

### Launch SLAM Toolbox:

```bash
ros2 launch turtlebot3_cartographer cartographer.launch.py use_sim_time:=true
```

### Teleoperate the Robot:

```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

### Save the Map:

After covering the environment:

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

## 🎬 Demonstration Video

The video [`working.mp4`](./working.mp4) shows:

- Mapping via teleoperation
- Saving the map
- Starting navigation stack
- Sending goal in RViz
- Terminal commands

⏺️ **All terminal commands are clearly shown in the video.**

---

## 📁 Directory Structure

```
task_3/
├── src/
│   ├── turtlebot3/...
│   ├── Dataset-of-Gazebo-Worlds-Models-and-Maps/...
│   └── other custom packages
├── map/
│   ├── my_map.yaml
│   └── my_map.pgm
├── working.mp4
└── README.md
```

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
