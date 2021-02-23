# i3dr_kinova-ros
ROS driver for using stereo camera systems from Industrial 3D Robotics with Kinova robotic arms.  

***WARNING: Currently untested on real Kinova robot, use with caution!***

## Status
![ROS Build](https://github.com/i3drobotics/i3dr_kinova-ros/workflows/ROS%20Build/badge.svg?event=push)

## Install

For an easy setup, a rosinstall file is provided in 'install' folder of this repo which can be used to get this package and it's dependent ros packages in your workspace. 
In your ROS workspace use the following command:
```
wstool init src https://raw.githubusercontent.com/i3drobotics/i3dr_kinova-ros/master/install/i3dr_kinova_https.rosinstall
```
If you already have a wstool workspace setup then use the following command instead:
```
wstool merge -t src https://raw.githubusercontent.com/i3drobotics/i3dr_kinova-ros/master/install/i3dr_kinova_https.rosinstall
wstool update -t src
```

If you do not use wstool, you can download the packages using the following command:
```
cd PATH_TO_ROS_WS/src
git clone https://github.com/i3drobotics/i3dr_stereo_camera-ros.git
git clone https://github.com/i3drobotics/i3dr_pcl_tools-ros.git
git clone https://github.com/i3drobotics/i3dr_titania-ros.git
git clone https://github.com/i3drobotics/camera_control_msgs.git
git clone https://github.com/i3drobotics/pylon_camera.git
git clone https://github.com/i3drobotics/i3dr_kinova-ros.git
git clone https://github.com/i3drobotics/kinova-ros.git
```

The pylon_camera package used by this package requires the pylonSDK to be installed on your system. Please download and install the pylon debian package for your architecture from [here](https://www.baslerweb.com/en/sales-support/downloads/software-downloads/#type=pylonsoftware;language=all;version=all;os=all)  
Look for 'pylon 6.x.x Camera Software Suite Linux x86 (64 Bit) - Debian Installer Package'

In order to build the package, you need to configure rosdep (i.e. the ROS command-line tool for checking and installing system dependencies for ROS packages) such that
it knows how to resolve this dependency. This can be achieved by executing the following commands:

```
sudo sh -c 'echo "yaml https://raw.githubusercontent.com/i3drobotics/pylon_camera/master/rosdep/pylon_sdk.yaml " > /etc/ros/rosdep/sources.list.d/15-plyon_camera.list'
rosdep update
```

To install package dependences use rodep:
```
rosdep install --from-paths src --ignore-src -r -y
```

Build using catkin (tested with catkin_make and catkin_build):
```
catkin_make
or
catkin build
```

## Run
### Run Titania
Plug in the Titania stereo camera to your machine and use the following launch file:
```
roslaunch i3dr_titania titania.launch rviz:=true
```
### Gazebo titania kinova simulation
To run a gazebo simulation of the stereo camera system mounted on a Kinova robotic arm run the following launcher:
```
roslaunch i3dr_kinova_control kinova_titania.launch rviz:=true
```

This can also be run with Moveit to control the robot:
```
roslaunch i3dr_kinova_control kinova_titania.launch rviz:=true moveit:=true
```
### Mapping
To mozaic point clouds to create a map as the robot moves, after running the 'kinova_titania' launcher run:
```
roslaunch i3dr_kinova_control mozaic.launch
```

Once the map is complete you can save the map to a file using the following command:
```
rosrun i3dr_pcl_tools save_pcl _pcl2_input:=/i3dr_titania/map_points2 _pcl_filepath:=path/to/file.ply
```