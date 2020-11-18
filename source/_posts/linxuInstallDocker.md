---
title: linxuInstallDocker
abbrlink: 97c87514
date: 2019-03-05 15:14:07
tags:
  - LINXU
  - docker安装
subtitle: linxu安装docker
description: linxu安装docker
keywords: linxu安装docker
author: momo
---
- 使用Docker容器部署node.js快速方便，特别是应用较多时部署迁移等使用Docker会更方便。
每个node.js应用需要放在一个独立的环境Docker容器内运行，相互隔离，互不影响。
- Docker部署node.js应用的优点是什么？
使用Docker容器部署node.js快速方便，特别是应用较多时部署迁移等使用Docker会更方便。
- 使用Docker部署node.js应用，大体的流程是什么样的？
<!-- more -->
  - 服务器安装好Docker 
  - 本地应用根目录编写好`Dockfile`文件 
  - 将整个应用一起上传到服务器目录下 
  - 使用终端连接服务器执行命令安装Docker 
  - 部署成功。具体的操作请看下文。
  
### 服务器安装Docker
1.安装一些系统依赖
```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```
2.添加软件源，这里使用阿里的源，更快更稳定:
 
```
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linxu/centos/docker-ce.repo
```
3.更新 yum 软件源缓存，并安装 docker-ce:

```
sudo yum makecache fast
sudo yum install docker-ce
```
4.常用命令

```
#开启docker
sudo systemctl enable docker
sudo systemctl start docker
#重启docker
sudo service docker restart
#停止docker
service docker stop
#关闭docker
systemctl stop docker
```
5.查看Docker版本：

```
docker -v

```
6.安装 docker-compose
```
yum install -y docker-compose
```
### docker-compose管理多个docker
  - 一个使用Docker容器的应用，通常由多个容器组成。使用Docker Compose，不再需要使用shell脚本来启动容器。在配置文件中，所有的容器通过services来定义，然后使用docker-compose脚本来启动，停止和重启应用，和应用中的服务以及所有依赖服务的容器。完整的命令列表如下：

  ```
  # docker-compose up // 创建并启动容器
  build 构建或重建服务
  help 命令帮助
  kill 杀掉容器
  logs 显示容器的输出内容
  port 打印绑定的开放端口
  ps 显示容器
  pull 拉取服务镜像
  restart 重启服务
  rm 删除停止的容器
  run 运行一个一次性命令
  scale 设置服务的容器数目
  start 开启服务
  stop 停止服务
  up 创建并启动容器
  ```

  配置docker-compose.yml

  ```

  version: '1'
  services:
    gogs:
      image: gogs/gogs
      ports:
        - "10022:22"
        - 10080:3000
      volumes:
        - /vagrant/gogs-data:/data
      restart: always
    mysql:
      image: mysql
      container_name: mysql
      command: mysqld --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
      ports:
        - 3306:3306
      volumes:
        - "/e/docker/mysql/data/db:/var/lib/mysql"
        - "/e/docker/mysql/data/conf:/etc/mysql/conf.d"
      restart: always
      environment:
        - MYSQL_ROOT_PASSWORD=root
        - MYSQL_USER=momo
        - MYSQL_PASSWORD=admin520
        - MYSQL_DATABASE=gogs
    drone-server:
        image: drone/drone
          ports:
            - 80:8000
          volumes:
            - /var/lib/drone:/var/lib/drone
          restart: always
          environment:
            - DRONE_OPEN=false
            - DRONE_ADMIN=admin
            - DRONE_GOGS=true
            - DRONE_GOGS_URL=http://106.12.109.188:10080
            - DRONE_SECRET=momo
        drone-agent:
          image: drone/drone
          command: agent
          restart: always
          depends_on: [ drone-server ]
          volumes:
            - /var/run/docker.sock:/var/run/docker.sock
          environment:
            - DRONE_SERVER=ws://106.12.109.188/ws/broker
            - DRONE_SECRET=momo
  ```

### shipyard中文版安装

1.下载所需docker镜像

```
docker pull rethinkdb
docker pull microbox/etcd
docker pull shipyard/docker-proxy
docker pull swarm
docker pull dockerclub/shipyard
```
2.修改原安装脚本为中文版安装脚本

```
#下载官方脚本
wget https://shipyard-project.com/deploy
若下载失败请使用
wget https://raw.githubusercontent.com/shipyard/shipyard-project.com/master/site/themes/shipyard/static/deploy
grep -n shipyard:latest deploy
sed -i 's/shipyard\/shipyard:latest/dockerclub\/shipyard:latest/g' deploy
```
3.设置web访问端口(根据需要修改)

```
#检查8080端口是否被占用，若占用需修改端口
#安装net-tools工具包，若已安装可跳过此步骤
yum install -y net-tools    
#查看宿主机8080端口是否被占用
netstat -tlnp | grep 8080  
#配置修改
grep -n 'PORT:-8080' deploy
SHIPYARD_PORT=${PORT:-8080}
#修改为
SHIPYARD_PORT=${PORT:-指定端口}
```
4.安装和删除

```
sh deploy                                //安装
cat deploy | ACTION=remove bash          //删除
```
5.使用

```
浏览器输入:http://主机IP:8080
默认账号:admin
默认密码:shipyard
```
6. 安装过程中错误,常用的解决办法

```
#容器冲突：
#出现错误一般都是提示容器冲突,如果刚搭建,可以直接把容器全部停止并删除
docker stop $(docker ps -a -q)        //停止所有服务
docker rm $(docker ps -a -q)          //删除所有服务

#也可以根据提示来找到容器的ID进行停止删除
docker ps -a
docker stop ID
docker rm ID
```
7.如何增加一个节点

```
curl https://shipyard-project.com/deploy | ACTION=node DISCOVERY=etcd://主服务器IP:4001 bash 

```
### registry私有服安装
```
# 拉取docker-registry
docker pull registry:latest
# 运行私服registry
docker run -d -p 5000:5000 --name registry registry
```
### 安装mysql
```
#安装mysql
docker pull mysql
#运行mysql
docker run -d -p 3306:3306 -e MYSQL_USER="momo" -e MYSQL_PASSWORD="123456" -e MYSQL_ROOT_PASSWORD="123456" --name mysql mysql --character-set-server=utf8 --collation-server=utf8_general_ci
# -e MYSQL_USER="momo"  ：添加momo用户
# -e MYSQL_PASSWORD="123456"：设置添加的用户密码
# -e MYSQL_ROOT_PASSWORD="123456"：设置root用户密码
# --character-set-server=utf8：设置字符集为utf8
# --collation-server=utf8_general_cli：设置字符比较规则为utf8_general_cli
```

### 挂载外部配置和数据的两种方式
1.配置目录到映射
```
#创建docker/mysql目录
  mkdir /docker
  mkdir /docker/mysql
  mkdir /docker/mysql/conf
  mkdir /docker/mysql/data
#创建my.cnf配置文件
  touch /docker/mysql/conf/my.cnf
#my.cnf添加如下内容：
  [mysqld]
  user=mysql
  character-set-server=utf8
  default_authentication_plugin=mysql_native_password
  [client]
  default-character-set=utf8
  [mysql]
  default-character-set=utf8
  #运行mysql
  docker run -d -p 3306:3306 --privileged=true -v /docker/mysql/conf/my.cnf:/etc/mysql/my.cnf -v /docker/mysql/data:/var/lib/mysql -e MYSQL_USER="momo" -e MYSQL_PASSWORD="123456" -e MYSQL_ROOT_PASSWORD="123456" --name mysql mysql --character-set-server=utf8 
  #--privileged=true 容器内的root拥有真正root权限，否则容器内root只是外部普通用户权限
```
2.直接配置本地目录运行
```
# 1
docker run -d -p 3306:3306 --privileged=true -v /data/mysql/data:/var/lib/mysql -e MYSQL_USER="momo" -e MYSQL_PASSWORD="123456" -e MYSQL_ROOT_PASSWORD=123456 --name mysql mysql --character-set-server=utf8 
# 2
docker run --name gogs-mysql --restart=always -v /opt/mysql/mysqlVolume:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 -d mysql:5.7.19
```
### 修改用户访问权限
1.进入容器
```
docker exec -it mysql bash
```
2.登录mysql
```
mysql -u root -p
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
```

3.添加远程登录用户
```
CREATE USER 'momo'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
GRANT ALL PRIVILEGES ON *.* TO 'liaozesong'@'%';
flush privileges;
```

### 安装docker-compose
```
# 安装python-pip
yum -y install epel-release
yum -y install python-pip

pip install docker-compose
# 待安装完成后，执行查询版本的命令，即可安装docker-compose
docker-compose version
```