# SCOUT_V2
## SCOUT V2 menual



## Basic usage of the ROS package

1. Install dependent ROS packages

```
$ sudo apt install ros-melodic-teleop-twist-keyboard
$ sudo apt-get install ros-melodic-joint-state-publisher-gui
$ sudo apt install ros-melodic-ros-controllers
```

Change ros-melodic-* in the command to ros-kinetic-* if you're using ROS Kinetic.

2. Clone the packages into your catkin workspace and compile

```
$ cd ~/catkin_ws/src
$ git clone -b scout_v2 --depth 1 https://github.com/agilexrobotics/scout_ros.git	
$ git clone -b scout_v2 --depth 1 https://github.com/agilexrobotics/agx_sdk.git
$ cd ..
$ catkin_make
```

$catkin_make 까지 실행 완료 해야함 

- 만약 asio.hpp 오류가 발생할 때

- ```
  $sudo apt-get install libasio-dev
  ```



3. Setup Webots simulation    

* Install Webots R2020a-rev1 (download from https://cyberbotics.com/ )

* Install Webots ROS package

  ```
  $ sudo apt install ros-melodic-webots-ros
  ```

* Set WEBOTS_HOME variable, add the following line to your "~/.bashrc"

  ```
  export WEBOTS_HOME=/usr/local/webots
  ```



### Setup CAN-To-USB adapter 

1. Enable gs_usb kernel module

   ```
   $ sudo modprobe gs_usb
   ```

2. Bringup can device

```
$ sudo ip link set can0 up type can bitrate 500000
```

3. If no error occured during the previous steps, you should be able to see the can device now by using command

```
$ ifconfig -a
```

4. Install and use can-utils to test the hardware

```
$ sudo apt install can-utils
```

5. Testing command

```
# receiving data from can0
$ candump can0
# send data to can0
$ cansend can0 001#1122334455667788
```







## SCOUT Robot teleop



우선 로봇전원을 킨 후에 컨트롤러 전원을 켜준다

컨트롤러의 두번째 레버스위치를 위로 올려준다

그럼 로봇은 노드를 받을 준비가 완료됐다



- 각 터미널마다(새로운 터미널창마다) 아래 명령어 두줄 실행 (~/.bashrc 에 삽입해 항상 실행해주는 것도 좋음)

```
$source ~/catkin_ws/devel/setup.bash
$rospack profile
```

로봇과 컴퓨터를 캔을 이용해 연결 후

- 캔 bringup을 위해 아래 명령어 실행 

```
$ sudo ip link set can0 up type can bitrate 500000
```

- 캔을 이용해 연결하면 아래 명령어를 이용해 스카웃 브링업

```
$ roslaunch scout_bringup scout_minimal.launch
```

- 키보드 조작을 위해 아래 명령어 실행

```
$ roslaunch scout_bringup scout_teleop_keyboard.launch
```



