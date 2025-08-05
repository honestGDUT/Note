1. 启动无人机和无人车仿真
```bash
roslaunch px4 outdoor3_ugv.launch
```

2. 启动通信脚本、位姿真值脚本
等待仿真完全运行后
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