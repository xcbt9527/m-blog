---
title: 前端备考面试题
tags: web前端面试
subtitle: web前端面试题
description: web前端面试题
author: 墨墨
abbrlink: da135270
date: 2020-12-01 16:38:19
keywords:
---

### call() 与 apply() 和 bind（）

其实对于 call apply 两者而言,作用完全一样,只是接受参数的方式不太一样

- call()接收的是参数列表
- apply()接收参数数组
- bind()方法创建一个新的函数，在 bind()被调用时，这个新函数的 this 被 bind 的第一个参数指定，其余的参数将作为新函数的参数供调用时使用。

例如:有一个函数定义如下

```javascript
apply 的实现：

Function.prototype.myApply= function(context){
context.fn = this;//1.将函数挂载到传入的对象
var arg = [...arguments].splice[1](0);//2.取参数
if(!Array.isArray(arg)) {
throw new Error('apply 的第二个参数必须是数组') //3.限制参数类型为数组
}
context.fn(arg) //4.执行对象的方法
delete context.fn; //5.移除对象的方法
}

var obj = {
name:'obj'
}
function sayName(arr){
console.log(this.name,arr)
}
sayName.myApply(obj,[1,2,3]) //obj [1, 2, 3]
```

```javascript
call实现：与apply的唯一区别就是参数格式不同

Function.prototype.myCall= function(context){
    context.fn = this;//1.将函数挂载到传入的对象
    var arg = [...arguments].splice(1);//2.取参数
    context.fn(...arg) //3.执行对象的方法
    delete context.fn; //4.移除对象的方法
}
var obj = {
    name:'obj1'
}
function sayName(){
    console.log(this.name,...arguments)
}
sayName.myCall(obj,1,2,3,5) //obj1 1,2,3,5
```

```javascript
bind在mdn上的实现：

Function.prototype.myBind = function(oThis){
    if(typeof this !== 'function'){
        throw new TypeError('被绑定的对象需要是函数')
    }
    var self = this
    var args = [].slice.call(arguments, 1)
    fBound = function(){ //this instanceof fBound === true时,说明返回的fBound被当做new的构造函数调用
        return self.apply(this instanceof fBound ? this : oThis, args.concat([].slice.call(arguments)))
    }
    var func = function(){}
    //维护原型关系
    if(this.prototype){
        func.prototype = this.prototype
    }
    //使fBound.prototype是func的实例，返回的fBound若作为new的构造函数，新对象的__proto__就是func的实例
    fBound.prototype = new func()
    return fBound
}

```

### vue 双向数据绑定

实现数据绑定的做法有大致如下几种：

> 发布者-订阅者模式（backbone.js）

- 发布者-订阅者模式: 一般通过 sub, pub 的方式实现数据和视图的绑定监听，更新数据方式通常做法是 vm.set('property', value)

> 脏值检查（angular.js）

- angular.js 是通过脏值检测的方式比对数据是否有变更，来决定是否更新视图，最简单的方式就是通过 setInterval() 定时轮询检测数据变动，当然 Google 不会这么 low，angular 只有在指定的事件触发时进入脏值检测，大致如下：

> 数据劫持（vue.js）

- 数据劫持: vue.js 则是采用数据劫持结合发布者-订阅者模式的方式，通过 `Object.defineProperty()`来劫持各个属性的 `setter`，`getter`，在数据变动时发布消息给订阅者，触发相应的监听回调。

要实现 mvvm 的双向绑定，就必须要实现以下几点：

> 1、实现一个数据监听器 Observer，能够对数据对象的所有属性进行监听，如有变动可拿到最新值并通知订阅者
> 2、实现一个指令解析器 Compile，对每个元素节点的指令进行扫描和解析，根据指令模板替换数据，以及绑定相应的更新函数
> 3、实现一个 Watcher，作为连接 Observer 和 Compile 的桥梁，能够订阅并收到每个属性变动的通知，执行指令绑定的相应回调函数，从而更新视图
> 4、mvvm 入口函数，整合以上三者

[双向数据绑定传送门](https://juejin.cn/post/6844903854740340749)
[响应式](https://juejin.cn/post/6844903561327820808)
[图解 Vue 响应式原理](https://juejin.cn/post/6857669921166491662)
[gulp 雪碧图压缩](https://blog.csdn.net/weixin_41350225/article/details/88954426)

### 首屏优化

- 雪碧图
- 分组打包
- 优先打包
- 代码压缩
- 开启 gzip
- 取消 console 的打印
- 代码切割
- 版本管理
