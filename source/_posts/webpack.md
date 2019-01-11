---
title: 学习Webpack
tags: 
- Webpack
categories:
- Webpack
---

# 模块打包器
根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。如何在一个大规模的代码库中，维护各种模块资源的分割和存放，维护它们之间的依赖关系，并且无缝的将它们整合到一起生成适合浏览器端请求加载的静态资源。市面上已经存在的模块管理和打包工具并不适合大型的项目，尤其单页面 Web 应用程序。最紧迫的原因是如何在一个大规模的代码库中，维护各种模块资源的分割和存放，维护它们之间的依赖关系，并且无缝的将它们整合到一起生成适合浏览器端请求加载的静态资源。
这些已有的模块化工具并不能很好的完成如下的目标：
*   将依赖树拆分成按需加载的块
*   初始化加载的耗时尽量少
*   各种静态资源都可以视作模块
*   将第三方库整合成模块的能力
*   可以自定义打包逻辑的能力
*   适合大项目，无论是单页还是多页的 Web 应用

# Webpack 的特点
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

# webpack是什么？
CommonJS和AMD是用于JavaScript模块管理的两大规范，前者定义的是模块的同步加载，主要用于NodeJS；而后者则是异步加载，通过requirejs等工具适用于前端。随着npm成为主流的JavaScript组件发布平台，越来越多的前端项目也依赖于npm上的项目，或者 自身就会发布到npm平台。因此，让前端项目更方便的使用npm上的资源成为一大需求。
web开发中常用到的静态资源主要有JavaScript、CSS、图片、Jade等文件，webpack中将静态资源文件称之为模块。 webpack是一个module bundler(模块打包工具)，其可以兼容多种js书写规范，且可以处理模块间的依赖关系，具有更强大的js模块化的功能。Webpack对它们进行统 一的管理以及打包发布

# 为什么使用 webpack？
1\. 对 CommonJS 、 AMD 、ES6的语法做了兼容
2\. 对js、css、图片等资源文件都支持打包
3\. 串联式模块加载器以及插件机制，让其具有更好的灵活性和扩展性，例如提供对CoffeeScript、ES6的支持
4\. 有独立的配置文件webpack.config.js
5\. 可以将代码切割成不同的chunk，实现按需加载，降低了初始化时间
6\. 支持 SourceUrls 和 SourceMaps，易于调试
7\. 具有强大的Plugin接口，大多是内部插件，使用起来比较灵活
8.webpack 使用异步 IO 并具有多级缓存。这使得 webpack 很快且在增量编译上更加快

# webpack使用引导
1.创建目录结构

2.引入webpack依赖
在项目根目录打开命令窗口引入项目依赖，全局安装
```
npm  install  webpack  -g // 全局安装webpack
```

3.创建配置文件
在项目根目录创建三个或多个webpack配置文件
（1）webpack.base.config.js  //公用的配置放在这里面（可通过插件继承）
（2）webpack.develop.config.js  //开发环境中用到的配置文件
（3）webpack.publish.config.js   //生产环境中用到的配置文件

4.修改配置文件
注意：开发环境的配置和生产环境的配置是不一样的
```
// 这是最基本的一个配置文件
// 编写配置文件，要有最基本的文件入口和输出文件配置信息等
// 里面还可以加loader和各种插件配置使用
var path = require('path');
module.exports = {
    entry:path.resolve(__dirname,'src/js/app.js'),
    output: {
        path: path.resolve(__dirname, 'deploy'),
        filename: 'bundle.js',
    },
}
```

5.运行webpack
a、通过配置文件运行（通用）
（1）在根目录运行webpack的命令，默认会去查找名称为webpack.config.js的文件执行，如果没有就会报配置信息没有配置的错误。
```
Webpack
```
（2）这时候我们可以通过运行下面的命令进行配置文件的选择
```
webpack –-config  webpack.develop.config.js
```
b、通过cli命令运行（了解）
（1）webpack的cli也提供了命令可以进行运行，例如：
```
Webpack  -watch       // webpack的监视命令，文件发生变化自动编译

webpack --entry ./entry.js --output-path dist --output-file bundle.js

  //配置文件的启动目录和输出目录
```
`webpack` 最基本的启动webpack命令
`webpack -w` 提供watch方法，实时进行打包更新
`webpack -p` 对打包后的文件进行压缩
`webpack -d` 提供SourceMaps，方便调试
`webpack --colors` 输出结果带彩色，比如：会用红色显示耗时较长的步骤
`webpack --profile` 输出性能数据，可以看到每一步的耗时
`webpack --display-modules` 默认情况下 node_modules 下的模块会被隐藏，加上这个参数可以显示这些被隐藏的模块
`webpack --display-error-details` 方便出错时能查阅更详尽的信息（比如 webpack 寻找模块的过程），从而更好定位到问题。
（2）你可以运行webpack  -h查看webpack的其他命令，自行了解，或者通过英文官网了解webpack Cli部分

c、作为nodejs的api运行（了解）
```
var webpack = require('webpack');
webpack({
//configuration
}, function(err, stats){});
```

d、注意：但是我们基本上不用这种直接提供的命令，因为我们需要手动的敲打很多字母，我们现在开发通用的方法都是使用配置文件的方式运行。

## 把运行命令配置到npm的script中
#### 为什么要用npm的script
1、npm 是一个非常好用的用来编译的指令，通过 npm 你可以不用去担心项目中使用了什么技术，你只要调用这个指令就可以了，只要你在 package.json 中设置 scripts 的值就可以了。
2、真正开发的时候你的webpack的命令会敲很长，因为有很多命令需要设置，即便你用了配置文件的方式，但一些--colors  --hot这些命令是没法在配置文件中配置的。比如：
```
webpack-dev-server --devtool eval --progress --colors --hot --content-base build
```
所以你不可能每次都敲这么长，因而我们就把这一大串配置到npm中

#### Npm的script的使用
1、你首先需要安装webpack，这时候不全局安装
```
npm i webpack --save
```
2、配置npm的package.json文件中
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "develop": "webpack --config webpack.develop.config.js",
    "publish": "webpack --config webpack.publish.config.js"
},
```
3、在根目录打开的命令窗口运行
```
npm  run  develop
```

## 为发布目录启动服务
如果需要一直输入 npm run develop 确实是一件非常无聊的事情，幸运的是，我们可以把让他安静的运行，让我们设置 webpack-dev-server
除了提供模块打包功能，Webpack还提供了一个基于Node.js Express框架的开发服务器，它是一个静态资源Web服务器，对于简单静态页面或者仅依赖于独立服务的前端页面，都可以直接使用这个开发服务器进行开 发。在开发过程中，开发服务器会监听每一个文件的变化，进行实时打包，并且可以推送通知前端页面代码发生了变化，从而可以实现页面的自动刷新。
1、安装webpack-dev-server
```
npm   i   webpack-dev-server   –save-dev
```

2、调整npm的package.json scripts 部分中开发命令的配置
```
{
 "scripts": {
 "develop": "webpack-dev-server  --config webpack.develop.config.js --devtool eval --progress --colors --hot --content-base src",
"publish": "webpack --config webpack.publish.config.js",
"watch": "webpack --config webpack.develop.config.js --watch --hot"
 }
}
```
注释：
`webpack-dev-server` - 在 localhost:8080 建立一个 Web 服务器
`--devtool eval` - 为你的代码创建源地址。当有任何报错的时候可以让你更加精确地定位到文件和行号
`--progress` - 显示合并代码进度
`--colors -- hot`，命令行中显示颜色！
`--content-base`  指向设置的输出目录//这点一定是我们的发布目录

3、访问 http://localhost:8080 你会看到效果。
总的来说，当你运行 npm run develop 的时候，会启动一个 Web 服务器，然后监听文件修改，然后自动重新合并你的代码。真的非常简洁！

4、注意
(1). 用webpack-dev-server生成bundle.js文件是在内存中的，并没有实际生成
(2). 如果引用的文件夹中已经有bundle.js就不会自动刷新了，你需要先把bundle.js文件手动删除
(3). 用webstorm的注意了，因为他是自动保存的，所以可能识别的比较慢，你需要手动的ctrl+s一下

## 浏览器自动刷新
现在可以启动一个服务并监听变化了，但是浏览器你还需要手动刷新一下，我们其实是可以通过下面的这个方法让他自动刷新的。

#### 修改webpack.develop.config.js的配置
修改entry部分如下：
```
var path = require('path');
module.exports = {
    entry:[
        'webpack/hot/dev-server',
        'webpack-dev-server/client?http://localhost:8080',
        path.resolve(__dirname,'src/js/app.js')
    ],
    output: {
        path: path.resolve(__dirname, 'deploy'),
        filename: 'bundle.js',
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: "style!css" }
        ]
    }
}

#常用加载器介绍
## 加载器介绍
Loader：这是webpack准备的一些预处理工具

## 编译jsx和ES6到原生js
#### 首先安装下面的所有依赖
```
npm install babel-loader --save-dev
npm install babel-core   babel-preset-es2015   babel-preset-react  --save-dev
```

#### 修改开发配置文件
```
module: {
    loaders: [
        {
            test: /\.jsx?$/, // 用正则来匹配文件路径，这段意思是匹配 js 或者 jsx
            loader: 'babel',// 加载模块 "babel" 是 "babel-loader" 的缩写
            query: {
                presets: ['es2015', 'react']
            }
        }
    ]
}

#### 使用
直接从新运行npm run develop即可

## 加载css
Webpack允许像加载任何代码一样加载 CSS。你可以选择你所需要的方式，但是你可以为每个组件把所有你的 CSS 加载到入口主文件中来做任何事情。加载 CSS 需要 css-loader 和 style-loader，他们做两件不同的事情，css-loader会遍历 CSS 文件，然后找到 url() 表达式然后处理他们，style-loader 会把原来的 CSS 代码插入页面中的一个 style 标签中。

#### 首先下载依赖
```
npm install css-loader   style-loader --save-dev
```
#### 修改配置文件
```
{
   test: /\.css$/, // Only .css files
   loader: 'style!css' // Run both loaders
}
```
！用来定义loader的串联关系，"-loader"是可以省略不写的，多个loader之间用“!”连接起来

#### Css加载策略
1、在你的主入口文件中，比如 src/app.js 你可以为整个项目加载所有的 CSS
```
Import  './project-styles.css';
```
CSS 就完全包含在合并的应用中，再也不需要重新下载。

2、懒加载（推荐）
如果你想发挥应用中多重入口文件的优势，你可以在每个入口点包含各自的 CSS。你把你的模块用文件夹分离，每个文件夹有各自的 CSS 和 JavaScript 文件。再次，当应用发布的时候，导入的 CSS 已经加载到每个入口文件中。

3、定制组件css
你可以根据这个策略为每个组件创建 CSS 文件，可以让组件名和 CSS 中的 class 使用一个命名空间，来避免一个组件中的一些 class 干扰到另外一些组件的 class。

4、使用内联样式取代 CSS 文件
在 “React Native” 中你不再需要使用任何 CSS 文件，你只需要使用 style 属性，可以把你的 CSS 定义成一个对象，那样就可以根据你的项目重新来考略你的 CSS 策略。

##  加载sass
####  下载依赖
```
Npm  install  sass-loader  --save-dev
```

#### 修改配置文件
```
{
      test: /\.scss$/,
     loader: 'style!css!sass'
 }
```

## 图片处理
直到 HTTP/2 你才能在应用加载的时候避免设置太多 HTTP 请求。根据浏览器不同你必须设置你的并行请求数，如果你在你的 CSS 中加载了太多图片的话，可以自动把这些图片转成 BASE64 字符串然后内联到 CSS 里来降低必要的请求数，这个方法取决与你的图片大小。你需要为你的应用平衡下载的大小和下载的数量，不过 Webpack 可以让这个平衡十分轻松适应。

#### 下载依赖
```
npm install url-loader  file-loader --save-dev
```

#### 修改配置文件
```
{
      test: /\.(png|jpg)$/,
      loader: 'url?limit=25000'
 }
```
加载器，它会把需要转换的路径变成 BASE64 字符串，在其他的 Webpack 书中提到的这方面会把你 CSS 中的 “url()” 像其他 require 或者 import 来处理。意味着如果我们可以通过它来处理我们的图片文件。

url-loader 传入的 limit 参数是告诉它图片如果不大于 25KB 的话要自动在它从属的 css 文件中转成 BASE64 字符串。

#### 大图片处理
在代码中是一下情况：
```
div.img{
    background: url(../image/xxx.jpg)
}
//或者
var img = document.createElement("img");
img.src = require("../image/xxx.jpg");
document.body.appendChild(img);
```
你可以这样配置
```
module: {
    {
      test: /\.(png|jpg)$/,
      loader: 'url-loader?limit=10000&name=build/[name].[ext]'
    }]
}
```
针对上面的两种使用方式，loader可以自动识别并处理。根据loader中的设置，webpack会将小于指点大小的文件转化成 base64 格式的 dataUrl，其他图片会做适当的压缩并存放在指定目录中。

#### 处理结果
```
After launching the server, small.png and big.png will have the following URLs.

<img src="data:image/png;base64,iVBOR...uQmCC">

<img src="4853ca667a2b8b8844eb2693ac1b2578.png">
```

## 文件处理（了解）
#### 说明
现在webpack处理css中的图片还是可以的，但是处理html中的图片能力有限，所以有图片的地方最好放在css中，或者使用require的方式

#### 下载依赖
```
npm install file-loader –save-dev
```

#### 修改配置文件
```
{
    test: /\.(png|jpeg|gif|jpg)$/,
    loader: 'file-loader?name=images/[name].[ext]'
}
```
注意：这点由于一直加载不上图片，所以我把输出的配置改为了
```
output: {
    path: path.resolve(__dirname, 'deploy'),
    filename: 'bundle.js',
},
```
我把下面的loader注释了，影响图片的加载
```
//{
//    test: /\.(png|jpg)$/,
//    loader: 'url?limit=25000'
//},
```

## 内联fonts（了解）
字体实在是非常难引入正确，首先，通常我们有 4 种不一样的格式，但是只有其中一种会被对应的浏览器使用到。你肯定不会想引入全部四种格式，这样只会让 CSS 文件更加膨胀，然后又没办法优化。
取决与你的项目，你可能可以选择出一种字体格式，如果你不考略 Opera Mini，所有的浏览器都支持 .woff 和 .svg 格式。问题是不同格式下在各种浏览器下字体看起来会有一点点不同。所以测试 .woff 和 .svg，然后找出能够在所有浏览器中看起来最好的那个。
```
{
      test: /\.woff$/,
      loader: 'url?limit=100000'
}
```

## 特殊模块的 Shim（了解）
比如某个模块依赖 window.jQuery, 需要从 npm 模块中将 jquery 挂载到全局
Webpack 有不少的 Shim 的模块, 比如 expose-loader 用于解决这个问题[https://github.com/webpack/docs/wiki/shimming-modules](https://github.com/webpack/docs/wiki/shimming-modules)
其他比如从模块中导出变量...具体说明有点晦涩..
手头的两个例子, 比如我们用到 Pen 这个模块,
这个模块对依赖一个 window.jQuery, 可我手头的 jQuery 是 CommonJS 语法的
而 Pen 对象又是生成好了绑在全局的, 可是我又需要通过 require('pen') 获取变量
最终的写法就是做 Shim 处理直接提供支持:
{test: require.resolve('jquery'), loader: 'expose?jQuery'},
{test: require.resolve('pen'), loader: 'exports?window.Pen'},

# 部署策略
## 发布配置
#### 修改npm的package.json文件
```
"publish": " webpack --config webpack.publish.config.js  -p",
```
指向生产的配置文件，并且加上了webpack的cli的-p,他会自动做一些优化

#### 修改webpack.publish.config.js文件
```
var path = require('path');
var node_modules = path.resolve(__dirname, 'node_modules');
module.exports = {
    entry: path.resolve(__dirname,'src/js/app.js'),
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js',
    },
    module: {
        loaders: [
            {
                test: /\.jsx?$/, // 用正则来匹配文件路径，这段意思是匹配 js 或者 jsx
                loader: 'babel',// 加载模块 "babel" 是 "babel-loader" 的缩写
                query: {
                    presets: ['es2015', 'react']
                }
            },
            {
                test: /\.css$/, // 处理css文件
                loader: 'style!css' // Run both loaders
            },
            {
                test: /\.scss$/,  //处理sass文件
                loader: 'style!css!sass'
            },
            {
                test: /\.(png|jpg)$/,
                loader: 'url?limit=25000'
            }
        ]
    }
}
```
可以看到，其实生产环境的配置和开发的配置没有太大的不同，主要是把不需要的东西给去掉了
```
Npm  run  publish
```

## 合并成单文件
一般情况下只有在下面的情况下才使用单入口模式：
1、应用很小
2、很少会更新应用
3、你不太关心初始加载时间

## 分离应用和第三方
#### 何时应该分离
当你的应用依赖其他库尤其是像 React JS 这种大型库的时候，你需要考虑把这些依赖分离出去，这样就能够让用户在你更新应用之后不需要再次下载第三方文件。当满足下面几个情况的时候你就需要这么做了：
1、当你的第三方的体积达到整个应用的 20% 或者更高的时候。
2、更新应用的时候只会更新很小的一部分
3、你没有那么关注初始加载时间，不过关注优化那些回访用户在你更新应用之后的体验。
4、有手机用户。

#### 修改配置文件
```
var webpack=require("webpack")；//在头部引入
```
结果：
注意：记住要把这些文件都加入到你的 HTML 代码中，不然你会得到一个错误：Uncaught ReferenceError: webpackJsonp is not defined。

## 和gulp的集成
分为使用流和不使用流[http://webpack.github.io/docs/usage-with-gulp.html](http://webpack.github.io/docs/usage-with-gulp.html)

gulp + webpack 构建多页面前端项目[http://cnodejs.org/topic/56df76559386fbf86ddd6916](http://cnodejs.org/topic/56df76559386fbf86ddd6916)

github-demo实际例子: https://github.com/MeCKodo/webpack

#常用插件介绍
webpack提供了[丰富的组件]用来满足不同的需求，当然我们也可以自行实现一个组件来满足自己的需求：
```
plugins: [

     //your plugins list

 ]
```
详细的请看这里：http://webpack.github.io/docs/list-of-plugins.html#uglifyjsplugin
注释：Word中出现的所有的包，都可以通过npm进行包查找，然后查看具体的使用方法；

## 压缩插件
这个插件是webpack自带的

## 提取css插件
在webpack中编写js文件时，可以通过require的方式引入其他的静态资源，可通过loader对文件自动解析并打包文件。通常会将js 文件打包合并，css文件会在页面的header中嵌入style的方式载入页面。但开发过程中我们并不想将样式打在脚本中，最好可以独立生成css文 件，以外链的形式加载。这时extract-text-webpack-plugin插件可以帮我们达到想要的效果。需要使用npm的方式加载插件，然后 参见下面的配置，就可以将js中的css文件提取，并以指定的文件名来进行加载。
```
npm install extract-text-webpack-plugin --save-dev
```
我发现这个有一个问题，就是他只能把css抽出来，但是sass的样式不能分离出来。
```
var ExtractTextPlugin = require("extract-text-webpack-plugin");
```

## 创建index.Html页面插件
```
html-webpack-plugin 
```

## 自动打开浏览器插件
```
open-browser-webpack-plugin
```
[https://github.com/baldore/open-browser-webpack-plugin](https://github.com/baldore/open-browser-webpack-plugin)

## 提取js公共部分插件
```
CommonsChunkPlugin
```

## ProvidePlugin插件

自动添加引用插件，全局暴露插件，直接使用

## 删除目录插件
```
clean-webpack-plugin 
```

## 拷贝文件插件
```
copy-webpack-plugin 
```

## 合并配置文件插件
```
webpack-config
```
[https://github.com/mdreizin/webpack-config](https://github.com/mdreizin/webpack-config)

# 其他了解知识点
## webpack中的非入口文件（异步加载）
这个是重点要配合chunkname属性，后面的react-router的动态路由会用到[http://react-china.org/t/webpack-output-filename-output-chunkfilename/2256/2](http://react-china.org/t/webpack-output-filename-output-chunkfilename/2256/2)

基本上都是在require.ensure去加载模块的时候才会出现，chunkFileName，个人理解是cmd和amd异步加载，而且没有给入口文 件时，会生成了no-name的chunk，所以你看到的例子，chunkFileName一般都会是[id].[chunkhash].js,也就是这 种chunk的命名一般都会是0.a5898fnub6.js.

## Resolve属性
webpack在构建包的时候会按目录的进行文件的查找，resolve属性中的extensions数组中用于配置程序可以自行补全哪些文件后缀：
```
resolve: {
        //查找module的话从这里开始查找
        root: '/pomy/github/flux-example/src', //绝对路径
        //自动扩展文件后缀名，意味着我们require模块可以省略不写后缀名
        //注意一下, extensions 第一个是空字符串! 对应不需要后缀的情况.
        extensions: ['', '.js', '.json', '.scss',’jsx’],
        //模块别名定义，方便后续直接引用别名，无须多写长长的地址
        alias: {
            AppStore : 'js/stores/AppStores.js',//后续直接 require('AppStore') 即可
            ActionType : 'js/actions/ActionType.js',
            AppAction : 'js/actions/AppAction.js'
        }
    }
```

## Externals属性

外部依赖不需要打包进bundle，当我们想在项目中require一些其他的类库或者API，而又不想让这些类库的源码被构建到运行时文件中，这在实际开发中很有必要。

比如：你在页面里通过script标签引用了jQuery：<script src="//code.jquery.com/jquery-1.12.0.min.js"></script>，所以并不想在其他js里再打包进入一遍，比如你的其他js代码类似：

其实就是不是通过require或者import引入的，而是直接写在html中的js地址。
