# GEONJI-3D: A Multimodal Dataset for 3D Perception in Unstructured Outdoor Environments

GEONJI-3D is a multimodal dataset collected mainly in the Geonji Mountain Research Forest at Jeonbuk National University. It provides synchronized RGB images, depth images, LiDAR point clouds, IMU measurements, and robot state data recorded on a tracked ground robot in the ROS 2 bag format.

The dataset contains two driving scenarios with forest walking trails, rough and irregular terrain, vegetation, fallen branches, revisited sections, and dynamic obstacles such as pedestrians. It is intended for research on multimodal sensor fusion, terrain perception, traversability estimation, localization and mapping, and robust outdoor navigation.

## Environment and format

- Ubuntu 22.04
- ROS 2 Humble
- ROS 2 bag storage: MCAP (`mcap`)

## Platform and sensors

| Component | Model | Nominal rate | Main specification |
|---|---|---:|---|
| Tracked mobile robot | AgileX BUNKER Pro 2.0 | 50Hz | Ground robot platform for rough terrain |
| Stereo RGB-D camera | Stereolabs ZED 2 | 15 Hz | 110° (H) × 70° (V) × 120° (D) FoV; 0.3–20 m depth range |
| 3D LiDAR | Ouster OS0-128 | 10 Hz | 128 channels; 90° vertical FoV |
| External IMU | WitMotion HWT905-TTL | 200 Hz | 9-DoF accelerometer, gyroscope, and magnetometer |

## Dataset download

Each scenario can be downloaded from Google Drive using the links below.

<table border="1">
  <thead>
    <tr>
      <th>Item</th>
      <th>Scenario 1</th>
      <th>Scenario 2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Download</th>
      <td><a href="https://drive.google.com/drive/folders/1WJrB4zXY_cY8xtSm09BSgamXTaApj62v?usp=drive_link">Download</a></td>
      <td><a href="https://drive.google.com/drive/folders/1M7H7Dij7eRwsSq8pI9SZzHWVae10d8Ln?usp=drive_link">Download</a></td>
    </tr>
    <tr>
      <th>Environment</th>
      <td>Relatively wide and flat forest walking trail</td>
      <td>Rough forest path with irregular terrain, fallen branches, dense vegetation, and revisited sections</td>
    </tr>
    <tr>
      <th>Bag directory</th>
      <td><code>outdoor1</code></td>
      <td><code>outdoor2</code></td>
    </tr>
    <tr>
      <th>MCAP file</th>
      <td><code>outdoor1_0.mcap</code></td>
      <td><code>outdoor2_0.mcap</code></td>
    </tr>
    <tr>
      <th>Size</th>
      <td>36.0 GiB</td>
      <td>45.1 GiB</td>
    </tr>
    <tr>
      <th>Duration</th>
      <td>265.076 s (04:25.076)</td>
      <td>317.883 s (05:17.883)</td>
    </tr>
    <tr>
      <th>Total messages</th>
      <td>153,185</td>
      <td>192,772</td>
    </tr>
  </tbody>
</table>

Both scenarios contain dynamic obstacles such as pedestrians. The recorded start times are 2026-08-27 06:34:13 KST for `outdoor1` and 2026-08-27 06:41:21 KST for `outdoor2`.

## Data statistics

| Bag directory | RGB frames | Depth frames | LiDAR scans | External IMU samples | Wheel odometry messages |
|---|---:|---:|---:|---:|---:|
| `outdoor1` | 3,428 | 3,425 | 2,116 | 53,088 | 13,195 |
| `outdoor2` | 4,302 | 4,262 | 2,663 | 63,590 | 15,775 |

The sensor rates below are nominal configuration rates. Message counts are the values actually stored in each bag, so the count divided by the bag duration may differ from the nominal rate because of sensor startup, transport timing, or dropped frames.

## ROS 2 topics

| Topic | Message type | Nominal rate | Description | `outdoor1` count | `outdoor2` count |
|---|---|---:|---|---:|---:|
| `/odom` | <code>nav_msgs/msg<br>/Odometry</code> | 50 Hz | Wheel odometry | 13,195 | 15,775 |
| `/imu` | <code>sensor_msgs/msg<br>/Imu</code> | 200 Hz | WitMotion HWT905-TTL IMU measurement | 53,088 | 63,590 |
| `/ouster/points` | <code>sensor_msgs/msg<br>/PointCloud2</code> | 10 Hz | Ouster OS0-128 LiDAR point cloud | 2,116 | 2,663 |
| `/ouster/imu` | <code>sensor_msgs/msg<br>/Imu</code> | 100 Hz | Ouster internal IMU measurement | 26,476 | 31,762 |
| `/ouster/metadata` | <code>std_msgs/msg<br>/String</code> | Once | Ouster sensor metadata | 1 | 1 |
| <code>/zed/zed_node/rgb<br>/color/rect/image</code> | <code>sensor_msgs/msg<br>/Image</code> | 15 Hz | Rectified RGB image | 3,428 | 4,302 |
| <code>/zed/zed_node/rgb/color<br>/rect/image/camera_info</code> | <code>sensor_msgs/msg<br>/CameraInfo</code> | 15 Hz | RGB camera calibration and projection data | 3,923 | 4,651 |
| <code>/zed/zed_node/depth<br>/depth_registered</code> | <code>sensor_msgs/msg<br>/Image</code> | 15 Hz | Depth image registered to the RGB camera frame | 3,425 | 4,262 |
| <code>/zed/zed_node/depth<br>/depth_registered/camera_info</code> | <code>sensor_msgs/msg<br>/CameraInfo</code> | 15 Hz | Registered depth camera information | 3,869 | 4,646 |
| `/zed/zed_node/imu/data` | <code>sensor_msgs/msg<br>/Imu</code> | 100 Hz | ZED 2 internal IMU measurement | 26,296 | 31,464 |
| `/zed/zed_node/odom` | <code>nav_msgs/msg<br>/Odometry</code> | 15 Hz | ZED odometry | 3,926 | 4,652 |
| `/tf` | <code>tf2_msgs/msg<br>/TFMessage</code> | Variable | Dynamic coordinate-frame transforms | 13,438 | 25,000 |
| `/tf_static` | <code>tf2_msgs/msg<br>/TFMessage</code> | Event-based | Static coordinate-frame transforms | 3 | 3 |
| `/robot_description` | <code>std_msgs/msg<br>/String</code> | Once | Robot description | 1 | 1 |

## Usage

Each bag directory must contain its MCAP file and `metadata.yaml` in the following structure:

```text
outdoor1/
├── metadata.yaml
└── outdoor1_0.mcap

outdoor2/
├── metadata.yaml
└── outdoor2_0.mcap
```

Inspect or play a bag with ROS 2 Humble:

```bash
source /opt/ros/humble/setup.bash
ros2 bag info outdoor1
ros2 bag play outdoor1
```

Use `outdoor2` in place of `outdoor1` for Scenario 2. If ROS 2 reports that the `mcap` storage plugin is unavailable, install the ROS 2 Humble MCAP storage package before opening the bags.

## Version history

| Version | Description | Release date |
|---|---|---|
| v2.0 | Replaced the previous recordings with two new MCAP scenarios (`outdoor1` and `outdoor2`) and added detailed bag and topic statistics | 2026-08-29 |
| v1.0 | Initial release with four outdoor scenarios | 2026-06-13 |
