# Git学习笔记 #
## 安装Git ##
### Mac 安装 ###
从Xcode  -》 Preferences -》Downloads，选择Command Line Tools，点击【install】就可以了（Xcode默认集成了Git）
### Linux 安装 ###
```
sudo apt-get install git
```
### windows安装 ###
从https://git-for-windows.github.io下载msysgit，安装完成后，在开始菜单里找到 Git -》 Git Bash

### 用户名和邮箱设置 ###
git安装完成后，执行以下操作：
```
git config --global user.name "TalliuJobs"
git config --global user.email "talliujobs@gmail.com"
```
--global参数，表示你这台机器上所有的Git仓库都会使用这个配置。当然也可以对某个仓库指定不同的用户名和Email地址。


## 创建版本库 ##
```
 cd ~/Documents/notes/GitLearning
 #把当前目录变成Git可以管理的仓库
 git init
 #文件添加到仓库
 git add GitLearning.md
 # -m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的
 git commit -m "创建版本库，OK"

 ```
