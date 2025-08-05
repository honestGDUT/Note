# launch文件的介绍及应用

## 1.1 launch文件作用
 launch 文件，可以一次性启动多个 ROS 节点，不用先输入roscore,并且可以设置丰富的参数。launch文件还可以嵌套使用。

## 基本结构
\<launch>和\</launch>配合使用
```xml
<launch>
<node pkg="package_name" type="executable_node" name="node_name" output="screen"/>
</launch>
```
type为可执行文件的名字  
name是节点运行后所命名的名字，取代了executable_node中节点的名字，这样子可以同时运行两个同样的节点。如下所示：  
```xml
    <node pkg="turtlesim" name="sim1" type="turtlesim_node"/>
    <node pkg="turtlesim" name="sim2" type="turtlesim_node"/>
```
这样以上两句可以同时运行turtle_node，产生的节点的名字会由name所赋予，不会产生节点重复的冲突，可以提高代码的复用性。

## 常用标签
1. \<node>  
node标签会指定一个准备运行的ROS节点，node标签是 launch 文件中最重要的标签，因为它实现了launch文件的基本功能，即同时启动多个ROS节点。  
```xml
<node pkg="package_name" type="executable_node" name="node_name" args="$()" respawn="true" output="sceen">
    ```
pkg：节点所在功能包的名称package_name；  
type：节点类型是可执行文件(节点)的名称executable_node；  
name：节点运行时的名称node_name；  
args：传递命令行设置的参数；  
respawn：是否自动重启，true表示如果节点未启动或异常关闭，则自动重启；false则表示不自动重启，默认值为false；  
output：是否将节点信息输出到屏幕，如果不设置该属性，则节点信息会被写入到日志文件，并不会显示到屏幕上。  


2. \<param>  
在工程项目开发中，我们常常需要改变程序变量的一些参数，如果在程序中赋值，我们每次修改参数都需要重新编译程序，大大降低了开发效率，而param标签则可以实现传递参数的功能，它可以定义一个将要被设置到参数服务器的参数，它的参数值可以通过文本文件、二进制文件或命令等属性来设置。 
    ```xml
    <param name="param_name" type="param_type" value="param_value" />
    <!-- param 标签可以嵌入到 node 标签中，以此来作为该 node 的私有参数 -->
    <node>
        <param name="param_name" type="param_type" value="param_value" />
    </node>
    ```
    name：参数名称param_name  
    type：参数类型double，str，int，bool，yaml  
    value：需要设置的参数值param_value  
3. \<rosparam>  
  rosparam标签可以实现节点从参数服务器上加载(load)、导出(dump)和删除(delete)YAML文件  
```xml
<!-- 加载package_name功能包下的example.yaml文件 -->
<rosparam command="load" file="$(find package_name)/example.yaml">
<!-- 导出example_out.yaml文件到package_name功能包下 -->
<rosparam command="dump" file="$(find package_name)/example_out.yaml" />
<!-- 删除参数 -->
<rosparam command="delete" param="xxx/param">
```
command：功能类型(load、dump、delete)  
file：参数文件的路径  
param：参数名称  

4. \<include>  
include标签功能和编程语言中的include预处理类似，它可以导入其他launch文件到当前include标签所在的位置，实现launch文件复用。  
    ```xml
    <include file="$(find pr2_controller_manager)/controller_manager.launch"/>
    ```
    如果需要传递参数则需要\<include> 和 \</include>配置使用，然后在这两个标签中定义\<arg>标签。示例如下：
    ```xml
        <include file="$(find gazebo_ros)/launch/empty_world.launch">
            <arg name="gui" value="$(arg gui)"/>
            <arg name="world_name" value="$(arg world)"/>
            <arg name="debug" value="$(arg debug)"/>
            <arg name="verbose" value="$(arg verbose)"/>
            <arg name="paused" value="$(arg paused)"/>
            <arg name="respawn_gazebo" value="$(arg respawn_gazebo)"/>
        </include>
    ```
    这些参数是显式的，会替代empty_world.launch中的参数。

5. \<remap>  
remap标签可以实现节点名称的重映射，每个remap标签包含一个原始名称和一个新名称，在系统运行后原始名称会被替换为新名称。  
    ```xml
    <remap from="turtle1/cmd_vel" to="/cmd_vel" />
    <!-- remap 标签同样可以嵌入到 node 标签中，以此来作为该 node 的私有重映射 -->
    <node>
        <remap from="turtle1/cmd_vel" to="/cmd_vel" />
    </node>
    ```
6. \<arg>
arg标签表示启动参数，该标签允许创建更多可重用和可配置的启动文件，其可以通过命令行、include 标签、定义在高级别的文件这 3 种方式配置值。同时注意：arg标签声明的参数不是全局的，只能在声明的单个启动文件中使用，可以当成函数的局部参数来理解。  
    ```xml
    <arg name="arg_name" default="arg_default" />
    <arg name="arg_name" value="arg_value" />
    <!-- 命令行传递的 arg 参数可以覆盖 default，但不能覆盖 value。 -->
    ```
    注意：arg和param标签的区别：  
    
    | 函数  |                  作用                  |
    |:-------:|:----------------------------------------:|
    | arg   | 启动时的参数，只在launch文件中有意义   |
    | param | 运行时的参数，参数会存储在参数服务器中 |

7. \<group>  
group标签可以实现将一组配置应用到组内的所有节点，它也具有命名空间ns特点，可以将不同的节点放入不同的 namespace。  
```xml
<!-- 用法1 -->
<group ns="namespace_1">
    <node pkg="pkg_name1" .../>
    <node pkg="pkg_name2" .../>
    ...
</group>

<group ns="namespace_2">
    <node pkg="pkg_name3" .../>
    <node pkg="pkg_name4" .../>
    ...
</group>
<!-- 用法2 -->
<!-- if = value：value 为 true 则包含内部信息 -->
<group if="$(arg foo1)">
    <node pkg="pkg_name1" .../>
</group>

<!-- unless = value：value 为 false 则包含内部信息 -->
<group unless="$(arg foo2)">
    <node pkg="pkg_name2" .../>
</group>
<!--
	当 foo1 == true 时包含其标签内部
	当 foo2 == false 时包含其标签内部
-->
```
