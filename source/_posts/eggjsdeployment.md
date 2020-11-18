---
title: eggjsdeployment
abbrlink: 91c78c33
date: 2019-03-29 11:17:31
tags:
  - LINXU
  - eggjs
subtitle: LINXU部署eggjs
description: LINXU部署eggjs
keywords: LINXU部署eggjs
author: momo
---
# LINXU中部署eggjs和相关环境

开发nodejs相关也已经有两年时间了，尽管公司已经构建好了相关的打包工具，让开发变得轻松便捷，但是个人对于构建nodejs和发布部署这一块还是不是很了解。这段时间闲余之时对这一块进行了了解和深入。

 - 在nodejs的官方文档上有关于nodejs应用程序docker化的解决方案：[把一个 Node.js web 应用程序给 Docker 化](https://nodejs.org/zh-cn/docs/guides/nodejs-docker-webapp/)。
 - 关于eggjs打包成docker的问题，在官方文档中也提供了部分参考。[eggjs部署](https://eggjs.org/zh-cn/core/deployment.html)  
 <!-- more -->
### LINXU安装nodejs环境
```
 # 下载
 wget https://nodejs.org/dist/v10.9.0/node-v10.9.0-linxu-x64.tar.xz    
 # 解压
 tar xf  node-v10.9.0-linxu-x64.tar.xz       
 # 进入解压目录
 cd node-v10.9.0-linxu-x64/    
 # 查看node是否安装完成              
 ./bin/node -v        
# 安装淘宝镜像
./bin/npm install -g cnpm --registry=https://registry.npm.taobao.org
# 基于 easywebpack-cli 模式构建Vue前端渲染项目
./bin/cnpm i easywebpack-cli  -g
 # 软连接 第一个地址是下载包的地址，第二个是软连的地址
ln -s /usr/software/nodejs/bin/* /usr/local/bin/ 
```
###  easywebpack 进行项目初始化和打包

##### 1.运行Egg + Vue 工程化应用程序
```
# 1.生成初始化骨架项目
easywebpack init
# 2.选择 Create Egg + Vue Application && Create Egg + Element UI Client Render Single Page Application 初始化骨架项目
# 3.安装依赖
cnpm install --production
# 4.运行
npm run dev
```
##### 2.打包
因为购买的是最低配置的云主机，在云主机跑可能会有点卡 所以就在本地打包好再上传上去

```
npm run build
```
##### 3.注意事项
- 关于打包报错问题
   - 如果构建的时候报错```easywebpack build prod```或者```easy build prod```需要安装两个依赖：```cnpm i --save easywebpack-vue imagemin-webpack-plugin```,如果```imagemin-webpack-plugin```安装失败删除了```node_modules```再重新试试
- sass目前已经是我使用最多的css预处理器语言：一时用一时爽，一直用一直爽
  - 在```easywebpack3.x```版本是默认开启了CSS预处理器语言```sass、less```等，但是在4.x版本之后默认关闭了css预处理语言。相关详情请看：[easywebpack4版本特性](https://www.yuque.com/easy-team/easywebpack/v4)，[webpack.config.js配置构建-loader](https://www.yuque.com/easy-team/easywebpack/loader)

### docker环境配置
docker在linxu的环境安装

```
# 1.安装一些系统依赖
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
# 2.添加软件源，这里使用阿里的源，更快更稳定:
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linxu/centos/docker-ce.repo
# 3.更新 yum 软件源缓存，并安装 docker-ce:
sudo yum makecache fast
sudo yum install docker-ce
# 4.开启docker
systemctl enable docker
systemctl start docker
# 5.查看docker版本
docker -v

# 6.常用命令
# 查看镜像
docker images
# 查看容器运行情况 -a是查看所有容器运行情况
docker ps -a
# 删除容器
docker rm (IMAGE ID)
# 删除镜像
docker rmi (IMAGE ID)
# 重启docker
service docker restart
# 停止docker
service docker stop
# 关闭docker
systemctl stop docker
```

#### docker打包镜像
使用docker打包镜像之前 需要一个Dockerfile配置文件，文件如下：
在打成docker之前就已经安装好依赖和打包，这里只需要打包成镜像就行了

```
FROM node
# 创建app目录
RUN mkdir -p /usr/src/admin/admin
# 设置工作目录
WORKDIR /usr/src/admin/admin
# 拷贝所有源代码到工作目录
COPY . /usr/src/admin/admin
# 进入工作目录文件夹
RUN cd /usr/src/admin/admin
# 暴露容器端口
EXPOSE 7001
# 启动node应用
CMD npm start
```
安装docker镜像

```
docker build -t node/eggjs:1.0 .
```

#### docker镜像运行

```
# 查看是否有此镜像
docker images
# 启用容器
# 1.eggjs容器启用
# -d为后台运行容器，容器使用7001端口，与Dockerfile里面的一致。
sudo docker run -d --net=host --name eggjs node/eggjs

# 2.普通node.js应用
sudo docker run -d --name eggjs -p 7001:7001 node/eggjs:1.0
```

