# Gammping的使用
需求列表  
- [ ] 雷达坐标系(laser)---->base_link  
- [ ] base_link---->odom  
- [ ] /scan（话题）  

1. 建图的仿真环境
启动gazebo环境、加载机器人模型和激光雷达
```
roslaunch wpr_simulation wpb_stage_simulation.launch
```

查看雷达的坐标系，添加--noarry参数不显示话题中的数组
```
rostopic echo /scan --noarry
```
查看tf树
```
rosrun rqt_tf_tree rqt_tf_tree
```

2. 运行gampping
```
rosrun gampping slam_gampping
```

3. 启动rviz
   配置好rviz，机器人模型、雷达数据、地图
4. 控制小车运动
```
rosrun rqt_robot_steering rqt_robot_steering 
```
或者
```
rosrun wpr_simulation keyboard_vel_ctrl
```

5. 保存和加载地图
控制小车建完图后，运行指令，生成地图文件
```
rosrun  map_server map_saver -f map
```


