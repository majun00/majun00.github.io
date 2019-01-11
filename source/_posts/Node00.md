---
title: 浅谈Node的异步方式和模块开发
tags: 
- NodeJS
- 异步编程
categories:
- NodeJS
---

# Node异步方式
#### 什么是Node?
简单的说 Node.js 就是运行在服务端的 JavaScript。Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。从上面这段话我们可以发现Node有以下特性 :
1. 基于chrome v8引擎
2. 单线程
3. 非阻塞io 基于事件驱动
4. common规范

#### 异步编程
我们常常谈到`单线程/多线程``阻塞/非阻塞``同步/异步`等问题, 那么Node的异步编程是怎么实现的呢? 首先看看异步编程的实现方式有哪些(暂不谈论ES6异步编程) :
1. 回调函数
2. 事件监听
3. 订阅/发布

那么Node是如何实现异步操作的呢?
服务端 : 同步
浏览器(客户端) : 异步 多线程 事件驱动(事件队列)
JS : 单线程 阻塞
nodejs :单进程 单线程 异步编程:回调 事件驱动(事件队列)处理并发

js的运行时单线程的 : 引入事件队列机制
Node.js中的事件模型与浏览器中的事件模型类似 : 单线程+事件队列

#### 异步I/O
1. 文件操作
2. 网络操作

在浏览器中也存在异步操作：
1. 定时任务
2. 事件处理
3. Ajax回调处理

Node.js中异步执行的任务：
1. 文件I/O
2. 网络I/O

# Node模块化开发
#### 传统非模块化开发有如下的缺点
1. 命名冲突
2. 文件依赖

#### 前端标准的模块化规范
1. AMD - requirejs
2. CMD - seajs

#### 服务器端的模块化规范
CommonJS - Node.js

#### Node模块化相关的规则
1. 如何定义模块：一个js文件就是一个模块，模块内部的成员都是相互独立
2. 模块成员的导出和引入:
exports与module的关系：module.exports = exports = {};
模块成员的导出最终以module.exports为准
如果要导出单个的成员或者比较少的成员，一般我们使用exports导出；如果要导出的成员比较多，一般我们使用module.exports的方式;这两种方式不能同时使用  
```
var sum = function(a,b){
    return parseInt(a) + parseInt(b);
}
// 方法1
// 导出模块成员
exports.sum = sum;
//引入模块
var module = require('./xx.js');
var ret = module.sum(12,13);

// 方法2
// 导出模块成员
module.exports = sum;
//引入模块
var module = require('./xx.js');
module();

// // 方法1
// exports.sum = sum;
// exports.subtract = subtract;
// 
// var m = require('./05.js');
// var ret = m.sum(1,2);
// var ret1 = m.subtract(1,2);
// console.log(ret,ret1);
// 
// // 方法2
// module.exports = {
//     sum : sum,
//     subtract : subtract,
//     multiply : multiply,
//     divide : divide
// }
// 
// var m = require('./05.js');
// console.log(m);
```

