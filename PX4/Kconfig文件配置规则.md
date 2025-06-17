#PX4 Kconfig 符号命名规则
1. 按照惯例，modules和drivers 的符号根据模块文件夹路径进行命名。例如，ADC驱动程序的符号位于 `src/drivers/adc/board_adc` 必须命名为` DRIVERS_ADC_BOARD_ADC `

2. 要为driver和module模块添加特定的选项，命名惯例是模块名+选项名。例如，`UAVCAN_V1_GNSS_PUBLISHER`, 其中GNSS_PUBLISHER是选项名，UAVCAN_V1是模块名。![alt text](/image/im_kconfig.png)

3. 这些选项必须要使用 `if` 声明来确保，只有当这个模块被使能后，选项才会显示出来
   eg.
   ```
    menuconfig DRIVERS_UAVCAN_V1
        bool "UAVCANv1"
        default n
        ---help---
            Enable support for UAVCANv1
    if DRIVERS_UAVCAN_V1
        config UAVCAN_V1_GNSS_PUBLISHER
            bool "GNSS Publisher"
            default n
    endif
   ```

## PX4 Kconfig 标签继承
每一个PX4主板都必须有一个 default.px4board 配置文件，可以有一个 bootloader.px4board 配置文件。然而，你也可以在一个不同标签文件下添加单独的配置，eg. cyphal.px4board . 默认下，cyphal.px4board继承default.px4board的所有设置。当修改cyphal.px4board文件，它只会储存与default.px4board不同的Kconfig keys.这用于简化配置管理。

## PX4 Menuconfig 启动
menuconfig 工具被用于修改px4主板配置，添加/移除模块、驱动以及其他功能。

有命令行和图形界面，启动命令分别为
```
make px4_sitl_default boardconfig
make px4_sitl_default boardguiconfig
```
这里我用的是仿真环境，px4_sitl_default可替换为你使用的主板