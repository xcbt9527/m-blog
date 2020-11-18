---
title: vue初步接触（二）
tags:
  - vue
abbrlink: cf2beb40
description: vue入门教程 vue学习 vue菜鸟 vue初涉
date: 2019-01-04 21:07:30
---
在上一篇文章中已经把`nodejs`安装完毕了，也把`vue`项目跑起来了。下面我们将初步接触vue。

### 目录结构&&了解使用
在菜鸟教程中，也已经有了`vue`的项目结构的文章,在这里我也简单的说一下。刚接触`vue`我们最主要着重看哪一方面的内容。[菜鸟教程传送门](http://www.runoob.com/vue2/vue-directory-structure.html)
下面的目录图是我们的生成vue项目之后打开可以看到的一些相关的目录文件。对于刚进门的同学而言，我们第一步需要了解的就是`package.json`文件。
在编辑器中打开此文件我们可以看到一个json结构的目录，在这里就简单说一下。
<!--more-->
![image](http://www.runoob.com/wp-content/uploads/2017/01/B6E593E3-F284-4C58-A610-94C6ACDAD485.jpg)
### `package.json`初步接触
在安装完项目以后我们需要使用`npm install`(已经安装淘宝镜像的使用`cnpm install`就可以了)安装一下依赖包（命令执行完之后会有`node_modules`这个文件夹），那么这些依赖包是从哪里来的呢？我们怎么知道我们安装了哪些依赖包呢？
![image](https://ws1.sinaimg.cn/large/006tNc79ly1fyul6zv9e5j311a0u00xg.jpg)
上面的图片已经简单的说了一下我们需要安装的依赖包和依赖了。那么我们需要怎么引入安装依赖呢？举个栗子，安装`axios`依赖包。
```
# 在命令行里面输入
cnpm install axios --save   #简写 cnpm i --save axios 其中包名和保存是不分先后的
```
### 开始编写我们的第一个`vue`文件
在我们的`vue`开发中，除了初期我们需要完善好我们的安装依赖，和共用的文件以外。我们的重心还是放在了我们的`vue`文件中。
首先，我们需要先运行项目，然后在浏览器中打开我们运行好的项目地址`localhost:8080`
```
npm run dev
```
在`webstrom`中打开我们的文件夹`src/components/HelloWord.vue`文件。
然后修改里面的内容,刷新页面我们就可以看到修改的东西了。
```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>
 
<script>
export default {
  name: 'hello',  // 组件名字
  data () { // data数据源
    return {
      msg: '欢迎来到菜鸟教程！'
    }
  }
}
</script>
```
### 接入`axios`进行ajax数据请求
  ####### `axios`是什么？
  `axios`是vue的异步请求的ajax封装好的一个包,在vue中，我们只需要直接引入就行了。[详细地址](https://www.kancloud.cn/yunye/axios/234845)
  ```
  cnpm i axios --save
  ```
  然后需要在`/src/main.js`中进行全局引入
  ```
  import axios from 'axios';  // 引入axios
  Vue.prototype.$http = axios;
  把axios挂载在vue上
  ```

### 解决`axios`跨域问题
  
  修改`config/index.js`文件

  ```
  # config/index.js
  # 修改相关跨域问题处理 直接查找相关key字段，并替换成一下的（ps：key就是autoOpenBrowser，value就是true）
  dev:{
    autoOpenBrowser: true,
		errorOverlay: true,
		notifyOnErrors: true,
    proxyTable: {
			'/': {
				//将api.zhuishushenqi.com印射为/api
				target: 'http://api.zhuishushenqi.com', // 接口域名
				secure: false, // 如果是https接口，需要配置这个参数
				changeOrigin: true, //是否跨域
			},
		},
  }
  ```
### 初尝`axios`请求
  本次示范使用小说网站来做示范，如有侵权，请联系作者删除。
  在`src`下面新建文件夹和文件 `index/index.vue`,然后在新建的文件下面输入以下代码
  ```
  # index/index.vue
<template>
  <div class="warp">
    <div class="box">
      <div class="title">男生小说</div>
      <ul>
        <!-- 这里使用的是for循环，for按照正规的规范还需要放置key保证唯一性 -->
        <!-- 本次教程中的v-for 循环中item就是数组里面的对象，index就是下标索引，我们这里的index索引就是用来当key值使用 -->
        <!-- :key 相当于 v-bind:key，只是一个简单的缩写 -->
        <li v-for="(item,index) in classification.male"
            :key="index">
          <!-- 把相关渲染的字段输出 -->
          <div>{{item.name}}</div>
          <div>{{item.bookCount}}</div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    name: "index", //组件名
    data() {
      // 存放数据源的钩子函数
      return {
        classification: {}
      };
    },
    /**
     * 在加载完vue之后执行的钩子函数
     */
    mounted() {
      this.init();
    },
    /**
     * 存放函数的钩子函数
     */
    methods: {
      /**
       * 此处的方法名相当于 function init(){}
       */
      init() {
        /**
         * 此处的this是本组件的指向
         */
        this.getClassification(); //获取分类
      },
      /**
       * 获取分类
       */
      getClassification() {
        // ({data}) 这是es6语法，意思是获取到最外层的data的那个字段
        this.$http.get("cats/lv2/statistics").then(({ data }) => {
          this.classification = data;
        });
      },
      funImg(icon) {
        const href = "http://statics.zhuishushenqi.com" + icon;
        return href;
      }
    }
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  .box {
    border: 1px solid #dedede;
  }
  ul li {
    list-style: none;
    display: inline-block;
    padding: 10px;
    text-emphasis: center;
  }
</style>

  ```
  然后你还需要引入这个组件，把他设置成路由可供我们在首页查看
  修改路由信息`src/router/index.js`:
  ```
  # 删除HelloWold,因为这个是项目自带的，直接删除就行可以
    import HelloWold from '@/components/HelloWold';
  # 引入index
    import indexPage from '@/components/index/index';
  # 修改路由，把HelloWold都删除了
  # mode ：vue-router 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。 如果不想要很丑的 hash，我们可以用路由的 history 模式，这种模式充分利用 history.pushState API 来完成 URL 跳转而无须重新加载页面。
  export default new Router({
	mode: 'history', 
	routes: [
		{
			path: '/',
			name: 'index',
			component: index,
		},
	],
});
  ```
  至此可以在页面上直接看到男生小说的输出。