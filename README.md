# Setup Ubuntu
Ubuntu 22.04.5 LTS (Jammy Jellyfish) の設定

## ROS2
[ROS 2 Humble](https://docs.ros.org/en/humble/index.html)を公式の手順に従ってインストールする。https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html を参照し、Set locale, Setup Sourcesを実行する。次にInstall ROS2 packagesの
```shell-session
sudo apt install ros-humble-desktop
```
を実行してROS2をインストールする。~/.bashrcに
```bash
source /opt/ros/humble/setup.bash
```
を追記する。

最後にROS2の依存関係ツールやgazebo, rqtをインストールする。
```shell-session
sudo apt install python3-colcon-common-extensions -y
sudo apt install python3-argcomplete
sudo apt install gazebo
sudo apt install ros-humble-gazebo-*
sudo apt install ros-humble-rqt-*
```

### 動作確認
動作確認のためターミナルを一つ開き
```shell-session
ros2 run demo_nodes_cpp talker
```
を実行する。次にもう一つターミナルを開き
```shell-session
ros2 run demo_node_py listener
```
を実行し通信ができていれば大丈夫。


## CRANE X7
### CycloneDDSのインストール
https://rt-net.jp/humanoid/archives/5069 にしたがってCycloneDDSを予めインストールする。
```shell-session
sudo apt update
sudo apt install ros-$ROS_DISTRO-rmw-cyclonedds-cpp
```
~/.bashrcに
```bash
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
```
を追記する。

### 依存ファイルのインストール
```shell-session
sudo apt install git
sudo apt install python3-rosdep
sudo apt install colcon 
```

### CRANE X7のROS2環境のインストール
```shell-session
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src

git clone https://github.com/IntelRealSense/realsense-ros.git
git clone -b humble https://github.com/rt-net/crane_x7_ros.git
git clone -b humble https://github.com/rt-net/crane_x7_description.git
git clone -b humble https://github.com/ros-controls/gz_ros2_control.git
rosdep update
rosdep install -r -y -i --from-paths .
```

ビルド
```shell-session
cd ../
colcon build --symlink-install
```
~/.bashrcに
```bash
source ~/ros2_ws/install/setup.bash
```
を追記する。

### サンプルの確認
https://note.com/kan_hatakeyama/n/n1750880e392e

### 電流制御による重力補償のサンプルのインストール
DYNAMIXEL SDKのインストール
```shell-session
sudo apt install build-essential
cd ~
git clone https://github.com/ROBOTIS-GIT/DynamixelSDK.git
cd ~/DynamixelSDK/c++/build/linux64
make
sudo make install
```
Dynamixel WizardのインストールやROS2での設定の詳細は https://zenn.dev/tasada038/articles/422d3f7f2232bb を参照されたいがとりあえずスキップ。

### その他依存関係のインストール
```shell-session
sudo apt install libyaml-cpp-dev libeigen3-dev
```
 
### RTマニピュレータC++ライブラリのビルド&インストール
```shell-session
cd ~
git clone https://github.com/rt-net/rt_manipulators_cpp
cd rt_manipuators_cpp/rt_manipulators_lib
./build_install_library.bash
```
### 動作確認
まずは　https://github.com/rt-net/rt_manipulators_cpp/blob/main/samples/samples01/README.md をよく読む。
ロボットとPCをUSBで接続する。他にPCにUSB接続されたデバイスがなければ、新しく接続されたデバイスは　/dev/ttyUSB0　のはず。その場合ターミナル上で
```bash
sudo chmod 666 /dev/ttyUSB0
```
とする。永続的ににアクセス権限を付与する場合はターミナル上で
```bash
sudo usermod -aG dialout $USER
reboot
```
とする。リブートするので注意する。最後にロボットとPC間の通信遅延を最小にするためにターミナル上で
```bash
sudo chmod a+rw /sys/bus/usb-serial/devices/ttyUSB0/latency_timer
sudo echo 1 > /sys/bus/usb-serial/devices/ttyUSB0/latency_timer
```
とする。

https://github.com/rt-net/rt_manipulators_cpp/blob/main/samples/README.md にしたがって各種サンプルをコンパイル、実行する。
https://rt-net.jp/humanoid/archives/4398 には重力補償のサンプルプログラムを書き換える例が示されている。
