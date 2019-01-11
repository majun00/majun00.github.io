---
title: ES6学习--异步编程--Generator
tags: 
- ES6
- Generator
categories:
- ES6
---

##异步编程 :
(异步编程的语法目的就是让异步编程更像同步编程)

1. 回调函数
利用回调函数实现异步编程本身没有问题, 问题在于回调函数的嵌套过多, 就会进入可怕的回调地狱. 

2. 事件监听

3. 发布订阅

4. Promise
是一种新的写法,允许将回调函数的横向加载,改成纵向加载, 但是代码过长也会看起来语义不清晰
> 其实我觉得promise挺好了...大概是我太年轻

## ES6异步编程:
协程: 多个线程互相协作，完成异步任务
第一步，协程A开始执行。
第二步，协程A执行到一半，进入暂停，执行权转移到协程B。
第三步，（一段时间后）协程B交还执行权。
第四步，协程A恢复执行。

#### Generator函数
Generator 函数就是协程在 ES6 的实现，最大特点就是可以交出函数的执行权（即暂停执行）。

> 整个 Generator 函数就是一个封装的异步任务，或者说是异步任务的容器。函数名之前要加星号, 异步操作需要暂停的地方，都用 yield 语句注明。
```
function* gen(x){
  var y = yield x + 2;
  return y;
}
```

Generator函数的执行 :
```
var g = gen(1);
g.next() // { value: 3, done: false }
g.next() // { value: undefined, done: true }
```
上面代码中，调用 Generator 函数，会返回一个内部指针g(即遍历器) 。这是 Generator 函数不同于普通函数的另一个地方，即执行它不会返回结果，返回的是指针对象。调用指针 g 的 next 方法，会移动内部指针（即执行异步任务的第一段），指向第一个遇到的 yield 语句，上例是执行到 x + 2 为止。换言之，next 方法的作用是分阶段执行 Generator 函数。每次调用 next 方法，会返回一个对象，表示当前阶段的信息（ value 属性和 done 属性）。value 属性是 yield 语句后面表达式的值，表示当前阶段的值；done 属性是一个布尔值，表示 Generator 函数是否执行完毕，即是否还有下一个阶段。

#### Generator 函数的数据交换和错误处理
Generator 函数可以暂停执行和恢复执行，这是它能封装异步任务的根本原因。它还有两个特性，使它可以作为异步编程的完整解决方案：函数体内外的数据交换和错误处理机制。

> next 方法返回值的 value 属性，是 Generator 函数向外输出数据；next 方法还可以接受参数，这是向 Generator 函数体内输入数据。

Generator 函数内部还可以部署错误处理代码，捕获函数体外抛出的错误。
```
function* gen(x){
  try {
    var y = yield x + 2;
  } catch (e){ 
    console.log(e);
  }
  return y;
}

var g = gen(1);
g.next();
g.throw（'出错了'）;
// 出错了
```
上面代码的最后一行，Generator 函数体外，使用指针对象的 throw 方法抛出的错误，可以被函数体内的 try ... catch 代码块捕获。这意味着，出错的代码与处理错误的代码，实现了时间和空间上的分离。

####Generator 函数的用法
```
var fetch = require('node-fetch');

function* gen(){
  var url = 'https://api.github.com/users/github';
  var result = yield fetch(url);
  console.log(result.bio);
}
```
上面代码中，Generator 函数封装了一个异步操作，该操作先读取一个远程接口，然后从 JSON 格式的数据解析信息。就像前面说过的，这段代码非常像同步操作，除了加上了 yield 命令。
```
var g = gen();
var result = g.next();

result.value.then(function(data){
  return data.json();
}).then(function(data){
  g.next(data);
});
```
首先执行 Generator 函数，获取遍历器对象，然后使用 next 方法，执行异步任务的第一阶段。由于Fetch 模块返回的是一个 Promise 对象，因此要用 then 方法调用下一个next 方法。

> 参考自http://www.ruanyifeng.com/blog
















