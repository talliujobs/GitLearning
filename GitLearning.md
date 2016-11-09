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
**小结**  
* HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。  
* 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。  
* 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

### 工作区和暂存区 ###
git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支

### 管理修改 ###
每次修改，如果不add到暂存区，那就不会加入到commit中
git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别

**小结**  
每次修改，如果不add到暂存区，那就不会加入到commit中

### 撤销修改 ###
git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
* 一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
* 一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次git commit或git add时的状态。


git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区  
git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本

**小结**  
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。  
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。  
场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

### 删除文件 ###
一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了，Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了。  
现在有两种情况：
* 确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit
* 删错了。可以使用 ``` git checkout -- test.txt ``` 很轻松地把误删的文件恢复到最新版本

git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

**小结**  
git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

## 远程仓库 ##
### 配置ssh key ###
第1步：创建SSH Key。
```
#如果已生成此邮箱的ssh key，可忽略此操纵，直接进行第2步
ssh-keygen -t rsa -C "talliujobs@gmail.com"
```
执行此命令，然后一路回车，使用默认值即可（由于这个Key也不是用于军事目的，所以也无需设置密码），此操作将会在~/.ssh目录下生成id_rsa和id_rsa.pub两个文件
如果有多个用户，需生成不同的ssh key ，可在上述命令后增加 -f <path>

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面。点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容，确定即可

### 添加远程库 ###
在GitHub中，新建一个库，名字为GitLearning。  
在本地的仓库下运行命令
```
git remote add origin git@github.com:talliujobs/GitLearning.git
git push -u origin master
```
第一条命令： 远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。  
第二条命令： git push命令，实际上是把当前分支master推送到远程。由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。  

**小结**  
要关联一个远程库，使用命令``` git remote add origin git@server-name:path/repo-name.git ```  
关联后，使用命令``` git push -u origin master ``` 第一次推送master分支的所有内容   
此后，每次本地提交后，只要有必要，就可以使用命令 ``` git push origin master ```  推送最新修改

### 从远程库克隆 ###
```
git clone git@github.com:michaelliao/gitskills.git
```
上述命令会将文件克隆至当前路径下，文件夹名为仓库名

## 分支管理 ##

### ###
```
#创建dev分支，然后切换到dev分支，实际上相当于两条命令 git branch dev 和 git checkout dev
git checkout -b dev
#查看当前分支
git branch
#切换回master分支
git checkout master
 ```
