---
title: Node框架学习(2)--Koa
tags: 
- NodeJS
- Koa
categories:
- NodeJS
---

## 一、基本用法

### 1.1 架设 HTTP 服务

> ```
> 
> // demos/01.js
> const Koa = require('koa');
> const app = new Koa();
> 
> app.listen(3000);
> ```

> ```
> 
> $ node demos/01.js
> 
> ```

打开浏览器，访问 http://127.0.0.1:3000 。你会看到页面显示"Not Found"，表示没有发现任何内容。这是因为我们并没有告诉 Koa 应该显示什么内容。

### 1.2 Context 对象

Koa 提供一个 Context 对象，表示一次对话的上下文（包括 HTTP 请求和 HTTP 回复）。通过加工这个对象，就可以控制返回给用户的内容。

`Context.response.body`属性就是发送给用户的内容。

> ```
> 
> // demos/02.js
> const Koa = require('koa');
> const app = new Koa();
> 
> const main = ctx => {
>   ctx.response.body = 'Hello World';
> };
> 
> app.use(main);
> app.listen(3000);
> 
> ```

上面代码中，`main`函数用来设置`ctx.response.body`。然后，使用`app.use`方法加载`main`函数。

`ctx.response`代表 HTTP Response。同样地，`ctx.request`代表 HTTP Request。

> ```
> 
> $ node demos/02.js
> 
> ```

访问 http://127.0.0.1:3000 ，现在就可以看到"Hello World"了。

### 1.3 HTTP Response 的类型

Koa 默认的返回类型是`text/plain`，如果想返回其他类型的内容，可以先用`ctx.request.accepts`判断一下，客户端希望接受什么数据（根据 HTTP Request 的`Accept`字段），然后使用`ctx.response.type`指定返回类型。

> ```
> 
> // demos/03.js
> const main = ctx => {
>   if (ctx.request.accepts('xml')) {
>     ctx.response.type = 'xml';
>     ctx.response.body = '<data>Hello World</data>';
>   } else if (ctx.request.accepts('json')) {
>     ctx.response.type = 'json';
>     ctx.response.body = { data: 'Hello World' };
>   } else if (ctx.request.accepts('html')) {
>     ctx.response.type = 'html';
>     ctx.response.body = '<p>Hello World</p>';
>   } else {
>     ctx.response.type = 'text';
>     ctx.response.body = 'Hello World';
>   }
> };
> ```

> ```
> 
> $ node demos/03.js
> 
> ```

访问 http://127.0.0.1:3000 ，现在看到的就是一个 XML 文档了。

[图片上传失败...(image-14a658-1512521964504)]

### 1.4 网页模板

实际开发中，返回给用户的网页往往都写成模板文件。我们可以让 Koa 先读取模板文件，然后将这个模板返回给用户。

> ```
> 
> // demos/04.js
> const fs = require('fs');
> 
> const main = ctx => {
>   ctx.response.type = 'html';
>   ctx.response.body = fs.createReadStream('./demos/template.html');
> };
> ```

> ```
> 
> $ node demos/04.js
> 
> ```

访问 http://127.0.0.1:3000 ，看到的就是[模板文件](https://github.com/ruanyf/koa-demos/blob/master/demos/template.html)的内容了。

## 二、路由

### 2.1 原生路由

网站一般都有多个页面。通过`ctx.request.path`可以获取用户请求的路径，由此实现简单的路由。

> ```
> 
> // demos/05.js
> const main = ctx => {
>   if (ctx.request.path !== '/') {
>     ctx.response.type = 'html';
>     ctx.response.body = '<a href="/">Index Page</a>';
>   } else {
>     ctx.response.body = 'Hello World';
>   }
> };
> ```

> ```
> 
> $ node demos/05.js
> 
> ```

访问 http://127.0.0.1:3000/about ，可以看到一个链接，点击后就跳到首页。

### 2.2 koa-route 模块

原生路由用起来不太方便，我们可以使用封装好的[`koa-route`](https://www.npmjs.com/package/koa-route)模块。

> ```
> 
> // demos/06.js
> const route = require('koa-route');
> 
> const about = ctx => {
>   ctx.response.type = 'html';
>   ctx.response.body = '<a href="/">Index Page</a>';
> };
> 
> const main = ctx => {
>   ctx.response.body = 'Hello World';
> };
> 
> app.use(route.get('/', main));
> app.use(route.get('/about', about));
> 
> ```

上面代码中，根路径`/`的处理函数是`main`，`/about`路径的处理函数是`about`。

> ```
> 
> $ node demos/06.js
> 
> ```

访问 http://127.0.0.1:3000/about 

### 2.3 静态资源

如果网站提供静态资源（图片、字体、样式表、脚本......），为它们一个个写路由就很麻烦，也没必要。[`koa-static`](https://www.npmjs.com/package/koa-static)模块封装了这部分的请求。

> ```
> 
> // demos/12.js
> const path = require('path');
> const serve = require('koa-static');
> 
> const main = serve(path.join(__dirname));
> app.use(main);
> ```

> ```
> 
> $ node demos/12.js
> 
> ```

访问 http://127.0.0.1:3000/12.js，在浏览器里就可以看到这个脚本的内容。

### 2.4 重定向

有些场合，服务器需要重定向（redirect）访问请求。比如，用户登陆以后，将他重定向到登陆前的页面。`ctx.response.redirect()`方法可以发出一个302跳转，将用户导向另一个路由。

> ```
> 
> // demos/13.js
> const redirect = ctx => {
>   ctx.response.redirect('/');
>   ctx.response.body = '<a href="/">Index Page</a>';
> };
> 
> app.use(route.get('/redirect', redirect));
> ```

> ```
> 
> $ node demos/13.js
> 
> ```

访问 http://127.0.0.1:3000/redirect ，浏览器会将用户导向根路由。

## 三、中间件

### 3.1 Logger 功能

Koa 的最大特色，也是最重要的一个设计，就是中间件（middleware）。为了理解中间件，我们先看一下 Logger （打印日志）功能的实现。

最简单的写法就是在`main`函数里面增加一行。

> ```
> 
> // demos/07.js
> const main = ctx => {
>   console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
>   ctx.response.body = 'Hello World';
> };
> ```

> ```
> 
> $ node demos/07.js
> 
> ```

访问 http://127.0.0.1:3000 ，命令行就会输出日志。

> ```
> 
> 1502144902843 GET /
> 
> ```

### 3.2 中间件的概念

上一个例子里面的 Logger 功能，可以拆分成一个独立函数。

> ```
> 
> // demos/08.js
> const logger = (ctx, next) => {
>   console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
>   next();
> }
> app.use(logger);
> 
> ```

像上面代码中的`logger`函数就叫做"中间件"（middleware），因为它处在 HTTP Request 和 HTTP Response 中间，用来实现某种中间功能。`app.use()`用来加载中间件。

基本上，Koa 所有的功能都是通过中间件实现的，前面例子里面的`main`也是中间件。每个中间件默认接受两个参数，第一个参数是 Context 对象，第二个参数是`next`函数。只要调用`next`函数，就可以把执行权转交给下一个中间件。

> ```
> 
> $ node demos/08.js
> ```

### 3.3 中间件栈

多个中间件会形成一个栈结构（middle stack），以"先进后出"（first-in-last-out）的顺序执行。

> 1.  最外层的中间件首先执行。
> 2.  调用`next`函数，把执行权交给下一个中间件。
> 3.  ...
> 4.  最内层的中间件最后执行。
> 5.  执行结束后，把执行权交回上一层的中间件。
> 6.  ...
> 7.  最外层的中间件收回执行权之后，执行`next`函数后面的代码。

> ```
> 
> // demos/09.js
> const one = (ctx, next) => {
>   console.log('>> one');
>   next();
>   console.log('<< one');
> }
> 
> const two = (ctx, next) => {
>   console.log('>> two');
>   next(); 
>   console.log('<< two');
> }
> 
> const three = (ctx, next) => {
>   console.log('>> three');
>   next();
>   console.log('<< three');
> }
> 
> app.use(one);
> app.use(two);
> app.use(three);
> 
> ```

> ```
> 
> $ node demos/09.js
> 
> ```

访问 http://127.0.0.1:3000 ，命令行窗口会有如下输出。

> ```
> 
> >> one
> >> two
> >> three
> << three
> << two
> << one
> 
> ```

如果中间件内部没有调用`next`函数，那么执行权就不会传递下去。作为练习，你可以将`two`函数里面`next()`这一行注释掉再执行，看看会有什么结果。

### 3.4 异步中间件

迄今为止，所有例子的中间件都是同步的，不包含异步操作。如果有异步操作（比如读取数据库），中间件就必须写成 [async 函数](http://es6.ruanyifeng.com/#docs/async)。

> ```
> 
> // demos/10.js
> const fs = require('fs.promised');
> const Koa = require('koa');
> const app = new Koa();
> 
> const main = async function (ctx, next) {
>   ctx.response.type = 'html';
>   ctx.response.body = await fs.readFile('./demos/template.html', 'utf8');
> };
> 
> app.use(main);
> app.listen(3000);
> 
> ```

上面代码中，`fs.readFile`是一个异步操作，必须写成`await fs.readFile()`，然后中间件必须写成 async 函数。

> ```
> 
> $ node demos/10.js
> 
> ```

访问 http://127.0.0.1:3000 ，就可以看到模板文件的内容。

### 3.5 中间件的合成

[`koa-compose`](https://www.npmjs.com/package/koa-compose)模块可以将多个中间件合成为一个。

> ```
> 
> // demos/11.js
> const compose = require('koa-compose');
> 
> const logger = (ctx, next) => {
>   console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
>   next();
> }
> 
> const main = ctx => {
>   ctx.response.body = 'Hello World';
> };
> 
> const middlewares = compose([logger, main]);
> app.use(middlewares);
> ```

> ```
> 
> $ node demos/11.js
> 
> ```

访问 http://127.0.0.1:3000 ，就可以在命令行窗口看到日志信息。

## 四、错误处理

### 4.1 500 错误

如果代码运行过程中发生错误，我们需要把错误信息返回给用户。HTTP 协定约定这时要返回500状态码。Koa 提供了`ctx.throw()`方法，用来抛出错误，`ctx.throw(500)`就是抛出500错误。

> ```
> 
> // demos/14.js
> const main = ctx => {
>   ctx.throw(500);
> };
> ```

> ```
> 
> $ node demos/14.js
> 
> ```

访问 http://127.0.0.1:3000，你会看到一个500错误页"Internal Server Error"。

4.2 404错误

如果将`ctx.response.status`设置成404，就相当于`ctx.throw(404)`，返回404错误。

> ```
> 
> // demos/15.js
> const main = ctx => {
>   ctx.response.status = 404;
>   ctx.response.body = 'Page Not Found';
> };
> ```

> ```
> 
> $ node demos/15.js
> 
> ```

访问 http://127.0.0.1:3000 ，你就看到一个404页面"Page Not Found"。

### 4.3 处理错误的中间件

为了方便处理错误，最好使用`try...catch`将其捕获。但是，为每个中间件都写`try...catch`太麻烦，我们可以让最外层的中间件，负责所有中间件的错误处理。

> ```
> 
> // demos/16.js
> const handler = async (ctx, next) => {
>   try {
>     await next();
>   } catch (err) {
>     ctx.response.status = err.statusCode || err.status || 500;
>     ctx.response.body = {
>       message: err.message
>     };
>   }
> };
> 
> const main = ctx => {
>   ctx.throw(500);
> };
> 
> app.use(handler);
> app.use(main);
> ```

> ```
> 
> $ node demos/16.js
> 
> ```

访问 http://127.0.0.1:3000 ，你会看到一个500页，里面有报错提示 `{"message":"Internal Server Error"}`。

### 4.4 error 事件的监听

运行过程中一旦出错，Koa 会触发一个`error`事件。监听这个事件，也可以处理错误。

> ```
> 
> // demos/17.js
> const main = ctx => {
>   ctx.throw(500);
> };
> 
> app.on('error', (err, ctx) =>
>   console.error('server error', err);
> );
> ```

> ```
> 
> $ node demos/17.js
> 
> ```

访问 http://127.0.0.1:3000 ，你会在命令行窗口看到"server error xxx"。

### 4.5 释放 error 事件

需要注意的是，如果错误被`try...catch`捕获，就不会触发`error`事件。这时，必须调用`ctx.app.emit()`，手动释放`error`事件，才能让监听函数生效。

> ```
> 
> // demos/18.js`
> const handler = async (ctx, next) => {
>   try {
>     await next();
>   } catch (err) {
>     ctx.response.status = err.statusCode || err.status || 500;
>     ctx.response.type = 'html';
>     ctx.response.body = '<p>Something wrong, please contact administrator.</p>';
>     ctx.app.emit('error', err, ctx);
>   }
> };
> 
> const main = ctx => {
>   ctx.throw(500);
> };
> 
> app.on('error', function(err) {
>   console.log('logging error ', err.message);
>   console.log(err);
> });
> 
> ```

上面代码中，`main`函数抛出错误，被`handler`函数捕获。`catch`代码块里面使用`ctx.app.emit()`手动释放`error`事件，才能让监听函数监听到。

> ```
> 
> $ node demos/18.js
> 
> ```

访问 http://127.0.0.1:3000 ，你会在命令行窗口看到`logging error`。

## 五、Web App 的功能

### 5.1 Cookies

`ctx.cookies`用来读写 Cookie。

> ```
> 
> // demos/19.js
> const main = function(ctx) {
>   const n = Number(ctx.cookies.get('view') || 0) + 1;
>   ctx.cookies.set('view', n);
>   ctx.response.body = n + ' views';
> }
> ```

> ```
> 
> $ node demos/19.js
> 
> ```

访问 http://127.0.0.1:3000 ，你会看到`1 views`。刷新一次页面，就变成了`2 views`。再刷新，每次都会计数增加1。

### 5.2 表单

Web 应用离不开处理表单。本质上，表单就是 POST 方法发送到服务器的键值对。[`koa-body`](https://www.npmjs.com/package/koa-body)模块可以用来从 POST 请求的数据体里面提取键值对。

> ```
> 
> // demos/20.js
> const koaBody = require('koa-body');
> 
> const main = async function(ctx) {
>   const body = ctx.request.body;
>   if (!body.name) ctx.throw(400, '.name required');
>   ctx.body = { name: body.name };
> };
> 
> app.use(koaBody());
> ```

> ```
> 
> $ node demos/20.js
> 
> ```

打开另一个命令行窗口，运行下面的命令。

> ```
> 
> $ curl -X POST --data "name=Jack" 127.0.0.1:3000
> {"name":"Jack"}
> 
> $ curl -X POST --data "name" 127.0.0.1:3000
> name required
> 
> ```

上面代码使用 POST 方法向服务器发送一个键值对，会被正确解析。如果发送的数据不正确，就会收到错误提示。

### 5.3 文件上传

[`koa-body`](https://www.npmjs.com/package/koa-body)模块还可以用来处理文件上传。

> ```
> 
> // demos/21.js
> const os = require('os');
> const path = require('path');
> const koaBody = require('koa-body');
> 
> const main = async function(ctx) {
>   const tmpdir = os.tmpdir();
>   const filePaths = [];
>   const files = ctx.request.body.files || {};
> 
>   for (let key in files) {
>     const file = files[key];
>     const filePath = path.join(tmpdir, file.name);
>     const reader = fs.createReadStream(file.path);
>     const writer = fs.createWriteStream(filePath);
>     reader.pipe(writer);
>     filePaths.push(filePath);
>   }
> 
>   ctx.body = filePaths;
> };
> 
> app.use(koaBody({ multipart: true }));
> ```

> ```
> 
> $ node demos/21.js
> 
> ```

打开另一个命令行窗口，运行下面的命令，上传一个文件。注意，`/path/to/file`要更换为真实的文件路径。

> ```
> 
> $ curl --form upload=@/path/to/file http://127.0.0.1:3000
> ["/tmp/file"]
> ```

> 参考自 http://www.ruanyifeng.com/blog/2017/08/koa.html
[示例库](https://github.com/ruanyf/koa-demos)  
[zip 文件](https://github.com/ruanyf/koa-demos/archive/master.zip) 
[demos](https://github.com/ruanyf/koa-demos/tree/master/demos)
