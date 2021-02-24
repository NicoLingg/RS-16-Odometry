# rs-16-odometry
Extract odometry data from the RS-16 3D Lidar


## Step 1: Install rslidar_sdk

Navigate to your ws 

```shell
cd src/
git clone https://github.com/RoboSense-LiDAR/rslidar_sdk.git
cd rslidar_sdk
git submodule init
git submodule update
```
Dependencies that have to be installed:
```shell
sudo apt-get update
sudo apt-get install -y libyaml-cpp-dev
sudo apt-get install -y  libpcap-dev
sudo apt-get install -y libprotobuf-dev protobuf-compiler
```

Compile with ROS catkin tools

(1) Open the *CMakeLists.txt* in the project，modify the line  on top of the file **set(COMPILE_METHOD ORIGINAL)** to **set(COMPILE_METHOD CATKIN)**.

```cmake
#=======================================
# Compile setup (ORIGINAL,CATKIN,COLCON)
#=======================================
set(COMPILE_METHOD CATKIN)
```

(2) Rename the file *package_ros1.xml*  in the rslidar_sdk to *package.xml*

(3) Go back to the root of workspace, run the following commands to compile and run.

```sh
catkin_make
source devel/setup.bash
roslaunch rslidar_sdk start.launch
```

More details can be found here: https://github.com/RoboSense-LiDAR/rslidar_sdk

## Step 2: Config connection 

The RS-LiDAR-16 adopts the UDP protocol and communicates with the computer through 100Mbps Ethernet. For this you need to plug in the sensor with an ethernet cable to your computer. To establish a communication between the sensor and the computer, the IP address of the computer should be set at the same network segment of that of the sensor. By default: 192.168.1.102 and subnet mask: 255.255.255.0. The IP adress of the sensor is by default 192.168.1.200.

To chang the IP address for Ubuntu, go to Wired Connection/Wired Settings, a new window will open. Click on setting (gear symbol) and select the IPv4 tab. Change IPv4 Method to Manual and enter 192.168.1.102 in the 'Address' field and 255.255.255.0 in 'Netmask; field respectively. Click apply.

Open a new terminal and try to ping the sensor:

```sh
ping 192.168.1.200
```

The last thing you need to change is the sensor type in rslidar_sdk/config/config.yaml line 16, depending on which sensor you have. These are your options: RS16, RS32, RSBP, RS128, RS80, RSM1, RSHELIOS. 

## Step 3: Install hdl_graph_slam

Install dependencies:
```sh
# for melodic
sudo apt-get install ros-melodic-geodesy ros-melodic-pcl-ros ros-melodic-nmea-msgs ros-melodic-libg2o
cd /src
git clone https://github.com/koide3/hdl_graph_slam.git
git clone https://github.com/koide3/ndt_omp.git
git clone https://github.com/SMRT-AIST/fast_gicp.git --recursive
```
Compile 
```sh
cd ..
catkin_make
source devel/setup.bash
```

More details can be found here: https://github.com/koide3/hdl_graph_slam


## Step 4: Download modified launch file
Download the launch file in this repo (hdl_graph_slam_robosense_rs16.launch) and copy it into src/hdl_graph_slam/launch
This launch file was. modified to work with the rs-16 3D Lidar


## Step 3: Run darknet 3d and realsense camera 

Launch rslidar_sdk
```shell
roslaunch rslidar_sdk start.launch
```

Launch the hdl_graph_slam
```shell
roslaunch hdl_graph_slam hdl_graph_slam_robosense_rs16.launch
```


