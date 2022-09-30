# SCOUT_V2
## SCOUT V2 설정 방법 menual 



## Basic usage of the ROS package

1. Install dependent ROS packages

```
$ sudo apt install -y libasio-dev
$ sudo apt install -y ros-$ROS_DISTRO-teleop-twist-keyboard
```

Change ros-melodic-* in the command to ros-kinetic-* if you're using ROS Kinetic.

2. Clone the packages into your catkin workspace and compile

```
$ cd ~/catkin_ws/src
$ git clone https://github.com/agilexrobotics/ugv_sdk.git  
$ git clone https://github.com/agilexrobotics/scout_ros.git
$ cd ..
$ catkin_make
```

$catkin_make 까지 실행 완료 해야함 

- 만약 asio.hpp 오류가 발생할 때

- ```
  $sudo apt-get install libasio-dev
  ```

3. Setup CAN-To-USB adapter

* Enable gs_usb kernel module(If you have already added this module, you do not need to add it)
    ```
    $ sudo modprobe gs_usb
    ```
    
* first time use scout-ros package
   ```
   $ rosrun scout_bringup setup_can2usb.bash
   ```
   
* if not the first time use scout-ros package(Run this command every time you turn off the power) 
   ```
   $ rosrun scout_bringup bringup_can2usb.bash
   ```
   
* Testing command
    ```
    # receiving data from can0
    $ candump can0
    ```

4. Launch ROS nodes

* Start the base node for scout

    ```
    $ roslaunch scout_bringup scout_robot_base.launch 
    ```


* Start the base node for scout-mini

    ```
    $ roslaunch scout_bringup scout_mini_robot_base.launch
    ```


* Start the keyboard tele-op node

    ```
    $ roslaunch scout_bringup scout_teleop_keyboard.launch
    ```






위 메뉴얼의 원본 >> https://github.com/agilexrobotics/scout_ros


# REMOTE PC에서 SCOUT과 직접 연결된 PC조작 방법

1. REMOTE PC의 bashrc 에서 ROS_HOSTNAME, ROS_MASTER_URI를 remote ip로 변경하기

2. SCOUT과 직접 연결된 PC의 bashrc에서 ROS_MASTER_URI는 remote ip로, ROS_HOSTNAME은은 SCOUT과 직접 연결된 PC의 ip로 변경하기

3. roscore 실행(Master pc 터미널에서)

   ```
   $ roscore
   ```

4. scout과 직접 연결된 PC(rilab-02)에 원격접속

   ```
   $ ssh scout@{scout연결된PC ip}
   만약 연결이 안된다면 scout pc를 재부팅 해보기
   ```

   원격 접속한 Robot pc터미널 창에서 실행

   ```
   $ sudo ip link set can0 up type can bitrate 500000
   ```

   ```
   $ roslaunch scout_bringup scout_mini_robot_base.launch
   ```

5. REMOTE PC터미널 창에서 조종 명령어 실행 (현재 ssh 터미널에서 실행해야 가능함)

   ```
   $ roslaunch scout_bringup scout_teleop_keyboard.launch
   ```

   

위 과정을 하기 위해선 로봇과 컨트롤러의 전원이 켜지고 로봇과 누크(작은 PC)가 연결되어 있어야 합니다.


