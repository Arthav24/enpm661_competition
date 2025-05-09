# ENPM661 Competition

This repository contains the source code and configuration files for the ENPM661 competition project. The project is built using ROS 2 Jazzy and leverages various dependencies to simulate and control turtlebot4.

## Dependencies

The project depends on the following ROS 2 packages:

- `ament_cmake`
- `geometry_msgs`
- `ros_gz_interfaces`
- `rclcpp`
- `nav_msgs`
- `tf2`
- `sensor_msgs`

Ensure these dependencies are installed in your ROS 2 environment.

## Installation and Launching Gazebo

### Native ROS-jazzy install
1. Clone the repository into your working directory:
   ```bash
   https://github.com/koustubh1012/enpm661_competition.git
2. Build the package using Colcon build
    ```bash
    colcon build --event-handlers console_cohesion+
3. Source the overlay and underlay

    ```bash
    source /opt/ros/jazzy/setup.bash
    source install/setup.bash
4. Launch the Gazebo setup using the launch file
    ```bash
    ros2 launch enpm661_competition turtlebot4_comp.launch.py
### Docker

1. To build the docker image using docker compose
    ```bash
    USERNAME_DOCKER=docker_user USERUID=$(id -u) USERGID=$(id -g) docker compose -f docker/enpm661-comp.yml build

2. To make container
    ```bash
    USERNAME_DOCKER=docker_user USERUID=$(id -u) USERGID=$(id -g) docker compose -f docker/enpm661-comp.yml run --rm enpm661-comp-docker
NOTE: use sudo inside docker just like native.

## Connecting to TurtleBot4
1. Connect to same Wifi network of robot. ping and verify the connection.
2. Setup environment in container to see topics from robot
    ```bash
    source /home/docker_user/config/tb4_setup.bash
    ros2 daemon stop
    ros2 daemon start
3. Check topic availability
    ```bash
    ros2 topic list
if everything is done right then you will be able to see topics

```bash
/parameter_events
/rosout
/tb4_1/battery_state
/tb4_1/cmd_audio
/tb4_1/cmd_lightring
/tb4_1/cmd_vel
/tb4_1/cmd_vel_unstamped
/tb4_1/diagnostics
/tb4_1/dock_status
/tb4_1/function_calls
... more
```

### Teleoperation

The tutlebot4 subscribes to cmd_vel in namespace. For eg if namespace of robot is tb4_1 then topics will be /tb4_1/<topics>.

Example to teleop TB4 with namespace tb4_1

```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -p stamped:=true -r /cmd_vel:=/tb4_1/cmd_vel
```
### Resetting odom
To reset the odom and odom $\rightarrow$ base_link tf
```bash
ros2 service call /tb4_1/reset_pose irobot_create_msgs/srv/ResetPose "{pose: {position: {x: 0.0, y: 0.0, z: 0.0}, orientation: {x: 0.0, y: 0.0, z: 0.0, w: 1.0}}}"
```


## Autonomous Navigation

1. Write your script to navigate turtlebot4 through the maze. Make sure to modify CMakeLists.txt file accordingly
