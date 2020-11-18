---
title: es6和数组结合的算法应用例子
tags:
  - es6
  - Array数组
description: es6和数组的应用 js数组应用
abbrlink: 80ede8c8
date: 2018-10-18 09:58:19
---
# JavaScript es6数组学习
#### 使用 constructor 判断对象类型

&nbsp;&nbsp;&nbsp;&nbsp;一个对象的constructor是它的构造函数的prototype.constructor，而每一个函数都有一个prototype，默认情况下，这个prototype有一个constructor属性，指向的是它自己。

<!--more-->
```
var test=new Object();
// 返回对创建此对象的数组函数的引用
console.log(test);
if (test.constructor===Array)
{
document.write("This is an Array");
}
if (test.constructor===Boolean)
{
document.write("This is a Boolean");
}
···
// 使用instanceof 运算符用来检测 constructor.prototype 是否存在于参数 object 的原型链上。

const arr = new Arrany();
arr instanceof Arrany //true

// 定义构造函数
function C(){} 
function D(){} 

var o = new C();

o instanceof C; // true，因为 Object.getPrototypeOf(o) === C.prototype
o instanceof D; // false，因为 D.prototype不在o的原型链上

o instanceof Object; // true,因为Object.prototype.isPrototypeOf(o)返回true
C.prototype instanceof Object // true

```
#### Array 数组在实际中的使用
  需求：把选中的对象插入到第一条
  分析：首先需要新建一个子数组，然后遍历寻找到数组中的选中的值，然后删除，再向数组的开头插入
```
const arr = new Array(1, 2, 3, 4, 5)
// 新建一个子数组
    arr.slice().forEach((res, index) => {
      if (res === 2) {
      // 删除选中的索引的数组，如果不指定删除个数，会直接删除后面所有的对象
        arr.splice(index,1)
        // 在数组开头插入选中的数据
        arr.unshift(res)
      }
    })
    // 结果
    console.log(arr) //[2,1,3,4,5]
```
  需求：删除小于0的数并根据从小到大排序，然后选中最接近5的数据。
  分析：先过滤小于0的数据，并根据大小排序，然后使用Math.abs获取相差的数，然后返回相差最小的数
```
const arr = new Array(5,6,81,2,13,-1,-4,-6,7,9);
const arrfilter = arr.filter(res=>res>0);
const arrmin = new Array();
// 从小到大排序
arrfilter.sort((a,b)=>{return a-b})
// 遍历获取相差值
arrfilter.forEach(res=>{
	arrmin.push(Math.abs(res -5))
})
// 获取最小值的索引
const index = arrmin.indexOf(Math.min.apply(null,arrmin))   // 获取到1
//获取到最接近5的值
arrfilter[index]    //输出5
```


