# EGO-Planner代码解读
1. 程序主入口main
ego_planner_node.cpp中，main函数中初始化了ego_planner_node节点。并使用类EGOReplanFSM.init(nh)进入程序主循环。

2. 类EGOReplanFSM
init函数 创建参数、定时器，初始化主要模块 
主要模块 
```
    visualization 
    planner_manager
```
定时器
```
    exec_timer_  execFSMCallback
    safety_timer_  checkCollisionCallback
```
订阅话题：回调函数
```
    odom_sub_    odometryCallback
```
发布话题
```
    bspline_pub_    /planning/bspline
    data_disp_pub_  /planning/data_display
```
判断目标类型
```
if MANUAL_TARGET    //如果在launch文件中flight_type=1,则使用2D Nav Goal to select goal (手动模式)
    订阅/waypoint_generator/waypoints
    回调函数 waypointCallback
else if PRESET_TARGET   //如果在launch文件中flight_type=2，则使用预设目标点，飞一个正方形
    planGlobalTrajbyGivenWps函数
else
    error
```

2. planGlobalTrajbyGivenWps函数
planner_managerplanner_manager_->planGlobalTrajWaypoints  //处理多个目标点，生成轨迹

visualization_->displayGoalPoint

状态机
    changeFSMExecState

3. waypointCallback回调函数

planner_manager_->planGlobalTraj  //处理单个目标点，生成轨迹
    