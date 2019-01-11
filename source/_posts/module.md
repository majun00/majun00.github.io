---
title: 前端模块化开发
tags: 
- 前端模块化开发
categories:
- 前端模块化开发
---

## 现在的前端开发
现在的前端开发, 不仅仅是完成浏览的基本需求，并且通常是一个单页面应用，每一个视图通过异步的方式加载，这导致页面初始化和使用过程中会加载越来越多的 JavaScript 代码. 如何在开发环境组织好这些碎片化的代码和资源，并且保证他们在浏览器端快速、优雅的加载和更新，就需要一个模块化系统

## 什么是模块化
把函数作为模块
缺陷: 污染全局变量 模块成员之间没什么关系
面向对象思想 并使用立即执行函数 实现闭包
避免了变量污染 同时同一模块内的成员也有了关系 在模块外部无法修改我们没有暴露出来的变量、函数
这就是简单的模块

## 期望的模块系统
模块的加载和传输，我们首先能想到两种极端的方式，一种是每个模块文件都单独请求，另一种是把所有模块打包成一个文件然后只请求一次。显而易见，每个模块都发起单独的请求造成了请求次数过多，导致应用启动速度慢；一次请求加载所有模块导致流量浪费、初始化过程慢。这两种方式都不是好的解决方案，它们过于简单粗暴。

分块传输，按需进行懒加载，在实际用到某些模块的时候再增量更新，才是较为合理的模块加载方案。要实现模块的按需加载，就需要一个对整个代码库中的模块进行静态分析、编译打包的过程。

在上面的分析过程中，我们提到的模块仅仅是指JavaScript模块文件。然而，在前端开发过程中还涉及到样式、图片、字体、HTML 模板等等众多的资源。如果他们都可以视作模块，并且都可以通过`require`的方式来加载，将带来优雅的开发体验，那么如何做到让 `require` 能加载各种资源呢？在编译的时候，要对整个代码进行静态分析，分析出各个模块的类型和它们依赖关系，然后将不同类型的模块提交给适配的加载器来处理。Webpack 就是在这样的需求中应运而生。

## 模块系统
#### script
*   全局作用域下容易造成变量冲突
*   文件只能按照 `<script>` 的书写顺序进行加载
*   开发人员必须主观解决模块和代码库的依赖关系
*   在大型项目中各种资源难以管理，长期积累的问题导致代码库混乱不堪

#### CommonJS
服务器端的 Node.js 遵循 [CommonJS规范](http://wiki.commonjs.org/wiki/CommonJS)，该规范的核心思想是允许模块通过 `require` 方法来同步加载所要依赖的其他模块，然后通过 `exports` 或 `module.exports` 来导出需要暴露的接口。
```
require("module");
require("../file.js");
exports.doStuff = function() {};
module.exports = someValue;
```
或
```
// moduleA.js
module.exports = function( value ){
    return value * 2;
}
```
```
// moduleB.js
var multiplyBy2 = require('./moduleA');
var result = multiplyBy2(4);
```

优点：
*   服务器端模块便于重用
*   [NPM](https://www.npmjs.com/) 中已经有将近20万个可以使用模块包
*   简单并容易使用

缺点：
*   同步的模块加载方式不适合在浏览器环境中，同步意味着阻塞加载，浏览器资源是异步加载的
*   不能非阻塞的并行加载多个模块

#### AMD
`define(id?, dependencies?, factory)`，它要在声明模块的时候指定所有的依赖 `dependencies`，并且还要当做形参传到 `factory` 中，对于依赖的模块提前执行，依赖前置。
```
define("module", ["dep1", "dep2"], function(d1, d2) {
  return someExportedValue;
});
require(["module", "../file"], function(module, file) { /* ... */ });
```
一些用例：
定义一个名为 `myModule` 的模块，它依赖 `jQuery` 模块：
```
define('myModule', ['jquery'], function($) {
    // $ 是 jquery 模块的输出
    $('body').text('hello world');
});
// 使用
define(['myModule'], function(myModule) {});
```
注意：在 webpack 中，模块名只有局部作用域，在 Require.js 中模块名是全局作用域，可以在全局引用。
定义一个没有 `id` 值的匿名模块，通常作为应用的启动函数：
```
define(['jquery'], function($) {
    $('body').text('hello world');
});
```
依赖多个模块的定义：
```
define(['jquery', './math.js'], function($, math) {
    // $ 和 math 一次传入 factory
    $('body').text('hello world');
});
```
模块输出：
```
define(['jquery'], function($) {

    var HelloWorldize = function(selector){
        $(selector).text('hello world');
    };

    // HelloWorldize 是该模块输出的对外接口
    return HelloWorldize;
});
```
在模块定义内部引用依赖：
```
define(function(require) {
    var $ = require('jquery');
    $('body').text('hello world');
});
```

优点：
*   适合在浏览器环境中异步加载模块
*   可以并行加载多个模块

缺点：
*   提高了开发成本，代码的阅读和书写比较困难，模块定义方式的语义不顺畅
*   不符合通用的模块化思维方式，是一种妥协的实现

4.CMD
```
define(function(require, exports, module) {
  var $ = require('jquery');
  var Spinning = require('./spinning');
  exports.doSomething = ...
  module.exports = ...
})
```

优点：
*   依赖就近，延迟执行
*   可以很容易在 Node.js 中运行

缺点：
*   依赖 SPM 打包，模块的加载逻辑偏重

5.UMD

6.ES6模块

[ES6 模块](http://es6.ruanyifeng.com/#docs/module)的设计思想，是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。
```
import "jquery";
export function doStuff() {}
module "localModule" {}
```

优点：
*   容易进行静态分析
*   面向未来的 EcmaScript 标准

缺点：
*   原生浏览器端还没有实现该标准
*   全新的命令字，新版的 Node.js才支持

实现：
*   [Babel](https://babeljs.io/)

## AMD、CMD、webpack的区别：
从前有两个规范，一个是AMD 一个是CMD 
RequireJS是AMD规范的实现，SeaJS是CMD规范的实现， 
一个主张提前加载依赖，一个主张延迟加载依赖
后来出现了 commonjs规范 
webpack就是支持commonjs规范的 
目前可以说是主导了前端构建格局。

CommomJS是服务端规范，node就是采用这个规范，他是同步加载，毕竟服务端不用考虑异步。 
它是对通用的JavaScript模式的标准化尝试，它包含有 AMD 定义 
AMD是异步加载模块的缩写，使用require引入模块，提倡依賴前置。 
CMD与AMD其实挺接近的，还因为有sea.js,中文资料还是比较亲切的。 
还有就是AMD和CommomJS的中间者UMD 
Webpack其实就是一个打包工具，他的思想就是一切皆模块，css是模块，js是模块，图片是模块。并且提供了一些列模块加载(各种-loader)来编译模块。官方推荐使用commonJS规范，但是也支持CMD和AMD

无论是`node`应用模块，还是`webpack` 配置 ，均是采用`CommonJS`模块化规范


## CommonJS 与 AMD 支持
Webpack 对 CommonJS 的 AMD 的语法做了兼容, 方便迁移代码

不过实际上, 引用模块的规则是依据 CommonJS 来的

```
require('lodash') // 从模块目录查找
require('./file') // 按相对路径查找
```

AMD 语法中, 也要注意, 是按 CommonJS 的方案查找的

```
define (require, exports. module) ->
  require('lodash') # commonjs 当中这样是查找模块的 

```

## CommonJs与AMD
> 在一开始，我们先讲一下它和以往我们所用的模块管理工具有什么不一样。在最开始的阶段，Js并没有这些模块机制，各种Js到处飞，得不到有效妥善的管理。后来前端圈开始制定规范，最耳熟能详的是CommonJs和AMD。

> CommonJs是应用在NodeJs，是一种同步的模块机制。它的写法大致如下：
```
var firstModule = require("firstModule");
//your code...
module.export = anotherModule
```

> AMD的应用场景则是浏览器，异步加载的模块机制。require.js的写法大致如下：
```
define(['firstModule'], function(module){
   //your code...
   return anotherModule
})
```

> 其实我们单比较写法，就知道CommonJs是更为优秀的。它是一种同步的写法，对Human友好，而且代码也不会繁琐臃肿。但更重要的原因是，随着npm成为主流的JavaScript组件发布平台，越来越多的前端项目也依赖于npm上的项目，或者自身就会发布到npm平台。所以我们对如何可以使用npm包中的模块是我们的一大需求。所以browserify工具就出现了，它支持我们直接使用require()的同步语法去加载npm模块。

> 当然我们这里不得不说的是，ES2015（ES6）里也有了自己的模块机制，也就是说ES6的模块机制是官方规定的，我们通过babel（一种6to5的编译器）可以使用比较多的新特性了，包括我们提到的模块机制，而它的写法大致如下：

```
import {someModule} from "someModule";
// your codes...
export anotherModule;
```

> 当然上面的写法只是最基本的，还有其他的不同加载模块的写法，可以看一下阮一峰老师的ECMAScript 6 入门或者babel的相关文档Learn ES2015。

## 功能特性
> browserify的出现非常棒，但webpack更胜一筹！

> 我们来看看webpack支持哪些功能特性：
*   支持CommonJs和AMD模块，意思也就是我们基本可以无痛迁移旧项目。
*   支持模块加载器和插件机制，可对模块灵活定制。特别是我最爱的babel-loader，有效支持ES6。
*   可以通过配置，打包成多个文件。有效利用浏览器的缓存功能提升性能。
*   将样式文件和图片等静态资源也可视为模块进行打包。配合loader加载器，可以支持sass，less等CSS预处理器。
*   内置有source map，即使打包在一起依旧方便调试。
*   看完上面这些，可以想象它就是一个前端工具，可以让我们进行各种模块加载，预处理后，再打包。之前我们对这些的处理是放在grunt或gulp等前端自动化工具中。有了webpack，我们无需借助自动化工具对模块进行各种处理，让我们工具的任务分的更加清晰。

## 回顾ES6模块
#### 对象的导出
```
1. export default{
        add(){}
 }
2. export fucntion add(){} 相当于 将add方法当做一个属性挂在到exports对象
```
#### 对象的导入
```
如果导出的是：export default{ add(){}}
那么可以通过  import obj from './calc.js'
如果导出的是：
export fucntion add(){} 
export fucntion substrict(){} 
export const PI=3.14
那么可以通过按需加载 import {add,substrict,PI} from './calc.js'
```

## 回顾Node模块
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

##回顾webpack
#### 模块打包器
根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。如何在一个大规模的代码库中，维护各种模块资源的分割和存放，维护它们之间的依赖关系，并且无缝的将它们整合到一起生成适合浏览器端请求加载的静态资源。市面上已经存在的模块管理和打包工具并不适合大型的项目，尤其单页面 Web 应用程序。最紧迫的原因是如何在一个大规模的代码库中，维护各种模块资源的分割和存放，维护它们之间的依赖关系，并且无缝的将它们整合到一起生成适合浏览器端请求加载的静态资源。
这些已有的模块化工具并不能很好的完成如下的目标：
*   将依赖树拆分成按需加载的块
*   初始化加载的耗时尽量少
*   各种静态资源都可以视作模块
*   将第三方库整合成模块的能力
*   可以自定义打包逻辑的能力
*   适合大项目，无论是单页还是多页的 Web 应用

#### Webpack 的特点
Webapck 和其他模块化工具有什么区别呢？
1. 代码拆分
Webpack 有两种组织模块依赖的方式，同步和异步。异步依赖作为分割点，形成一个新的块。在优化了依赖树后，每一个异步区块都作为一个文件被打包。
2. Loader
Webpack 本身只能处理原生的 JavaScript 模块，但是 loader 转换器可以将各种类型的资源转换成 JavaScript 模块。这样，任何资源都可以成为 Webpack 可以处理的模块。
3. 智能解析
Webpack 有一个智能解析器，几乎可以处理任何第三方库，无论它们的模块形式是 CommonJS、 AMD 还是普通的 JS 文件。甚至在加载依赖的时候，允许使用动态表达式 `require("./templates/" + name + ".jade")`。
4. 插件系统
Webpack 还有一个功能丰富的插件系统。大多数内容功能都是基于这个插件系统运行的，还可以开发和使用开源的 Webpack 插件，来满足各式各样的需求。
5. 快速运行
Webpack 使用异步 I/O 和多级缓存提高运行效率，这使得 Webpack 能够以令人难以置信的速度快速增量编译。

#### webpack是什么？
CommonJS和AMD是用于JavaScript模块管理的两大规范，前者定义的是模块的同步加载，主要用于NodeJS；而后者则是异步加载，通过requirejs等工具适用于前端。随着npm成为主流的JavaScript组件发布平台，越来越多的前端项目也依赖于npm上的项目，或者 自身就会发布到npm平台。因此，让前端项目更方便的使用npm上的资源成为一大需求。
web开发中常用到的静态资源主要有JavaScript、CSS、图片、Jade等文件，webpack中将静态资源文件称之为模块。 webpack是一个module bundler(模块打包工具)，其可以兼容多种js书写规范，且可以处理模块间的依赖关系，具有更强大的js模块化的功能。Webpack对它们进行统 一的管理以及打包发布

#### 为什么使用 webpack？
1\. 对 CommonJS 、 AMD 、ES6的语法做了兼容
2\. 对js、css、图片等资源文件都支持打包
3\. 串联式模块加载器以及插件机制，让其具有更好的灵活性和扩展性，例如提供对CoffeeScript、ES6的支持
4\. 有独立的配置文件webpack.config.js
5\. 可以将代码切割成不同的chunk，实现按需加载，降低了初始化时间
6\. 支持 SourceUrls 和 SourceMaps，易于调试
7\. 具有强大的Plugin接口，大多是内部插件，使用起来比较灵活
8.webpack 使用异步 IO 并具有多级缓存。这使得 webpack 很快且在增量编译上更加快
