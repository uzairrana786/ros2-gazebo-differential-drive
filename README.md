# ROS 2 Differential Drive Robot Simulation using Gazebo

**Tags:** ![ROS 2 Jazzy](https://img.shields.io/badge/ROS_2-Jazzy-22314E?style=for-the-badge&logo=ros&logoColor=white)
![Gazebo Harmonic](https://img.shields.io/badge/Gazebo-Harmonic-FF6600?style=for-the-badge&logo=gazebo&logoColor=white)
![Ubuntu 24.04](https://img.shields.io/badge/Ubuntu-24.04_Noble-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white)
![URDF Xacro](https://img.shields.io/badge/Model-URDF_%2F_Xacro-007ACC?style=for-the-badge&logo=xml&logoColor=white)
![License](https://img.shields.io/badge/License-Apache_2.0-green?style=for-the-badge)

## Purpose & Learning Goals

I created this repository as a hands-on learning project to understand how mobile robots are modeled, simulated, and controlled in **ROS 2 Jazzy** and **Gazebo Harmonic**.

My main targets for this baseline project were:

* Learn how to build a custom robot model from scratch using **URDF / Xacro** with proper inertia, collision geometry, and ground clearance.

* Understand how Gazebo Harmonic's `DiffDrive` plugin works and how it differs from legacy Gazebo Classic plugins.

* Connect Gazebo simulation topics (`/cmd_vel`, `/odom`, `/clock`) to ROS 2 using `ros_gz_bridge`.

* Automate launching the world, spawning the robot, and starting the topic bridges using a single Python launch file.

## Requirements & Prerequisites

Before running this project, make sure your environment has the following installed:

* **Operating System:** Ubuntu 24.04 LTS (Noble Numbat) — native Linux or via WSL2 on Windows

* **ROS 2 Distribution:** ROS 2 Jazzy Jalisco (compatible with Ubuntu 24.04 LTS)

* **Simulator:** Gazebo Harmonic (compatible with ROS 2 Jazzy)

* **Required ROS 2 Packages:**

  * `ros-jazzy-ros-gz` (for `ros_gz_sim` and `ros_gz_bridge`)

  * `ros-jazzy-xacro`

  * `ros-jazzy-robot-state-publisher`

  * `ros-jazzy-teleop-twist-keyboard`

## Project Overview

This package (`my_diff_bot`) simulates a 3-wheeled differential-drive robot inside a custom SDF obstacle world:

* **Drive System:** Two actuated drive wheels (`8 cm` radius) controlled via `/cmd_vel` and a frictionless front caster wheel (`4 cm` radius) for balance.

* **Chassis (Robot main body) Geometry:** `0.5m x 0.3m x 0.15m` body with `8 cm` of ground clearance so the chassis never drags on the floor.

### Project Structure

```
my_diff_bot/
├── launch/
│   └── sim_robot.launch.py      # Launches Gazebo, robot_state_publisher, spawner, and bridge
├── urdf/
│   └── robot.urdf.xacro         # Robot links, joints, collision/inertia, and DiffDrive plugin
├── worlds/
│   └── simple_obstacle.sdf      # Custom ground plane and box obstacles
├── CMakeLists.txt
└── package.xml

```

## How to Build & Run

Open your **Ubuntu (Linux) or WSL2 terminal** before running the commands below.

### 1. Clone and Build

```
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
git clone https://github.com/uzairrana786/ros2-gazebo-differential-drive.git my_diff_bot

cd ~/ros2_ws
source /opt/ros/jazzy/setup.bash
colcon build --packages-select my_diff_bot
source install/setup.bash
```

### 2. Launch the Simulation

```
ros2 launch my_diff_bot sim_robot.launch.py
```

### 3. Drive the Robot

Open a second Linux/WSL2 terminal, source ROS 2, and run the keyboard teleop node:

```
source /opt/ros/jazzy/setup.bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

Use `i` (forward), `j` (left), `l` (right), `,` (reverse), and `k` (stop) to drive around the obstacles.

## Results & What Was Achieved

* Built a stable differential-drive URDF/Xacro model that spawns cleanly without clipping or dragging on the ground plane.

* Bridged ROS 2 and Gazebo Harmonic topics successfully (`/cmd_vel` commands move the wheels in real time and `/odom` publishes robot pose).

* Verified smooth steering and obstacle navigation using keyboard teleoperation.

### Demo & Screenshots

*(Add your screenshots or demo GIF/video in a `media/` folder and link them below)*

## Next Steps

Now that the locomotion and ROS-Gazebo bridge baseline is working, I am building a separate follow-up repository on top of this base to add a **2D LiDAR sensor**, visualize laser scans in **RViz2**, and work toward **SLAM** and autonomous navigation.
