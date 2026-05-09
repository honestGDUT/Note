# Note

个人技术笔记仓库，用于在多台电脑间同步和共享学习记录。

## 目录结构

```
.
├── C++/                    # C++ 编程笔记
│   ├── Eigen库/            # Eigen 矩阵库使用
│   └── image/              # 相关图片
├── Linux/                  # Linux 系统使用笔记
│   └── image/
├── Markdown/               # Markdown 语法笔记
│   └── image/
├── PX4/                    # PX4 飞控笔记
│   ├── 代码解读/           # PX4 源码解读
│   ├── 二次开发/           # PX4 二次开发指南
│   ├── hardware/           # 硬件相关配置
│   ├── software/           # 软件使用与配置
│   └── image/
├── ROS/                    # ROS 机器人操作系统笔记
│   └── image/
├── UAV/                    # 无人机相关笔记
│   ├── algorithm/          # 算法（B样条曲线）
│   ├── daily_record/       # 实验记录
│   ├── Demo/               # 演示方案
│   ├── ego代码解读/        # EGO-Planner 代码解读
│   └── image/
├── droneHardware/          # 无人机硬件选型
│   └── image/
├── gazebo/                 # Gazebo 仿真笔记
│   └── image/
├── git_use/                # Git 使用笔记
│   └── image/
├── vim_use/                # Vim 使用笔记
├── vscode_use/             # VSCode 使用笔记
└── 遥控器/                 # 遥控器相关（固件刷写等）
```

## 内容概览

### C++
- 继承语法与虚函数使用
- Eigen 库配置与矩阵块操作
- 卡尔曼滤波编程
- 自动互斥锁、类变量初始化

### Linux
- 系统监控（CPU/内存/磁盘/GPU）
- 终端代理、截图工具、桌面图标
- 常用命令（备份、改权限、卸载、pip 等）
- Terminator 终端使用

### PX4
- **代码解读**：编译选项、板级配置、飞行模式切换
- **二次开发**：系统架构、编写驱动/应用程序、uORB 主题、自定义工作队列、控制器图解
- **software**：环境配置、Gazebo 仿真、EKF 模块、Offboard 模式、Kconfig 配置
- **hardware**：电机顺序配置、固件问题排查

### ROS
- 功能包创建、CMakeLists/package.xml 配置
- launch 文件使用、USB 摄像头
- SLAM 建图与保存、导航架构
- XTDrone 仿真（键盘控制、多机降落）

### UAV
- 无人机组装与配件、气压计定高
- A* 路径规划算法、B 样条曲线
- Fast-perching 算法及分支使用
- EGO-Planner 代码解读与仿真
- PX4Ctrl 使用流程、Aruco 识别

### Gazebo
- 自定义四旋翼模型
- SW 模型转 SDF 格式

### 其他
- **Markdown**：标题、表格、代码块、链接、任务列表等语法
- **Git**：工作流、分支管理、标签、代理配置、.gitignore
- **Vim/VSCode**：快捷键与常用命令
- **遥控器**：FS-i6 刷 14 通道固件

## 使用教程

### 1. 安装 VSCode

推荐使用 [Visual Studio Code](https://code.visualstudio.com/) 编辑和预览笔记。

### 2. 安装插件

- Markdown All in One
- Markdown Preview Enhanced
- markmap

### 3. 修改预览样式

通过 Markdown Preview Enhanced 自定义 CSS 样式：

```
Ctrl+Shift+P → Markdown Preview Enhanced: Customize Css
```

打开 `style.css` 文件，参考样式如下：

```css
#nice {
    line-height: 1.8;
    color: #2b2b2b;
    font-family: Optima-Regular, Optima, PingFangTC-Light, PingFangSC-light, PingFangTC-light;
    letter-spacing: 2px;
    background-image: linear-gradient(90deg, rgba(50, 0, 0, 0.04) 3%, rgba(0, 0, 0, 0) 3%), linear-gradient(360deg, rgba(50, 0, 0, 0.04) 3%, rgba(0, 0, 0, 0) 3%);
    background-size: 20px 20px;
    background-position: center center;
}

p {
    font-size: 15px !important;
}

h1 {
    display: table;
    margin: 30px auto 20px auto !important;
    padding: 10px !important;
    background-image: linear-gradient(to left, rgb(253, 213, 231), rgb(194, 226, 249));
    border-width: 1px;
    border-radius: 10px;
    box-shadow: 3px 3px 3px #ccc;
    font-size: 20px !important;
    text-align: center;
}

h2 {
    display: table;
    margin: 30px auto 20px auto !important;
    padding: 0px 10px !important;
    border-bottom: 5px solid #205792;
    text-align: center;
    font-size: 18px !important;
}

h3 {
    border-bottom: #2b2b2b;
}

h3:before {
    content: "✒️ ";
}

h4:before {
    content: "✏️";
}

h5 {
    background-color: #f1f1f1;
    border-left: 5px solid #fff;
    padding-left: 5px !important;
    box-shadow: -3px 0px #205792;
}

h6 {
    border-left: 5px solid rgba(0, 0, 0, 0);
    box-shadow: 0px 2px #205792;
}

img {
    width: 95%;
    margin: 5px auto 8px auto !important;
    border-radius: 5px;
    box-shadow: 3px 2px 3px #ccc;
}

strong {
    color: #ff4c00 !important;
}

em {
    font-weight: 800;
    font-style: normal !important;
}
```
