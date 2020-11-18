---
title: sass 在vue中的使用
tags:
  - vue
  - sass
abbrlink: 9908c040
date: 2019-01-07 09:57:44
description: sass使用 sass学习 sass在vue中使用
---
### 什么是`sass`?
  `SASS`是`CSS预处理器`，是一种专门处理`CSS`的编程语言，是一种CSS的开发工具，提供了许多便利的写法，大大节省了设计者的时间，使得CSS的开发，变得简单和可维护。
  - 安装`SASS`
    - 在vue中安装`SASS`
      ```
      npm i sass --save
      ``` 
<!-- more -->
  - 使用`SASS`
    - SASS文件就是普通的文本文件，里面可以直接使用CSS语法。文件后缀名是.scss，意思为Sassy CSS。 
    - 基本使用这一块我觉得阮一峰老师写的[SASS用法指南](http://www.ruanyifeng.com/blog/2012/06/sass.html)已经说得很明白了，在这里就不详细多说了。
  - 在`vue`中使用`SASS`
    - 在webpack中，所有预处理器都要匹配相应的loader,vue-loader允许其他的webpack-loader处理组件中的一部分吗，然后它根据lang属性自动判断出要使用的loaders。所以，其实只要安装处理sass/scss的loader。就能在vue中使用scss了。现在我们来安装sass/scss。
      ```
      npm install sass-loader node-sass --save-dev
      ``` 
  - 在`vue`中简单使用`SASS`
    ```
    <!-- src/components/index/index.vue -->
    <template>
    <div class="warp">
      <div class="box">123</div>
    </div>
    </template>
    <style lang="scss" rel="stylesheet/scss" scoped>
      @import './index.scss';
    </style>

    <!-- src/components/index/index.scss -->
    .warp {
      width: 100px;
      .box {
        color: red;
      }
    }
    ``` 