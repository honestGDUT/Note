## 安装所需
    1.PX4源码
    2.Gazebo
    3.MAVROS
    5.TurtleBOt3小车

需要知道如何修改px4的model模型，world世界。通过修改sdf文件实现，然后在相关的launch文件中将使用的model和world修改为自定义的sdf文件。最好了解一下sdf文件中各个参数的含义，以及如何编写。

mavros_posix_sitl.launch 是PX4生态中用于启动软件在环（SITL）仿真的核心ROS启动文件，它整合了PX4飞控、Gazebo仿真环境和MAVROS通信节点，为无人机算法开发和测试提供完整的仿真环境。

1. 配置好px4的模型，世界以及可以增加小车、摄像头、雷达、GPS等一些其他模型或世界。然后roslaunch
2. 在自己的工作空间下，创建目标跟踪降落的功能包

PX4无人机添加摄像头以及配置的修改
默认的PX4无人机是不带摄像头的，我们需要修改配置文件使其带上一个摄像头。

修改mavros_posix_sitl.launch </br>

    复制一份配置文件作为副本，在副本中修改（后续roslaunch运行这个副本）

    ```
    cd ~/PX4_Firmware/launch
    cp mavros_posix_sitl.launch mavros_posix_sitl_cp.launch		# 不修改原始无人机文件，复制一个副本进行修改
    gedit mavros_posix_sitl_cp.launch
    ```

作如下改动
1. 添加
    ```
    <arg name="my_model" default="iris_downward_depth_camera"/>
```
2. 修改
    ```
    <arg name="sdf" default="$(find mavlink_sitl_gazebo)/models/$(arg vehicle)/$(arg vehicle).sdf"/> 
    ```
    改为
    ```
    <arg name="sdf" default="$(find mavlink_sitl_gazebo)/models/$(arg my_model)/$(arg my_model).sdf"/> 

    ```
注意添加的代码需要在修改的上方