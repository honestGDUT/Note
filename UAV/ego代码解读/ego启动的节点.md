启动launch文件后打印的信息来源
    planner_manager.cpp     initPlanModules ---- grid_map_->initMap
    mockamap.cpp            map.generate  perlin3D
    pcl_render_node.cpp     rcvGlobalPointCloudCallBack
                            rcvLocalPointCloudCallBack
                            rcvOdometryCallbck
    traj_server.cpp         

    然后是状态机的信息
    ego_replan_fsm.cpp      changeFSMExecState
                            printFSMExecState
1. mockamap
启动launch文件后，一些信息就是这个节点打印的。
2. ego_planner_node

3. odom_visualization

4. pcl_render_node

5. quadrotor_simulator_so3

6. so3_control

7. traj_server

8. waypoint_generator