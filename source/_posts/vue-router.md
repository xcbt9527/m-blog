---
title: vue-router
date: 2019-02-18 09:47:14
tags: vue
subtitle: vue-router带参数跳转的两种方式
description:  vue-router带参数跳转的两种方式
keywords: vue-route,vue路由带参跳转的两种方式 
author: momo
abbrlink: 6098be36
---
[vue-router](https://router.vuejs.org/zh/)在vue的日常开发中是必不可少的一部分。这篇文章就简单说一下vue-router带参数跳转的两种方式。
<!-- more -->
### 普通直接跳转
  vue-roter普通跳转直接使用`this.$rote.push`进行跳转
  ```
  this.$router.push({
        path: '/index',
      });
  ```
### 直接使用路径跳转
  我们平时看小说的时候，会发现不同的书本，地址的链接不一样，不只是带的参数不一样，连地址的二级路径三级路径也有可能不一样。如：https://www.qu.la/book/101104/
    可以看到笔趣阁的这个路径，book后面的就是书本的id，我们需要传输book后面的三级路径的名字当成id传输给后台。
    在vue-roter里面是这样的：
    ```
    <!-- router/index.js -->
    <!-- 设置二级路径的地址为可带参数 -->
    {
      path: '/book/:id',
      name: 'book',
      component: bookView,
    }
    <!-- 在参数上带上二级路径参数地址 -->
    <!-- 点击跳转 -->
    this.$router.push({
        path: '/book/101104',
    });
    <!-- 获取id -->
    <!-- book/index.js -->
    <!-- watch是监听属性 -->
    watch: {
    $router: () => {
      if (this.$route.name === 'book') { 
        console.log(this.$route.params.id);
        }
      },
    }
    ```
### 在连接上带上参数
  平时如果多细心留意看的话，我们会经常看到一些链接上面都是很长的，或者是有一些奇怪的参数。如：https://www.qu.la/book/101104?cfrom=momo
  这种就是直接在链接上面添加了参数，在跳转的时候获取
  使用为：
  ```
  <!-- index/index.js -->
  this.$router.push({
        path: '/book/101104',
        query: {
          cfrom: 'momo',
        },
      });
    <!-- book/index.js -->
    console.log($route.query.cfrom);
  ```