# ubuntu20.04死机
具体表现为开机后能进入桌面，但点击右上角想打开系统设置，或着点击浏览器都会死机，键盘和鼠标都无法输入，无法打开终端，只能长按电源关机。

## 解决方法:(目前还有效)
1. 针对上述情况，能进入桌面。
2. 进入桌面后不要点击进入任何图形界面，使用ctrl + Alt + T打开 终端，输入下列代码，进入tty命令行模式。
     ```bash
     sudo systemctl set-default multi-user.target
     sudo reboot now
     ```
3. 开机后,电脑显示，这是不正常的，正常的应该会打印出欢迎进入ubuntu的信息！
![alt text](/image/im_guidriver.jpg)

4. 下面将禁用自带的nouveau驱动
   若电脑安装了vim，就输入
```bash
sudo vim /etc/modprobe.d/black.list
```
加入以下内容（将自带驱动加入黑名单）
```bash
blacklist vga16fb
blacklist nouveau
blacklist rivafb
blacklist rivatv
blacklist nvidiafb
```
执行(这一步我好像没有执行？？)
```bash
sudo update-initramfs -u
```

5. 重启 
   ```
   sudo reboot now
   ```


