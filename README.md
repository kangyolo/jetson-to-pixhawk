# Jetson Nano to Pixhawk
The connection using Mavlink and for these methods I've tried Dronekit and ROS. This documentation consists both of them.

- Ubuntu 20.04
- Jetpack 4.6
- MicroSD 128 GB Sandisk A2

## ROS Noetic MAVROS
First things first install ROS Noetic based on these articles on [ros wiki](http://wiki.ros.org/noetic/Installation/Ubuntu)
It is recommended to install ROS-Dekstop or ROS-Base only, the full version of ROS will take a lot of storages.
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
setup the ros installation
```
source /opt/ros/noetic/setup.bash
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

connect the pixhawk to jetson using micro USB, the connection will establish as `/dev/ttyACM0`, you can verify using `ls /dev/tty*`
before connect to the pixhawk, we have to set permission for the `/dev/ttyACM0` using user dialout, replace the `<username>` with your own username
```
sudo chmod +x /dev/ttyACM0
sudo usermod -a -G dialout <username>
```
then we can access the pixhawk with mavros, for standard pixhawk 2.4 using `apm.launch` and pixhawk cube with `apm2.launch`
```
roslaunch mavros apm.launch
```
open new terminal to monitor the ros node 
```
rostopic list
rostopic echo <the node that you want to monitor e.g./mavros/state>
```
to test arming the motor, please set the precheck arm in Mission Planner, and complete the pre-arming check then we can try to arm the motor
```
rosrun mavros mavsafety arm
```
## Dronekit
[Dronekit](https://dronekit-python.readthedocs.io/en/latest/) is a python library to interact with MAVLink, let's get it
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python-dev
sudo pip install future
sudo apt-get install screen python-lxml
sudo pip install pyserial dronekit MAVProxy
```
connect the pixhawk using USB
```
sudo mavproxy.py --master /dev/ttyACM0
```
then will come up this message if completed

```
Connect /dev/ttyACM0 source_system=255
Log Directory:
Telemetry log: mav.tlog
Waiting for heartbeat from /dev/ttyACM0
MAV> Detected vehicle 1:1 on link 0
online system 1
STABILIZE> Mode STABILIZE
AP: PreArm: RC not found
AP: PreArm: Hardware safety switch
fence present
Servo volt 1.0
AP: ArduCopter V4.4.0 (502702df)
AP: ChibiOS: 1ec9f168
AP: Pixhawk1 002E002F 32385109 34343039
AP: RCOut: PWM:1-14
AP: IMU0: fast sampling enabled 8.0kHz/1.0kHz
AP: Frame: QUAD/X
```

By: [Oki Aryawan](https://www.instagram.com/oki_aryawan/)
