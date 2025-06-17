# PX4开发

编译命令 make px4_fmu-v2_default
含义:配置选项在 boards/px4/fmu-v2/default.px4board中
make micoair_h743_default
含义:配置选项在 boards/micoair/h743/default.px4board中
make px4_sitl_default
含义:配置选项在 boards/px4/sitl/default.px4board中

如果没有指明default，则会默认编译default

## 1.创建功能包源文件
1.在 /src/examples下创建一个my_example_app 并在该目录下新建一个.c格式的源文件。
2.编写代码
```
#include <px4_platform_common/log.h>


__EXPORT int my_example_app_main(int argc, char *argv[]);

int my_example_app_main(int argc, char *argv[])
{
	PX4_INFO("Hello World!");
	return OK;
}

```
3.创建Kconfig文件,在/src/examples下创建Kconfig文件，编写代码
```
menuconfig 
```
