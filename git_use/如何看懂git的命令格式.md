理解 Git 命令格式（如 git init 的说明）关键在于掌握命令行参数约定的通用语法和符号含义。下面是如何解读这类格式的详细指南：
```bash
git init [-q | --quiet] [--bare] [--template=<template-directory>]
	 [--separate-git-dir <git-dir>] [--object-format=<format>]
	 [--ref-format=<format>]
	 [-b <branch-name> | --initial-branch=<branch-name>]
	 [--shared[=<permissions>]] [<directory>]
```

## 核心符号含义
1. [ ] (方括号):  
可选参数。括号内的内容可以省略，命令仍能执行。  
示例: [<directory>] 表示你可以指定目录（如 git init my_project），也可以直接运行 git init。  

2. | (竖线):  
互斥选项。只能选择其中一个参数，不能同时使用。二者之间等价或者互斥  
示例: [-q | --quiet] 表示可用 -q 或 --quiet，但不能同时用。  

3. < > (尖括号):  
占位符。表示需要用户替换的实际值（如路径、分支名等）。  
示例: <directory> 需替换为目录名（如 ./project）。  
 
4. --flag (双连字符):  
长选项。全称形式，通常更易读（如 --bare）。  

5. -f (单连字符):  
短选项。简写形式（如 -q 是 --quiet 的缩写）。  

6. = (等号):  
赋值符号。用于指定参数值，不可省略空格。  
示例: --template=<template-directory> 必须写成 --template=/path/to/template。  

git init参数详解
```bash
git init [-q | --quiet]          # 静默模式（减少输出）
          [--bare]               # 创建裸仓库（无工作目录）
          [--template=<template-directory>]  # 指定模板目录
          [--separate-git-dir <git-dir>]     # 分离.git目录到指定路径
          [--object-format=<format>]         # 指定对象存储格式（如sha1/sha256）
          [--ref-format=<format>]           # 指定引用存储格式
          [-b <branch-name> | --initial-branch=<branch-name>]  # 设置初始分支名
          [--shared[=<permissions>]]        # 设置仓库共享权限（如group）
          [<directory>]                     # 指定仓库创建路径
```          

关于具体每个参数的含义，需要用时，建议直接到官网文档上查找。各个博客写的水平参差不齐，且不全面。  

需要注意的是有些参数可能是git新版本才支持的，这时候要么直接git help <指令名>查看当前版本是否支持这些参数。
或者直接更新git
```bash
git --version  
sudo add-apt-repository ppa:git-core/ppa  #必需步骤，否则无法安装最新版本
sudo apt update
sudo apt install git
```