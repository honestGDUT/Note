# Fast-Perching 匀加速目标模型修改报告

## 1. 核心轨迹优化器 (traj_opt)

这部分是改动的核心，主要目的是让优化求解器（Optimizer）能够理解目标平台具有加速度，并正确计算目标函数（cost）和梯度（gradient）。

### 1.1 接口更新

**文件**: `traj_opt.h` & `traj_opt_perching.cc`

**改动**: `generate_traj` 函数新增输入参数 `const Eigen::Vector3d& car_a`，用于传入目标加速度。

### 1.2 目标函数与梯度计算 (objectiveFunc)

**运动学模型更新**: 
- 将目标位置预测公式从 $p_t = p_0 + v_0 T$ 更新为匀加速公式：$p_t = p_0 + v_0 T + \frac{1}{2} a_0 T^2$
- 将目标速度预测公式从 $v_t = v_0$ 更新为：$v_t = v_0 + a_0 T$

**末端状态约束**:
- 更新了优化问题中末端位置 `tailS.col(0)` 和末端相对速度 `land_v_` 的计算，加入了加速度项。

**对时间 $T$ 的梯度修正**:
这是解决求解器报错（ret: -1004）的关键。由于末端状态现在是时间 $T$ 的二次函数（位置）和一次函数（速度），对 $T$ 求导时必须包含加速度带来的分量。
```c++
// 位置部分对T的导数: v + a*T
obj.mincoOpt_.gdT += obj.mincoOpt_.gdTail.col(0).dot(obj.N_ * (car_v_ + car_a_ * T)); 
// 速度部分对T的导数: a
obj.mincoOpt_.gdT += obj.mincoOpt_.gdTail.col(1).dot(obj.N_ * car_a_);
```
**碰撞检测梯度**:
- 在计算障碍物（平台）碰撞代价时，障碍物速度同样修正为 $V_{car} + A_{car} \times \Delta t$。

### 1.3 初值猜测生成 (Initial Guess)

**修正 BVP 边界条件**:
这是解决"规划出的轨迹很奇怪"的关键。BVP（边值问题）求解器用于生成优化的初值。之前的代码在使用 BVP 生成初值时未考虑加速度项和正确的末端偏移量（`tail_q_v_ * robot_l_`），导致初值轨迹严重偏离可行解。修正后:
```c++
// 修正了位置：加入加速度项 + 机械臂偏移项
bvp_f.col(0) = car_p_ + car_v_ * T_bvp + 0.5 * car_a_ * T_bvp * T_bvp + tail_q_v_ * robot_l_;
// 修正了速度：加入加速度项 - 相对速度项
bvp_f.col(1) = (car_v_ + car_a_ * T_bvp) - tail_q_v_ * v_plus_;
```
## 2. 规划节点 (planning)

这部分修改主要用于支持新参数的输入、仿真环境的更新以及动态调参功能。

### 2.1 仿真逻辑更新

**文件**: `planning_nodelet.cpp`

**改动**: 在 `debug_timer_callback` 仿真循环中，更新了模拟目标（fake target）的状态更新方程，使其按照匀加速运动，以便在 Rviz 中正确模拟和测试。
```c++
target_p = target_p + target_v * dt + 0.5 * target_a * dt * dt;
target_v = target_v + target_a * dt;
```
### 2.2 动态调参 (Dynamic Reconfigure)

**新增功能**: 为了方便调试加速度参数和优化权重，集成了 `dynamic_reconfigure`。

**文件**:
- `Planning.cfg`: 定义了包括 `perching_ax/y/z` 在内的所有可调参数。
- `planning_nodelet.cpp`: 添加了回调函数，实时更新参数到规划器中。
- `CMakeLists.txt` & `package.xml`: 添加了相关编译依赖。

## 3. 启动配置

**文件**: `perching.launch`

**改动**: 新增了 `perching_ax`, `perching_ay`, `perching_az` 参数接口，允许在启动时设置目标加速度。