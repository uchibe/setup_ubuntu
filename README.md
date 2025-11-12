# Setup Ubuntu
Ubuntu 22.04.5 LTS (Jammy Jellyfish) の設定

## ROS2
[ROS 2 Humble](https://docs.ros.org/en/humble/index.html)を公式の手順に従ってインストールする。https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html を参照。

```bash
$ sudo apt install python3-colcon-common-extensions -y
$ sudo apt install python3-argcomplete
```

## Setup
```bash 
~$ git clone https://github.com/RealManRobot/ros2_rm_robot.git
~$ cd ros2_rm_robot/rm_install/scripts
~/ros2_rm_robot/rm_install/scripts$ sudo ./ros2_install.sh
~/ros2_rm_robot/rm_install/scripts$ sudo ./moveit2_install.sh
~$ cd ros2_rm_robot/rm_driver/lib
~/ros2_rm_robot/rm_driver/lib$ sudo ./lib_install.sh
```
