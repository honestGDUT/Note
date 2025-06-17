# 首先确定自己的python版本，以及相关依赖是否安装正确

thefuck 依赖于python3, python3-dev, pip3 , python3-setuptools

## 正式安装
```bash
sudo pip3 install thefuck
```

## 修改别名
```bash
sudo vim ~/.bashrc
```
在最后一行添加一个 fuck
```bash
eval "$(thefuck --alias fuck)"
```
按ESC键 :wq 退出文件
## 使配置文件生效
```bash
source ~/.bashrc
```




