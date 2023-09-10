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

connect the pixhawk to jetson using micro USB, the connection will establish as `/dev/ttyACM0`, you can verify using `lsusb -l /dev/tty*`
before connect to the pixhawk, we have to set permission for the `/dev/ttyACM0` using user dialout, replace the `<username>` with your own username
```
sudo chmod +x /dev/ttyACM0
sudo usermod -a -G dialout <username>
sudo usermod -a -G <group> <username>
```
then we can access the pixhawk with mavros, for standard pixhawk 2.4 using `apm.launch` and pixhawk cube with `apm2.launch`
```
roslaunch mavros apm.launch
```
open new terminal to monitor the ros node 
```
rostopic list
rostopic echo <the node that you want to monitor e.g /mavros/state>
```
to test arming the motor, please set the precheck arm in Mission Planner, and comlete the pre-arming check then we can try to arm the motor
```
rosrun mavros mavsafety arm
```
## Dronekit
