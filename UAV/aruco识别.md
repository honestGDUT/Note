1. 先安装aruco_ros包
sudo apt-get install ros-<your_ros_distro>-aruco-ros

2. 校准摄像头
roslaunch usb_cam usb_cam-test.launch 
 
rosrun camera_calibration cameracalibrator.py --size 6x4 --square 0.39 image:=/usb_cam/image_raw


3. 编写自己的launch文件来测试
   
rosrun image_view image_view image:=/aruco_single/result