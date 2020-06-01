### 1、下载并安装MySQL
* 安装mysql 以来到yum
    
    
    wget -i -c https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm

* 下载到指定位置后，就可以执行安装了


    yum -y install mysql80-community-release-el7-3.noarch.rpm
    yum -y install mysql-community-server
###  2、启动MySQL

* 启动MySQL服务：systemctl start  mysqld.service
* 查看MySQL服务：systemctl status  mysqld.service

* 查看MySQL是不是开机自启，可以执行命令查看开机自启列表

    
    systemctl list-unit-files|grep enabled
* 此时如果要进入MySQL得找出root用户的密码，输入命令


    grep "password" /var/log/mysqld.log

* 得到密码后，登录mysql，输入命令


    mysql -uroot -p

* 最后一步，设置允许远程连接。


    如果直接使用命令：GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password'; 
    会提示一个语法错误，有人说是mysql8的分配权限不能带密码隐士创建账号了，要先创建账号再设置权限。也有的说8.0.11之后移除了grant 添加用户的功能。

    创建新用户 admin
    创建用户：CREATE USER 'admin'@'%' IDENTIFIED BY '123456';
    允许远程连接：GRANT ALL ON *.* TO 'admin'@'%';
    本人测试过，使用 update user set host = '%'  where user = 'root'; 也可以修改
    刷新数据库权限 flush privileges;
