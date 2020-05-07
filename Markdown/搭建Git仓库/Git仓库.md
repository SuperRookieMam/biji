###git仓库搭建：
####1. 安装
* yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-devel
* yum install git
####2. 创建用户
* adduser git 
 ####3.创建证书登录：
* 收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。
####第4. 初始化Git仓库：
* 先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：<br>
    git init --bare sample.git
* 给文件加分组：

        Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，
        并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：
    chown -R git:git sample.git
####第五步，禁用shell登录：

    出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行 
    git:x:1003:1003::/home/qixiao:/bin/bash
    改为：
    git:x:1003:1003::/home/qixiao:/bin/git-shell
    
    或者使用命令 
    passwd  -l  git 锁定后就不能登录
    passwd  -u  user 不锁定就乐意登录了
    
#### 修改git端口号
您可以使用git remote set-url CLI命令更新SSH URL中的端口。例如，要将源远程端口更新为端口3322，可以运行以下命令：
  git remote set-url origin ssh://username@<git服务器地址>:3322/opt/git/demo.git    
#### 创建允许git clone 的用户   
* 如果没有创建 git 用户组创建用户组 groupadd gitgroup
* 修改git仓库所在目录/srv/sample.git的用户组为gitgroup 


     chgrp -R gitgroup /srv/est.git
    

* 创建一个新用户  adduser XXX  
* 允许用户登录 passwd qixiao
* 将用户qixiao添加到用户组gitgroup

        usermod -G gitgroup git
    
    
    