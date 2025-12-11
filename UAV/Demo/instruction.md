1. 启动无人机和无人车仿真
把launch文件中的参数修改，再删去多余的无人机
```xml
<arg name="fcu_url" default="udp://:14540@127.0.0.1:14557"/>
```
然后启动
```bash
roslaunch px4 outdoor2_precision_landing.launch
```

1. 启动通信脚本、位姿真值脚本
修改通信脚本中的无人机数量，等待仿真完全运行后
```bash
cd ~/XTDrone/communication
bash single_vehicle_communication.sh 

cd ~/XTDrone/sensing/pose_ground_truth
bash single_get_local_pose.sh 
```

3. 启动自定义飞行
```bash 
roslaunch agcplanner run_in_sim.launch
```