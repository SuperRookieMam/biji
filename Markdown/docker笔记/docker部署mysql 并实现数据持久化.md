#mysql部署
#### 在你的linux 创建文件夹，我的文件夹接口目录如下：

        usr/local/docker
                   ├── mysql/conf
                       │      ├──my.conf
                       │      ├── privileges.sql
                       │	  └── setup.sh
                       └──DockerFile
                       │
                       └──docker-compose.yml 
        
* 创建privileges文件

        -- 创建可远程登陆的mysql设置
        use mysql;
        -- 设置root 允许登录的Ip为所有
        update user set host='%' where user ='root';
        -- 设置root 允许登录的Ip为所有
        update user set plugin='mysql_native_password' where user ='root';
        -- 设置允许远程登录所有操作
        ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'InitMysql@2020';
        -- 刷新Mysql 权限
        flush privileges;
* 创建 setup.sh

        #!/bin/bash
        set -e
        
        echo '1.启动mysql....'
        #启动mysql
        service mysql start
        
        echo '4.开始修改密码....'
        mysql < /mysql/privileges.sql
        
        echo `mysql容器启动完毕,且数据导入成功`
* 创建 Dckerfile 
        
        FROM mysql:8
        
        #设置免密登录
        ENV MYSQL_ALLOW_EMPTY_PASSWORD yes
        #将所需文件放到容器中
        COPY conf/setup.sh /mysql/setup.sh
        #执行可远程登录的sql
        COPY conf/privileges.sql /mysql/privileges.sql
        
        #设置容器启动时执行的命令
        CMD ["sh", "/mysql/setup.sh"]


*  创建 docker-compose.yml


    version: "3"
    services:
      mysql-yhl:
        # 指定容器的名称
        container_name: "mysql-yhl"
        # 指定镜像和版本
        image: "mysql8:yhl"
        ports:
          - "3306:3306"
    #  environment:
    #    MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
    #    MYSQL_ROOT_HOST: ${MYSQL_ROOT_HOST}
        volumes:
          # 把定义的数据目录挂载到docker
          - "/usr/local/docker/mysql/data:/var/lib/mysql"
          # 把定义的数据目录挂载到docker
          - "/usr/local/docker/mysql/conf:/etc/mysql/conf.d"
        #docker 重启后，容器自启动
        restart: always
    


       
