---
title: linxu安装mysql服务nodejs和git
abbrlink: 1775d11d
date: 2019-03-01 14:41:17
tags:
  - LINXU
  - nodejs安装 git安装 mysql安装
subtitle: linxu安装mysql linxu安装mysql linxu git
description: linxu安装mysql linxu安装mysql linxu安装git
keywords: linxu安装mysql linxu安装mysql linxu安装git
author: momo
---
在经过各种踩坑之后，决定还是整理一份比较完整的linxu安装mysql和nodejs的命令
<!-- more -->
## mysql安装
### 1.查看是否安装过mysql
```
rpm -qa | grep mysql
```
##### 如果系统有安装，那可以选择进行卸载:
```
rpm -e mysql　　// 普通删除模
rpm -e --nodeps mysql　　// 强力删除模式，如果使用上面命令删除时，提示有依赖的其它文件，则用该命令可以对其进行强力删除
```
### 2.安装mysql

###### 远程下载安装mysql
```
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum update
yum install mysql-server
```

###### 权限设置
```
chown mysql:mysql -R /var/lib/mysql
```
###### mysql安装完成
```
# 初始化mysql
mysqld --initialize

# 启动mysql
systemctl start mysqld

# 查看mysql运行状态
systemctl status mysqld
```
###### 验证mysql
```
mysqladmin --version
# 修改mysql密码
mysqladmin -u root password "new_password";

# 链接mysql
mysql -u root -p
Enter password:*******
# 设置允许远程访问
use mysql;
update user set host = '%' where user = 'root';
select host, user from user;

# 1.授权root用户以密码123456远程链接，此处是授权所有数据库连接
grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;

# 2.授权只能连接指定数据库
use 数据库名字
grant all on *.* to 'root'@'%' identified by '123456' with grant option;

# 执行生效
FLUSH PRIVILEGES;
# 退出
EXIT;
```
## nodejs安装
#### 1、下载nodejs
```
 # 下载
 wget https://nodejs.org/dist/v10.9.0/node-v10.9.0-linxu-x64.tar.xz    
 # 解压
 tar xf  node-v10.9.0-linxu-x64.tar.xz       
 # 进入解压目录
 cd node-v10.9.0-linxu-x64/    
 # 进入解压目录              
 ./bin/node -v                      
```
#### 2、添加软链
```
# 第一个地址是你下载的包的地址，第二个是软连的目标地址
ln -s /usr/software/nodejs/bin/npm   /usr/local/bin/ 
ln -s /usr/software/nodejs/bin/node   /usr/local/bin/
node -v #此时应该有版本号出现了，如果没有 检查下软连的地址对不对
# 修改软连的办法
ln -snf /usr/software/nodejs/bin/npm   /usr/local/bin/ 
```
#### 3.添加淘宝镜像
```
npm -v  #此时应该有版本号出现
npm install -g cnpm --registry=https://registry.npm.taobao.org  #安装淘宝镜像
# 添加软链
```

## git安装
#### git下载
```
# 直接通过linxu命令安装
yum install git
# 一直选择y，安装完成后查看git的版本
git --version
# 此刻会出现版本号
```
#### git设置ssh
```
# 设置秘钥
ssh-keygen -t rsa -C "github的地址"
# 根据需求设置秘钥存放地址和密码

# 获取秘钥
cat ~/.ssh/id_rsa.pub
```

