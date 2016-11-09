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

 ## 时光穿梭机 ##
```
#查看仓库当前的状态
git status
#查看具体修改的内容，按q退出
git diff
git add GitLearning.md
```

### 版本回退 ###
```
#查看文件修改的历史提交记录
git log
#查看简洁版的历史提交记录
git log --pretty=oneline
#版本回退。上一个版本就是HEAD^，上上一个版本就是HEAD^^，往上100个版本写成HEAD~100
git reset --hard HEAD^
#使用commit id来回退
git reset --hard 3628164
#查看每一次操作的git命令
git reflog
```
### 工作区和暂存区 ###
git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支

### 管理修改 ###
每次修改，如果不add到暂存区，那就不会加入到commit中
git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别
