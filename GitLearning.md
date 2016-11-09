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
如果有多个用户，需生成不同的ssh key ，可在上述命令后增加 -f "path"

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

### 创建与合并分支 ###
```
#创建dev分支，然后切换到dev分支，实际上相当于两条命令 git branch dev 和 git checkout dev
git checkout -b dev
#查看当前分支
git branch
#切换回master分支
git checkout master
#合并指定分支到当前分支
git merge dev
#删除dev分支
git branch -d dev
```
**小结**  
查看分支：git branch  
创建分支：git branch "name"  
切换分支：git checkout "name"  
创建+切换分支：git checkout -b "name"  
合并某分支到当前分支：git merge "name"  
删除分支：git branch -d "name"  

### 解决冲突 ###
执行 ``` git merge "name" ```命令后，如果有冲突，Git会提示我们，必须手动解决冲突后再提交。  
打开文件，会发现Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容。修改后保存，再次 add、commit即可

``` git log --graph --pretty=oneline --abbrev-commit ```可查看分支合并的情况，如下：  
<pre>
*   f5fb24e 分支冲突测试4，解决冲突
|\
| * 122289e 分支冲突测试2，添加了一些内容
* | 6bf857d 分支冲突测试3，添加了一些内容
|/
* 973d37f 分支冲突测试1，无实际意义
* c376dc9 创建与合并分支
* 3cc98b0 切换分支测试，无实际意义
* dbb7460 分支管理
* d9033f2 从远程库克隆
* b768add 添加远程库
* c0bf9ac 远程仓库，配置ssh key
* 7e5b114 删除文件
* 45ceff1 撤销修改
* 1f837c0 管理修改
* 0eb2185 工作区和暂存区
* 82dcefa 版本回退
* a137032 此次提交是为了测试版本回退功能，文件内容没有实际意义
* 6ea306c 时光穿梭机，开头部分
* 01ce643 创建版本库，OK
</pre>

### 分支管理策略 ###
在实际开发中，我们应该按照几个基本原则进行分支管理：  
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；  
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；  
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。  

**小结**  
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

### Bug分支 ###


```
#把当前工作现场“储藏”起来，等以后恢复现场后继续工作
git stash
#在master分支上修改bug
git checkout master
git checkout -b issue-101
#修改完成后
git add "file"
git commit -m "fix bug 101"
#切换到master分支，并完成合并，最后删除issue-101分支
git checkout master
git merge --no-ff -m "merged bug fix 101" issue-101
git branch -d issue-101

#返回dev分支接着干活
git checkout dev
#查看stash内容列表
git stash list

#使用 git stash apply 或者 git stash pop 来恢复stash内容
#apply，恢复后，stash内容并不删除，需要用git stash drop来删除
git stash apply
#pop，恢复的同时把stash内容也删了
git stash pop
#恢复指定的stash
git stash apply stash@{0}
```

### Feature分支 ###
新建Feature分支见前文所述。如果Feature分支需要被取消，使用 ``` git branch -D "name" ``` 强行删除分支

### 多人协作 ###
多人协作的工作模式通常是这样：
1. 首先，可以试图用git push origin branch-name推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。

**小结**  
* 查看远程库信息，使用git remote -v；
* 本地新建的分支如果不推送到远程，对其他人就是不可见的；
* 从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
* 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
* 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
* 从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。


## 标签管理 ##
tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。比如 按照tag v1.2查找对应的commit

### 创建标签 ###
git tag "name" [id] 用于新建一个标签，默认为HEAD，也可以指定一个commit id；  
git tag -a "tagname" -m "blablabla..." [id] -a指定标签名，-m指定说明文字；  
git tag -s "tagname" -m "blablabla..." [id] 可以用PGP签名标签；  
git tag 可以查看所有标签  

### 操作标签 ###
git push origin "tagname" 可以推送一个本地标签；  
git push origin --tags 可以推送全部未推送过的本地标签；  
git tag -d "tagname" 可以删除一个本地标签；  
git push origin :refs/tags/"tagname" 可以删除一个远程标签。  


## 自定义Git ##
### 忽略特殊文件 ###
忽略文件的原则是：
1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

创建.gitignore文件，示例如下：
```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build

# My configurations:
db.ini
deploy_key_rsa

# for test
*.class
```
```
#把.gitignore提交到版本库
git add .gitignore
git commit -m "***"
```

```
#查看文件 test.class 被哪个规则忽略了
git check-ignore -v test.class
#强制提交被忽略的文件
git add -f test.class
```
### 配置别名 ###
```
git config --global alias.ci commit #以后提交就可以简写成git ci -m "bala bala bala..."
git config --global alias.co checkout
git config --global alias.br branch
```
**配置文件**  
每个仓库的Git配置文件都放在.git/config文件中：
```
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true
    ignorecase = true
    precomposeunicode = true
[remote "origin"]
    url = git@github.com:michaelliao/learngit.git
    fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
    remote = origin
    merge = refs/heads/master
[alias]
    last = log -1
```

当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中
```
[alias]
    co = checkout
    ci = commit
    br = branch
    st = status
[user]
    name = Your Name
    email = your@email.com
```

### 搭建Git服务器 ###
服务器：Ubuntu或Debian

```
#安装git：
sudo apt-get install git
#创建一个git用户，用来运行git服务：
sudo adduser git
#创建证书登录
#收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。
#初始化Git仓库：
#先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：
sudo git init --bare sample.git
#不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：
sudo chown -R git:git sample.git
#禁用shell登录：
#出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：
#git:x:1001:1001:,,,:/home/git:/bin/bash
#改为：
#git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
#克隆远程仓库：
git clone git@server:/srv/sample.git

```
