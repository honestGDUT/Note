## 1.启动一个gazebo仿真

```
make px4_sitl_default gazebo
```

## 2.启动mavros
```
roslaunch mavros px4.launch fcu_url:="udp://:14540@127.0.0.1:14557"
```

## 3.启动ADRC节点及参数
克隆项目并将项目名改为adrc
```
git clone https://github.com/htchit/PX4_ROS_ADRC.git
```

进入工作空间
```
cd ~/catkin_ws
```
运行
```
roslaunch adrc adrc_param.launch
```

## 4.启动python脚本
```
rosrun adrc setpoint.py
```


## 5.调试技巧
### 5.1 ros主题
`rostopic list`
列出所有话题

`rostopic echo /mavros/setpoint_raw/local`
列出/mavros/setpoint_raw/local话题的所有数据

`rostopic info /mavros/serpoint_raw/local`
列出/mavros/setpoint_raw/local话题的发布方和订阅方

`rostopic hz TOPIC`
显示主题发布频率

`rostopic pubs TOPIC`
将数据发布到主题
### 5.1 ros节点
`rosnode list`
列出所有运行中的节点

`rosnode kill NODE`
结束节点进程
### 5.1 ros服务
`rosservice list`
列出所有的服务

`rosservice call [service] [args]`
调用某个服务
### 5.1 ros参数
`rosparam list`
列出所有参数名

`rosparam set [parameter] [value]`
设置参数值

`rosparam get [parameter]`
获取参数值

`rosparam load FILE`
从文件加载参数


