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
