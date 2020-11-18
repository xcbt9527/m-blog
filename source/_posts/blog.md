---
title: 新建一个博客步骤
categories: hoex
tags:
  - hoex
description: hexo在coding下安装部署 hexo安装 hexo部署 hexo学习
abbrlink: d92ac943
date: 2018-06-07 09:57:44
---
#### 1、本地初始化安装hexo
在开始之前你需要在coding上申请一个账号并且把ssh放到电脑里。[传送门](https://coding.net/help/doc/account/ssh-key.html)

<!--more-->
```
cnpm i -g hexo  //全局安装 hexo
hexo init hexo  // (文件夹名字) 初始化 hexo
cd hexo (进入文件夹)
git init // 初始化 git
git remote add origin https://git.coding.net/momolw/hexo.git // 添加到你的coding项目中
git pull    // 拉去远程的分支下面的目录
git checkout -b hexo
git add .   // 把所有本地目录都添加到git中
git commit -m 'init' // 提交所有本地文件 并命名为init
git push origin hexo  // 推送到git的hexo分支中,master主要用来上传发布好的文章
cnpm i // 安装依赖
hexo server //运行本地项目 在浏览器http://localhost:4000 可以访问

```
#### 2、Hexo配置

   打开项目下的 _config.yml修改相关博客相关信息

|参数             | 描述           |
| ---------     |-------------|
| title          | 网站标题
|subtitle        | 网站副标题     
|description     | 网站描述,主要用于SEO
|author          | 作者 
|language        | 网站使用的语言
|timezone        | 网站时区。hexo默认使用电脑时区


-  修改deploy 

```
deploy:
  type: git
  repository:
     coding: https://git.coding.net/xxxx/xxxx   //博客地址
```

#### 写文章

  1、执行hexo new xxx 生成文章目录
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 进入source/_posts目录,自己直接新建md文件，用这个命令的好处是帮我们自动生成时间，一般的完整格式如下。

|参数             | 描述           |
| ---------     |-------------|
| title          | #文章页面上的显示名称，一般是中文
|date        | #文章生成时间，一般不改，当然也可以任意修改  
|categories     | 默认分类 #分类
|tags:          | [tag1,tag2,tag3] 文章标签，可空，多标签请用格式，注意:后面有个空格
|description        | 附加一段文章摘要，字数最好在140字以内，会出现在meta的description里面
  2、 进入source/_posts目录，打开刚刚新建的文章，开始创作。
  3、 写完文章后执行 hexo generate 生成修改后的博客页面。
  4、 再执行 hexo deploy 就可以发布到github了。



#### 打包部署
  - 在coding的项目->代码->pages服务->部署
  - 代码分支我们默认选择master，如果是付费用户可以选择多分支。
  - 下面还有一些自定义域名和ssl安全证书这些相关的。如果需要用到的话，可以选择填写。
  ```
    // d是deploy简写
    hexo d -g //打包所有文章并推送到coding上
  ```
#### Hexo常用命令

  ##### 常见命令 
  ```
  hexo new "postName" // 新建文章
  hexo generate // 生成静态页面至public目录
  hexo server // 开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
  hexo deploy // 部署到GitHub
  hexo help  // 查看帮助
  hexo version  // 查看Hexo的版本
  ```

  ##### 缩写
  ```
  hexo n == hexo new
  hexo g == hexo generate
  hexo s == hexo server
  hexo d == hexo deploy
  ```

  ##### 组合命令
  ```
  hexo s -g #生成并本地预览
  hexo d -g #生成并上传
  ```  

