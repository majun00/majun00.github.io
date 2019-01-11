---
title: Node学习(2)
tags: 
- NodeJS
categories:
- NodeJS
---

## 脚本模式
`console.log("Hello World");`
保存该文件，文件名为 helloworld.js， 并通过 node命令来执行：`node helloworld.js`, 程序执行后，正常的话，就会在终端输出 Hello World。

## 交互模式
打开终端，键入node进入命令交互模式，可以输入一条代码语句后立即执行并显示结果，例如：
```
$ node
> console.log('Hello World!');
Hello World!
```

## 创建 Node.js 应用
Node.js 应用是由哪几部分组成的：
1. 引入 required 模块：我们可以使用 require 指令来载入 Node.js 模块。
2. 创建服务器：服务器可以监听客户端的请求，类似于 Apache 、Nginx 等 HTTP 服务器。
3. 接收请求与响应请求 服务器很容易创建，客户端可以使用浏览器或终端发送 HTTP 请求，服务器接收请求后返回响应数据。

步骤一、引入 required 模块
我们使用 require 指令来载入 http 模块，并将实例化的 HTTP 赋值给变量 http，实例如下:
`var http = require("http");`

步骤二、创建服务器
接下来我们使用 http.createServer() 方法创建服务器，并使用 listen 方法绑定 8888 端口。 函数通过 request, response 参数来接收和响应数据。
实例如下，在你项目的根目录下创建一个叫 server.js 的文件，并写入以下代码：
```
var http = require('http');
http.createServer(function (request, response) {
    // 发送 HTTP 头部 
    // HTTP 状态值: 200 : OK
    // 内容类型: text/plain
    response.writeHead(200, {'Content-Type': 'text/plain'});
    // 发送响应数据 "Hello World"
    response.end('Hello World\n');
}).listen(8888);
// 终端打印如下信息
console.log('Server running at http://127.0.0.1:8888/');
```
以上代码我们完成了一个可以工作的 HTTP 服务器。
`node server.js`

分析Node.js 的 HTTP 服务器：
第一行请求（require）Node.js 自带的 http 模块，并且把它赋值给 http 变量。
接下来我们调用 http 模块提供的函数： createServer 。这个函数会返回 一个对象，这个对象有一个叫做 listen 的方法，这个方法有一个数值参数， 指定这个 HTTP 服务器监听的端口号。

## Node.js REPL(交互式解释器)
表示一个电脑的环境，可以输入命令，并接收系统的响应。
```
$ node
> 
```
注意: 
1. 简单的表达式运算
2. 使用变量
3. 多行表达式
4. 下划线(_)变量
5. 可以使用下划线(_)获取表达式的运算结果：
6. REPL 命令
`ctrl + c` - 退出当前终端。
`ctrl + c 按下两次` - 退出 Node REPL。
`ctrl + d` - 退出 Node REPL.
`向上/向下 键` - 查看输入的历史命令
`tab 键` - 列出当前命令
`.help` - 列出使用命令
`.break` - 退出多行表达式
`.clear` - 退出多行表达式
`.save filename` - 保存当前的 Node REPL 会话到指定文件
`.load filename` - 载入当前 Node REPL 会话的文件内容。

> 参考自http://www.runoob.com/









