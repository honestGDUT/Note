# PX4环境配置 -- Ubuntu22.04

1. 下载PX4源码，需要科学上网<br>
   ```bash
   git clone https://github.com/PX4/PX4-Autopilot.git
   ```
2. 进入PX4-Autopilot文件夹，切换需要的源码分支，这里选择的是v1.13.1<br>
    ```bash
    cd PX4-Autopilot
    git tag  -l                   查看有哪些版本
    git checkout v1.13.1          切换指定版本
    git describe --always --tags  查看当前版本
    ```
3. 更新仓库地址、下载子模块<br>
    ```bash
    git submodule sync --recursive
    git submodule update --init --recursive
    ```
4. 配置环境<br>
    ```bash
    bash ./Tools/setup/ubuntu.sh
    ``` 
    