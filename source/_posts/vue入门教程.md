---
title: vue入门教程（-）
tags:
  - vue
abbrlink: c8396672
description: vue入门教程 vue学习 vue菜鸟 vue初涉
date: 2019-01-04 20:13:23
---
现在网络上vue的入门教程已经很多，并且`菜鸟教程`等都已经录入了[入门教程](http://www.runoob.com/w3cnote/vue-js-quickstart.html),在这里我也简单的说一下window从安装到打开项目的一些注意事项。
<!--more-->
### 步骤一：安装nodejs环境
  下载[安装包](https://nodejs.org/en/download/),下载node的安装包，mac为pkg格式，windows为msi格式.

  * [mac版本](http://www.runoob.com/nodejs/nodejs-install-setup.html)
    * 通过安装包来安装nodejs 
      * 双击安装包直接安装即可,安装包会一键同时安装node和npm，无需再额外安装npm。查看是否安装成功。打开终端，输入命令查看
      ```
            // 通常的， -v都是查看版本号：不简写是：-version，-help是查看帮助
            node -v // 查看nodejs是否安装成功,如安装成功会现实版本号如：v8.11.3
            npm -v // 查看npm是否安装成功,如安装成功会现实版本号如：v8.11.3
      ```
  * [windows版本](http://www.runoob.com/nodejs/nodejs-install-setup.html)    
      * 通过安装包来安装nodejs [32位安装包](https://nodejs.org/dist/v4.4.3/node-v4.4.3-x86.msi)/ [64位安装包](https://nodejs.org/dist/v4.4.3/node-v4.4.3-x64.msi)
        * 运行安装包，一直点击Run，等到第五个步骤有一个树形图标，勾选最后一个`add to path`。添加到本地环境地址。否则需要自己配置。
          查看是否安装成功：
      ```
      # 检测PATH环境变量是否配置了Node.js，点击开始=》运行=》输入"cmd" => 输入命令"path"，输出如下结果：
        PATH=C:\oraclexe\app\oracle\product\10.2.0\server\bin;C:\Windows\system32;
        C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;
        c:\python32\python;C:\MinGW\bin;C:\Program Files\GTK2-Runtime\lib;
        C:\Program Files\MySQL\MySQL Server 5.5\bin;C:\Program Files\nodejs\;
        C:\Users\rg\AppData\Roaming\npm
      # 我们可以看到环境变量中已经包含了C:\Program Files\nodejs\
      检查Node.js版本
      # -v都是查看版本号：不简写是：-version,如安装成功会现实版本号如：v8.11.3
      node -v
      ```

### 步骤二安装`cnpm`淘宝镜像
  这是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。
  运行命令添加淘宝镜像：
      # npm install 用于安装线上依赖包
      # -g 安装到全局的node_modules上 --save是仅安装到本项目下
      # cnpm 是命名依赖的名字 这个比较少用
      npm install -g cnpm --registry=https://registry.npm.taobao.org
      # 查看版本是否安装成功 如果安装成 第一个是cnpm的版本号如：cnpm@6.0.0（xxxxxxxx）
      cnpm -v 

### 步骤三 安装`vue`
  菜鸟教程是直接安装`vue`来使用。但是日常生活中我们一边都会安装`vue-cli`来.
    # 因为在步骤三安装了cnpm 所以下面的我们都会使用cnpm来进行安装依赖（ps：如果不懂的话 你可以理解为：cnpm是国内淘宝的网站的东西，而npm则是国外的东西。没有其他差别，但是访问国内的肯定速度上快很多）
    cnpm install -g @vue/cli
    # 查看vue是否安装成功
    vue -V
    # vue --version 查看返回版本号：如：2.9.6
### 步骤四 使用`vue-cli`安装项目
  * mac系统进入桌面上安装vue项目
    ```
    # 进入终端运行一下命令
    cd Desktop  # 进入桌面
    ```
  * windows系统进入d盘安装vue项目
    ```
    # 进入cmd运行一下命令
    D:  # 进入d盘
    ```
  然后进行`vue`项目的安装
  ```
  # vue init webpack 使用webpack构建vue初始化项目
  vue init webpack my-project
  # 然后会询问你一些相关项目问题
  # Project name (my-project) // 是否使用my-project作为项目名字 一般是回车
  # Project description (A Vue.js project)  // 项目介绍 回车
  # Author // 作者 回车
  # Vue build // 选择构建的方式 回车
  # Install vue-router? (Y/n) // 是否使用vue-router？ Y 回车 以后会说到
  # Use ESLint to lint your code? // 是否使用eslint控制你的代码 n 回车 不使用，以后会说到
  # Set up unit tests // 是否设置单元测试 n 回车 不使用，以后会说到
  # Setup e2e tests with Nightwatch? // 不使用 n回车  //这些暂时不需要关注，后期再去关注
  # Should we run `npm install` for you after the project has been created? (recom mended) // 使用哪一种方式安装依赖，我们自己安装 如果你安装有了yarn也可以。不推介直接使用npm（慢），选择第三个 No, I will handle that myself
  # 因为我们选择了自己安装 所以在运行完以后需要到文件目录下安装依赖
  # mac和window进入目录命令都是一样
    cd my-project
    cnpm i // 安装依赖，简写等价于 cnpm install
  # 在加载完之后，我们就可以使用命令跑起项目来了
   npm run dev
  # 提示进入`http://localhost:8080`之后，直接去浏览器打开地址。
  # 到此，vue的安装结束。
  ```