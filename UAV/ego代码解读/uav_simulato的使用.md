# UAV Simulator 节点列表

根据 simulator.xml 启动文件，uav_simulator 包含以下6个核心节点：

## 核心节点列表

1. **random_forest**
   - **包**: map_generator
   - **类型**: random_forest
   - **功能**: 生成随机障碍物环境，发布全局点云地图
   - **位置**: simulator.xml:13

2. **quadrotor_simulator_so3**
   - **包**: so3_quadrotor_simulator
   - **类型**: quadrotor_simulator_so3
   - **功能**: 模拟四旋翼动力学，接收SO3控制命令并发布里程计信息
   - **位置**: simulator.xml:40
   - **实现**: 在 `quadrotor_simulator_so3.cpp` 中定义 (quadrotor_simulator_so3.cpp:199-202)

3. **so3_control**
   - **包**: so3_control
   - **类型**: SO3ControlNodelet (nodelet)
   - **功能**: 实现SO3几何控制器，将位置命令转换为力和姿态命令
   - **位置**: simulator.xml:52
   - **实现**: 在 `so3_control_nodelet.cpp` 中定义 (so3_control_nodelet.cpp:13)

4. **so3_disturbance_generator**
   - **包**: so3_disturbance_generator
   - **类型**: so3_disturbance_generator
   - **功能**: 为里程计添加噪声，生成力和力矩扰动
   - **位置**: simulator.xml:67

5. **odom_visualization**
   - **包**: odom_visualization
   - **类型**: odom_visualization
   - **功能**: 在RViz中可视化无人机的位姿和轨迹
   - **位置**: simulator.xml:75
   - **实现**: 在 `odom_visualization.cpp` 中定义 (odom_visualization.cpp:1-30)

6. **pcl_render_node**
   - **包**: local_sensing_node
   - **类型**: pcl_render_node
   - **功能**: 模拟深度相机，通过光线投射生成深度图像
   - **位置**: simulator.xml:85
   - **实现**: 根据是否启用CUDA，使用不同的源文件 (`pcl_render_node.cpp` 或 `pointcloud_render_node.cpp`) (CMakeLists.txt:55-83)



## 使用方法
# 1. 订阅里程计信息

如果需要获取无人机的位置和速度信息,订阅 /visual_slam/odom 话题:
```cpp
ros::Subscriber odom_sub = n.subscribe("/visual_slam/odom", 10, odomCallback);  
  
void odomCallback(const nav_msgs::Odometry::ConstPtr& odom) {  
    // 获取位置  
    double x = odom->pose.pose.position.x;  
    double y = odom->pose.pose.position.y;  
    double z = odom->pose.pose.position.z;  
      
    // 获取速度  
    double vx = odom->twist.twist.linear.x;  
    double vy = odom->twist.twist.linear.y;  
    double vz = odom->twist.twist.linear.z;  
}
```
参考实现见 so3_control_nodelet.cpp,它订阅里程计并更新控制器状态:so3_control_nodelet.cpp:114-140

## 2. 发布位置指令
如果您有自己的规划算法,需要发布 quadrotor_msgs::PositionCommand 到 /planning/pos_cmd:
```cpp
ros::Publisher pos_cmd_pub = n.advertise<quadrotor_msgs::PositionCommand>("/planning/pos_cmd", 10);  
  
quadrotor_msgs::PositionCommand cmd;  
cmd.position.x = target_x;  
cmd.position.y = target_y;  
cmd.position.z = target_z;  
cmd.velocity.x = target_vx;  
cmd.velocity.y = target_vy;  
cmd.velocity.z = target_vz;  
cmd.acceleration.x = target_ax;  
cmd.acceleration.y = target_ay;  
cmd.acceleration.z = target_az;  
cmd.yaw = target_yaw;  
cmd.yaw_dot = target_yaw_dot;  
  
// 设置控制增益  
cmd.kx[0] = kx; cmd.kx[1] = kx; cmd.kx[2] = kx;  
cmd.kv[0] = kv; cmd.kv[1] = kv; cmd.kv[2] = kv;  
  
pos_cmd_pub.publish(cmd);
```
SO3控制器会接收此命令并解析: so3_control_nodelet.cpp:94-111

## 3. 获取环境地图
订阅 /map_generator/global_cloud 获取全局障碍物点云：simulator.xml:95
```cpp
ros::Subscriber map_sub = n.subscribe("/map_generator/global_cloud", 1, mapCallback);  
  
void mapCallback(const sensor_msgs::PointCloud2::ConstPtr& cloud) {  
    pcl::PointCloud<pcl::PointXYZ> pcl_cloud;  
    pcl::fromROSMsg(*cloud, pcl_cloud);  
    // 使用点云进行路径规划或避障  
}
```
## 4. 获取深度图像
如果需要模拟深度相机数据,订阅 pcl_render_node 发布的深度话题:
```cpp
os::Subscriber depth_sub = n.subscribe("/pcl_render_node/depth", 1, depthCallback);  
ros::Subscriber pose_sub = n.subscribe("/pcl_render_node/camera_pose", 1, poseCallback);
```

# 启动流程

```bash
roslaunch ego_planner simulator.xml
```

模拟器的数据流为: 您的规划节点发布 PositionCommand → SO3控制器转换为 SO3Command → 四旋翼模拟器更新状态 → 发布 Odometry。   
如果需要添加扰动,可以发布到 force_disturbance 和 moment_disturbance 话题