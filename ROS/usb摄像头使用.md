# Ubuntu ROS系统轻松接入USB摄像头

1. 安装ros-drivers功能包，该功能包包含了用于驱动USB摄像头的软件：
```bash
sudo apt-get install ros-melodic-usb-cam
```
3. 安装opencv库，该库是图像处理的基础库：
```bash
sudo apt-get install libopencv-dev
```

## 接入USB摄像头
使用以下命令来启动USB摄像头：
```bash 
roslaunch usb_cam usb_cam-test.launch
```

# 使用rqtimageview查看图像