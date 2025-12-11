## Git笔记：查看和修改远程仓库URL地址
在项目地址下面输入（任选其一）：
```bash
git remote get-url origin

git remote -v

git remote show origin 

git config remote.origin.url
```

修改远程仓库地址  
```bash
# 查看远端地址
git remote -v  
# 查看远端仓库名
git remote 
# 重新设置远程仓库
git remote set-url origin https://github.com/xx/xx.git (新地址)
```
或修改 .git 隐藏文件夹下的 config 文件内容，将 [remote "origin"] url 修改成你需要替换的新地址：