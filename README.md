# Jetson Nano to Pixhawk
The connection using mavlink and for these method I've tried Dronekit and ROS. This documentation consist both of it.

Ubuntu 20.04
Jetpack 4.6

## ROS Noetic MAVROS
First things first install ROS Noetic based on these article on [ros wiki](http://wiki.ros.org/noetic/Installation/Ubuntu)
It is recommended to install ROS-Dekstop or ROS-Base only, the full version of ROS will take a lot of strorages.
```
sudo apt install ros-noetic-desktop
```
or run
```
sudo apt install ros-noetic-ros-base
```
then install the mavros package from [Ardupilot article](https://ardupilot.org/dev/docs/ros-install.html)
```
sudo apt-get install ros-noetic-mavros ros-noetic-mavros-extras
```
here we will need a Geographiclib, don't download it, just run from the mavros folder
```
cd /opt/ros/noetic/lib/mavros/
sudo ./install_geographiclib_datasets.sh
```
it will take about 10 minutes or more, just wait for it.

## Dronekit
