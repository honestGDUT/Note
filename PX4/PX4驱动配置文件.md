https://blog.csdn.net/weixin_45031928/article/details/138603594


rc.board_sensors文件作用是启动连接到板上的传感器。

-A 是加速度计，-G 是陀螺仪。

-s 是 Internal SPI 启动，-S 是 External SPI 启动。

-R 后面是旋转角度。如果之前的芯片的X轴正方向不是沿着飞控的正前方向。这里就不能使用默认参数0了，这里的参数既不是角度制的角度也不是弧度制的角度，这里的数字需要根据旋转的角度进行选择。

```
uint8 orientation		# Direction the sensor faces from MAV_SENSOR_ORIENTATION enum

uint8 ROTATION_YAW_0		= 0 # MAV_SENSOR_ROTATION_NONE
uint8 ROTATION_YAW_45		= 1 # MAV_SENSOR_ROTATION_YAW_45
uint8 ROTATION_YAW_90		= 2 # MAV_SENSOR_ROTATION_YAW_90
uint8 ROTATION_YAW_135		= 3 # MAV_SENSOR_ROTATION_YAW_135
uint8 ROTATION_YAW_180		= 4 # MAV_SENSOR_ROTATION_YAW_180
uint8 ROTATION_YAW_225		= 5 # MAV_SENSOR_ROTATION_YAW_225
uint8 ROTATION_YAW_270		= 6 # MAV_SENSOR_ROTATION_YAW_270
uint8 ROTATION_YAW_315		= 7 # MAV_SENSOR_ROTATION_YAW_315

```

比如如果芯片的X轴正方向指向飞控的右侧，那么就需要将坐标轴沿YAW轴旋转90度，则参数应该选择ROTATION_YAW_90，设为2。

-b 后面是SPI总线序号。
```
# 内部 SPI 总线 BMI088 加速度计、陀螺仪
bmi088 -b 2 -A -R 0 -s start
bmi088 -b 2 -G -R 0 -s start
```
