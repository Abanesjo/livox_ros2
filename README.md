# Livox ROS Driver 2 - MID360

This repository is a ROS2 Humble package for a Livox MID360 LiDAR. It keeps the
driver focused on MID360 bringup and publishes PointCloud2 data by default.

## Prerequisites

- Ubuntu 22.04
- ROS2 Humble
- Livox-SDK2 installed with `liblivox_lidar_sdk_shared.so` and headers visible
  to CMake

## Build

From the workspace root:

```shell
source /opt/ros/humble/setup.bash
colcon build --packages-select livox_ros_driver2
```

## Run

Source the workspace, then start the MID360 driver:

```shell
source install/setup.bash
ros2 launch livox_ros_driver2 bringup.launch.xml
```

Start the driver with RViz:

```shell
ros2 launch livox_ros_driver2 bringup.launch.xml rviz:=true
```

The launch file always sets `xfer_format=0`, so the LiDAR point cloud is
published as `sensor_msgs/msg/PointCloud2` on `/livox/lidar`. IMU data is
published on `/livox/imu`.

## Configuration

The MID360 network configuration lives in:

```text
config/MID360_config.json
```

Update the host IP and LiDAR IP values in that file to match the network
interface connected to the sensor. The default launch file passes this config
to the driver through the `user_config_path` parameter.

The RViz display config lives in:

```text
config/display_point_cloud.rviz
```

## Launch Parameters

`launch/bringup.launch.xml` sets the driver parameters directly:

| Parameter | Value | Notes |
| --- | --- | --- |
| `xfer_format` | `0` | Always publish PointCloud2. |
| `multi_topic` | `0` | Publish all LiDAR data on `/livox/lidar`. |
| `data_src` | `0` | Raw LiDAR input. |
| `publish_freq` | `10.0` | Point cloud publish rate in Hz. |
| `output_data_type` | `0` | Publish to ROS2 topics. |
| `frame_id` | `livox_frame` | Frame used by PointCloud2 messages and RViz. |
| `rviz` | `false` | Launch argument; set `rviz:=true` to start RViz. |

The generated custom Livox message definitions remain in `msg/`, but the
provided bringup path does not use them.
