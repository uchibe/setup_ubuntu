# Setup Ubuntu
Ubuntu 22.04.5 LTS (Jammy Jellyfish) の設定

## ROS2
[ROS 2 Humble](https://docs.ros.org/en/humble/index.html)を公式の手順に従ってインストールする。https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html を参照。Set locale, Setup Sourcesを実行後にInstall ROS2 packagesの
```shell-session
$ sudo apt install ros-humble-desktop
```
を実行してROS2をインストール。.bashrcに
```bash
source /opt/ros/humble/setup.bash
```
を追記する。

最後にROS2の依存関係ツールやgazebo, rqtをインストールする。
```shell-session
$ sudo apt install python3-colcon-common-extensions -y
$ sudo apt install python3-argcomplete
sudo apt install gazebo
sudo apt install ros-humble-gazebo-*
sudo apt install ros-humble-rqt-*
```

動作確認のためターミナルを一つ開き
```shell-session
$ ros2 run demo_nodes_cpp talker
```
を実行する。次にもう一つターミナルを開き
```shell-session
$ ros2 run demo_node_py listener
```
を実行し通信ができていれば大丈夫。


## CRANE X7
```shell-session
$ sudo apt install git
$ sudo apt install python3-rosdep
$ sudo apt install colcon 
```

```
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
git clone -b ros2 https://github.com/rt-net/crane_x7_ros.git
git clone -b ros2 https://github.com/rt-net/crane_x7_description.git
git clone -b humble https://github.com/ros-controls/gz_ros2_control.git
rosdep update
rosdep install -r -y -i --from-paths .
```


## Setup
```shell-session
~$ git clone https://github.com/RealManRobot/ros2_rm_robot.git
~$ cd ros2_rm_robot/rm_install/scripts
~/ros2_rm_robot/rm_install/scripts$ sudo ./ros2_install.sh
~/ros2_rm_robot/rm_install/scripts$ sudo ./moveit2_install.sh
~$ cd ros2_rm_robot/rm_driver/lib
~/ros2_rm_robot/rm_driver/lib$ sudo ./lib_install.sh
```
