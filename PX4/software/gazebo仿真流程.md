# 仿真

## model.config的作用
当通过 launch 文件启动时：  

```xml
<include file="$(find px4)/launch/posix_sitl.launch">
    <arg name="vehicle" value="x500"/> <!-- 指向models/x500目录 -->
</include>
```
工作流程：
1. PX4 解析 **vehicle=x500** 参数  
2. 定位到 **models/x500** 目录  
3. 读取 **model.config** 获取：  
    主 SDF 文件路径 (model.sdf)  
    必需插件列表  
    依赖关系  
4. 将配置传递给 Gazebo  
5. Gazebo 根据配置加载模型和插件


## 模型继承机制
