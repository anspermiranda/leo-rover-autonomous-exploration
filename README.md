# Leo Rover Autonomous Exploration, Mapping and Navigation

## Project Overview

This project focuses on developing an autonomous mobile robot using the Leo Rover platform capable of exploring an unknown environment, generating a map, navigating autonomously, and performing colour-based object pick-and-drop operations.

The robot uses ROS2 Navigation Stack (Nav2), SLAM, LiDAR sensing, frontier-based exploration, and localization techniques to move intelligently in unknown environments. The system was tested both in simulation and on the physical Leo Rover platform.

The robot is designed to:
- Explore unknown environments autonomously
- Generate occupancy grid maps using SLAM
- Navigate safely while avoiding obstacles
- Detect coloured objects
- Pick coloured objects
- Drop them into their respective coloured bins

The complete system was validated in:
- Simulation environment
- Real-world Leo Rover hardware platform

---

# Workspace Location

```bash
/home/student19/ros_ws_leo
```

---

# Before Opening Any Terminal

Always source the workspace before running any ROS2 command.

```bash
cd ~/ros_ws_leo
source install/setup.bash
```

---

# Initial Verification Steps

## 1. Check Available Topics

Use this command to verify active ROS2 topics:

```bash
ros2 topic list
```

---

## 2. Check Topic Values

To inspect topic data:

```bash
ros2 topic echo /<topic_name> --once
```

Example:

```bash
ros2 topic echo /scan --once
```

---

## 3. Check TF Transform Bridge

Verify transforms between frames:

```bash
ros2 run tf2_ros tf2_echo <from_frame> <to_frame>
```

Example:

```bash
ros2 run tf2_ros tf2_echo base_footprint laser
```

---

# System Launch Procedure

## Terminal 1 — Launch RPLiDAR

```bash
cd ~/ros_ws_leo
source install/setup.bash

ros2 launch rplidar_ros rplidar_a2m12_launch.py
```

### Purpose

This launches the LiDAR sensor.

### Important Note

The `/scan` topic only appears after launching the LiDAR.

So if `/scan` does not appear during:

```bash
ros2 topic list
```

before launching LiDAR, that is completely normal.

---

# Terminal 2 — Static Transform Publisher

```bash
cd ~/ros_ws_leo
source install/setup.bash

ros2 run tf2_ros static_transform_publisher 0 0 0 0 0 0 base_footprint laser
```

---

## Purpose

This creates a static transform between:
- `base_footprint`
- `laser`

This helps the robot understand where the LiDAR is positioned relative to the robot body.

Without this transform:
- TF messages may drop
- Localization becomes unstable
- The rover may behave incorrectly
- The robot may appear disconnected from sensor data

For simplicity:
- All coordinate values are kept as `0`

These values can later be modified based on the actual LiDAR mounting position.

---

# Terminal 3 — SLAM and Navigation

```bash
cd ~/ros_ws_leo
source install/setup.bash

ros2 launch leo_nav navigation.launch.xml
```

---

## Purpose

This launches:
- SLAM
- Navigation Stack (Nav2)
- Localization
- Path Planning

The robot can now:
- Build maps
- Navigate autonomously
- Receive navigation goals

---

# Terminal 4 — RViz Visualization

```bash
cd ~/ros_ws_leo
source install/setup.bash

rviz2
```

---

## Purpose

RViz is used for:
- Visualizing LiDAR scans
- Viewing generated maps
- Viewing robot model
- Monitoring navigation
- Sending navigation goals

---

## Initial Localization Procedure

Before navigation works correctly:

### Step 1 — Initialize Robot Pose

In RViz:
- Click **"2D Pose Estimate"**
- Click and drag on the robot position
- Ensure arrow direction matches the front side of the rover

This localizes the robot correctly.

---

### Step 2 — Send Navigation Goal

In RViz:
- Click **"Nav2 Goal"**
- Click and drag toward a target position

The robot should now autonomously move toward the goal.

---

# Terminal 5 — Autonomous Exploration

```bash
cd ~/ros_ws_leo
source install/setup.bash

ros2 launch leo_explorer explore.launch.py use_sim_time:=false
```

---

## Purpose

This launches autonomous frontier-based exploration.

The robot:
- Explores unknown regions
- Detects unexplored frontiers
- Moves autonomously
- Expands the generated map

This allows full autonomous environment exploration without manually sending goals.

---

# Terminal 6 — Save Generated Map

```bash
cd ~/ros_ws_leo
source install/setup.bash

mkdir -p ~/maps

ros2 run nav2_map_server map_saver_cli -f ~/maps/leo_map
```

---

## Important

Keep SLAM running while saving the map.

Otherwise:
- The map may save incorrectly
- Incomplete occupancy grids may be generated

---

# Autonomous Object Pick and Drop

## Objective

The final objective of the project is autonomous object handling in unknown environments.

The robot performs:
1. Autonomous exploration
2. Mapping
3. Localization
4. Navigation
5. Colour object detection
6. Pick operation
7. Bin identification
8. Correct colour-based object dropping

---

# Features

- ROS2-based autonomous navigation
- SLAM mapping
- Frontier exploration
- LiDAR integration
- TF transform management
- RViz visualization
- Autonomous goal navigation
- Real robot deployment
- Simulation testing
- Colour-based object sorting

---

# Technologies Used

- ROS2
- Nav2
- SLAM Toolbox
- RViz2
- TF2
- RPLiDAR
- Frontier Exploration
- Python
- Ubuntu Linux

---

# Simulation and Real Robot Testing

The complete system was validated in:
- Simulation environment
- Physical Leo Rover platform

Both systems demonstrated:
- Autonomous mapping
- Navigation
- Exploration
- Goal reaching
- Environment understanding

---

# Future Improvements

- Improved object detection
- Deep learning integration
- Dynamic obstacle avoidance
- Multi-room exploration
- Improved path optimization
- Autonomous manipulation arm integration

---

# Author

Ansper Miranda
