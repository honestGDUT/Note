# 编译烧录源代码错误
编译代码
```bash
make px4_fmu-v3_default
make px4_fmu-v5_default
make px4_fmu-v6c
```

1. 在build文件夹下找到对应的.px4固件
2. 提前打开QGC地面站，先不要接入飞控。在地面站上打开固件下载界面，再接入飞控，此时地面站才会弹出下载固件选项。
3. 点击高级设置，选择自定义固件文件夹，选择编译好的.px4固件
   

## 问题一：固件id不匹配
我使用的飞控板为micoair743，固件id为1166，而源代码中的/boards/px4文件夹下无1166的id。在我手动修改/boards/px4/fmu-v...对应文件夹下firmware.prototype文件中的id，然后再次编译，就不会报错了。

## 问题二：镜像文件过大，无法装入飞控板

在解决第一个问题后，烧录固件时报这个错误。

修改default.px4board文件后，解决了Flash溢出的问题，但是出现了下载到飞控板后，飞控无响应，无法进入地面站配置页面。推测是飞控板的传感器型号与default.px4board文件中配置的不符导致的。
![alt text](</image/im_mapsize.png>)

查看micoair.cn官网后，发现px4 在v1.16.0以后支持micoair_743，故在v1.16.0的px4源码中运行
```bash
make micoair_h743
```
发现flash大小符合要求，然后在build/micoair_743_default中找到.px4固件，用QGC地面站烧录好固件，固件id正常！！！问题一解决。能正常进入地面站！！！问题二解决。

