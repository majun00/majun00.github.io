---
title: Gulp
tags: 
- Gulp
categories:
- Gulp
---

## gulp配置

- 安装配置gulp:

  + npm install gulp-cli -g

  + npm install gulp --save

  + 新建gulpfile.js

  + npm install gulp-uglify --save

  + npm install gulp-concat --save

  + npm install gulp-cssnano --save

  + npm install gulp-htmlmin --save

> 还有gulp-minify-css gulp-imagemin gulp-less gulp-ruby-sass

```
npm install gulp-uglify gulp-watch-path stream-combiner2 gulp-sourcemaps gulp-minify-css gulp-autoprefixer gulp-less gulp-ruby-sass gulp-imagemin gulp-util --save-dev
```

## 五个核心方法

- gulp.task('任务名',function(){})   创建任务

- gulp.src('./*.css')     指定想要处理的文件

- gulp.dest()     指定最终处理后的文件的存放路径

- gulp.watch()    自动的监视文件的变化，然后执行相应任务

- gulp.run('任务名')     直接执行相应的任务

#### 检测代码修改自动执行任务

```
gulp.task('auto', function () {
    // 监听文件修改，当文件被修改则执行 script 任务
    gulp.watch('js/*.js', ['script'])
}) 
```

#### 使用 gulp.task('default', fn) 定义默认任务

增加如下代码

```
gulp.task('default', ['script', 'auto']);
```

此时你可以在命令行直接输入 `gulp` +回车，运行 `script` 和 `auto` 任务。

#### 压缩JS

```
// 获取 gulp
var gulp = require('gulp')

// 获取 uglify 模块（用于压缩 JS）
var uglify = require('gulp-uglify')

// 压缩 js 文件
// 在命令行使用 gulp script 启动此任务
gulp.task('script', function() {
    // 1\. 找到文件
    gulp.src('js/*.js')
    // 2\. 压缩文件
        .pipe(uglify())
    // 3\. 另存压缩后的文件
        .pipe(gulp.dest('dist/js'))
})

// 在命令行使用 gulp auto 启动此任务
gulp.task('auto', function () {
    // 监听文件修改，当文件被修改则执行 script 任务
    gulp.watch('js/*.js', ['script'])
})

// 使用 gulp.task('default') 定义默认任务
// 在命令行使用 gulp 启动 script 任务和 auto 任务
gulp.task('default', ['script', 'auto'])
```

#### 压缩CSS

```
// 获取 gulp
var gulp = require('gulp')

// 获取 minify-css 模块（用于压缩 CSS）
var minifyCSS = require('gulp-minify-css')

// 压缩 css 文件
// 在命令行使用 gulp css 启动此任务
gulp.task('css', function () {
    // 1\. 找到文件
    gulp.src('css/*.css')
    // 2\. 压缩文件
        .pipe(minifyCSS())
    // 3\. 另存为压缩文件
        .pipe(gulp.dest('dist/css'))
})

// 在命令行使用 gulp auto 启动此任务
gulp.task('auto', function () {
    // 监听文件修改，当文件被修改则执行 css 任务
    gulp.watch('css/*.css', ['css'])
});

// 使用 gulp.task('default') 定义默认任务
// 在命令行使用 gulp 启动 css 任务和 auto 任务
gulp.task('default', ['css', 'auto'])
```

#### 压缩图片

```
// 获取 gulp
var gulp = require('gulp');

// 获取 gulp-imagemin 模块
var imagemin = require('gulp-imagemin')

// 压缩图片任务
// 在命令行输入 gulp images 启动此任务
gulp.task('images', function () {
    // 1\. 找到图片
    gulp.src('images/*.*')
    // 2\. 压缩图片
        .pipe(imagemin({
            progressive: true
        }))
    // 3\. 另存图片
        .pipe(gulp.dest('dist/images'))
});

// 在命令行使用 gulp auto 启动此任务
gulp.task('auto', function () {
    // 监听文件修改，当文件被修改则执行 images 任务
    gulp.watch('images/*.*)', ['images'])
});

// 使用 gulp.task('default') 定义默认任务
// 在命令行使用 gulp 启动 images 任务和 auto 任务
gulp.task('default', ['images', 'auto'])
```

#### 编译LESS

```
// 获取 gulp
var gulp = require('gulp')
// 获取 gulp-less 模块
var less = require('gulp-less')

// 编译less
// 在命令行输入 gulp less 启动此任务
gulp.task('less', function () {
    // 1\. 找到 less 文件
    gulp.src('less/**.less')
    // 2\. 编译为css
        .pipe(less())
    // 3\. 另存文件
        .pipe(gulp.dest('dist/css'))
});

// 在命令行使用 gulp auto 启动此任务
gulp.task('auto', function () {
    // 监听文件修改，当文件被修改则执行 less 任务
    gulp.watch('less/**.less', ['less'])
})

// 使用 gulp.task('default') 定义默认任务
// 在命令行使用 gulp 启动 less 任务和 auto 任务
gulp.task('default', ['less', 'auto'])
```

#### 编译SASS

```
// 获取 gulp
var gulp = require('gulp')
// 获取 gulp-ruby-sass 模块
var sass = require('gulp-ruby-sass')

// 编译sass
// 在命令行输入 gulp sass 启动此任务
gulp.task('sass', function() {
    return sass('sass/') 
    .on('error', function (err) {
      console.error('Error!', err.message);
   })
    .pipe(gulp.dest('dist/css'))
});

// 在命令行使用 gulp auto 启动此任务
gulp.task('auto', function () {
    // 监听文件修改，当文件被修改则执行 images 任务
    gulp.watch('sass/**/*.scss', ['sass'])
});

// 使用 gulp.task('default') 定义默认任务
// 在命令行使用 gulp 启动 sass 任务和 auto 任务
gulp.task('default', ['sass', 'auto'])
```

#### 实例
```

var gulp =  require('gulp');

// js里引用js文件用require

var uglify=require('gulp-uglify');

var concat=require('gulp-concat');

var cssnano=require('gulp-cssnano');

var htmlmin = require('gulp-htmlmin');

gulp.task('test', function(){

  console.log(123)

}

执行任务: gulp 任务名 如 gulp test

gulp.task('script', function(){

  gulp.src(['./app.js','./sign.js'])

  // src指定要处理的文件

  // 参数是匹配的规则,也可以是数组，数组中的元素就是匹配的规则

  .pipe(concat('index.js'))

  // concat 的参数是合并之后的文件名字

  .pipe(uglify())

  .pipe(gulp.dest('./dist'))

  // dest方法参数，指定输出文件的路径

})

gulp.task('style', function(){

  gulp.src(['./*.css'])

  .pipe(concat('index.css'))

  .pipe(cssnano())

  .pipe(gulp.dest('./dist'))

})

gulp.task('html', function(){

 gulp.src(['./index.html'])

 .pipe(htmlmin({collapseWhitespace:true}))

 .pipe(gulp.dest('./dist'))

})

gulp.task('mywatch', function(){

// gulp.watch 监视文件变化，执行相应任务

  gulp.run('script')

  // 执行指定的任务

  gulp.watch(['./app.js','sign.js'],['script'])

  // watch监视js文件的变化，然后执行script任务

  // 第一个参数：要监视的文件的规则

  // 第二个参数：是要执行的任务

})

```

## gulp 高级

#### package.json

如果你熟悉 npm 则可以利用 `package.json` 保存所有 `npm install --save-dev gulp-xxx` 模块依赖和模块版本。

在命令行输入

```
npm init
```

会依次要求补全项目信息，不清楚的可以直接回车跳过

```
name: (gulp-demo) 
version: (1.0.0) 
description: 
entry point: (index.js) 
test command: 
...
...
Is this ok? (yes) 
```

最终会在当前目录中创建 `package.json` 文件并生成类似如下代码：

```
{
  "name": "gulp-demo",
  "version": "0.0.0",
  "description": "",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/nimojs/gulp-demo.git"
  },
  "keywords": [
    "gulp",
  ],
  "author": "nimojs <nimo.jser@gmail.com>",
  "license": "MIT",
  "bugs": {
    "url": "https://github.com/nimojs/gulp-demo/issues"
  },
  "homepage": "https://github.com/nimojs/gulp-demo"
}
```

#### 安装依赖

安装 gulp 到项目（防止全局 gulp 升级后与此项目 `[gulpfile.js](http://gulpfile.js/)` 代码不兼容）

```
npm install gulp --save-dev
```

此时打开 `package.json` 会发现多了如下代码

```
"devDependencies": {
    "gulp": "^3.8.11"
}
```

声明此项目的开发依赖 gulp

接着安装其他依赖：

> 安装模块较多，请耐心等待，若一直安装失败可使用[npm.taobao.org](http://npm.taobao.org/)

```
npm install gulp-uglify gulp-watch-path stream-combiner2 gulp-sourcemaps gulp-minify-css gulp-autoprefixer gulp-less gulp-ruby-sass gulp-imagemin gulp-util --save-dev
```

此时，[package.json](https://github.com/nimojs/gulp-demo/blob/master/package.json) 将会更新

```
"devDependencies": {
    "colors": "^1.0.3",
    "gulp": "^3.8.11",
    "gulp-autoprefixer": "^2.1.0",
    "gulp-imagemin": "^2.2.1",
    "gulp-less": "^3.0.2",
    "gulp-minify-css": "^1.0.0",
    "gulp-ruby-sass": "^1.0.1",
    "gulp-sourcemaps": "^1.5.1",
    "gulp-uglify": "^1.1.0",
    "gulp-watch-path": "^0.0.7",
    "stream-combiner2": "^1.0.2"
}
```

当你将这份 gulpfile.js 配置分享给你的朋友时，就不需要将 `node_modules/` 发送给他，他只需在命令行输入

```
npm install
```

就可以检测 `package.json` 中的 `devDependencies` 并安装所有依赖。

#### 设计目录结构

我们将文件分为2类，一类是源码，一类是编译压缩后的版本。文件夹分别为 `src` 和 `dist`。(注意区分 `dist` 和 ·`dest`的区别)

```
└── src/
│
└── dist/
```

`dist/` 目录下的文件都是根据 `src/` 下所有源码文件构建而成。

在 `src/` 下创建前端资源对应的的文件夹

```
└── src/
    ├── less/    *.less 文件
    ├── sass/    *.scss *.sass 文件
    ├── css/     *.css  文件
    ├── js/      *.js 文件
    ├── fonts/   字体文件
    └── images/   图片
└── dist/
```

你可以点击 [nimojs/gulp-demo](https://github.com/nimojs/gulp-demo/archive/master.zip) 下载本章代码。

#### 让命令行输出的文字带颜色

gulp 自带的输出都带时间和颜色，这样很容易识别。我们利用 [gulp-util](https://github.com/gulpjs/gulp-util) 实现同样的效果。

```
var gulp = require('gulp')
var gutil = require('gulp-util')

gulp.task('default', function () {
    gutil.log('message')
    gutil.log(gutil.colors.red('error'))
    gutil.log(gutil.colors.green('message:') + "some")
})
```

使用 `gulp` 启动默认任务以测试[图片上传失败...(image-11abe7-1512463170543)]

#### 配置 JS 任务

##### gulp-uglify

检测`src/js/`目录下的 js 文件修改后，压缩 `js/` 中所有 js 文件并输出到 `dist/js/` 中

```
var uglify = require('gulp-uglify')

gulp.task('uglifyjs', function () {
    gulp.src('src/js/**/*.js')
        .pipe(uglify())
        .pipe(gulp.dest('dist/js'))
})

gulp.task('default', function () {
    gulp.watch('src/js/**/*.js', ['uglifyjs'])
})
```

`src/js/**/*.js` 是 glob 语法。[百度百科：glob模式](http://baike.baidu.com/view/4019153.htm) 、[node-glob](https://github.com/isaacs/node-glob)

在命令行输入 `gulp` 后会出现如下消息，表示已经启动。

```
[20:39:50] Using gulpfile ~/Documents/code/gulp-book/demo/chapter7/gulpfile.js
[20:39:50] Starting 'default'...
[20:39:50] Finished 'default' after 13 ms
```

此时编辑 [src/js/log.js](https://github.com/nimojs/gulp-demo/blob/master/src/js/log.js) 文件并保存，命令行会出现如下消息，表示检测到 `src/js/**/*.js` 文件修改后重新编译所有 js。

```
[20:39:52] Starting 'js'...
[20:39:52] Finished 'js' after 14 ms
```

##### gulp-watch-path

此配置有个性能问题，当 `gulp.watch` 检测到 `src/js/` 目录下的js文件有修改时会将所有文件全部编译。实际上我们只需要重新编译被修改的文件。

简单介绍 `gulp.watch` 第二个参数为 `function` 时的用法。

```
gulp.watch('src/js/**/*.js', function (event) {
    console.log(event);
    /*
    当修改 src/js/log.js 文件时
    event {
        // 发生改变的类型，不管是添加，改变或是删除
        type: 'changed', 
        // 触发事件的文件路径
        path: '/Users/nimojs/Documents/code/gulp-book/demo/chapter7/src/js/log.js'
    }
    */
})
```

我们可以利用 `event` 给到的信息，检测到某个 js 文件被修改时，只编写当前修改的 js 文件。

可以利用 `gulp-watch-path` 配合 `event` 获取编译路径和输出路径。

```
var watchPath = require('gulp-watch-path')

gulp.task('watchjs', function () {
    gulp.watch('src/js/**/*.js', function (event) {
        var paths = watchPath(event, 'src/', 'dist/')
        /*
        paths
            { srcPath: 'src/js/log.js',
              srcDir: 'src/js/',
              distPath: 'dist/js/log.js',
              distDir: 'dist/js/',
              srcFilename: 'log.js',
              distFilename: 'log.js' }
        */
        gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)

        gulp.src(paths.srcPath)
            .pipe(uglify())
            .pipe(gulp.dest(paths.distDir))
    })
})

gulp.task('default', ['watchjs'])
```

[use-gulp-watch-path 完整代码](https://github.com/nimojs/gulp-book/tree/master/demo/chapter7/use-gulp-watch-path.js)

`watchPath(event, search, replace, distExt)`

| 参数 | 说明 |
| --- | --- |
| event | `gulp.watch` 回调函数的 `event` |
| search | 需要被替换的起始字符串 |
| replace | 第三个参数是新的的字符串 |
| distExt | 扩展名(非必填) |

此时编辑 [src/js/log.js](https://github.com/nimojs/gulp-demo/blob/master/src/js/log.js) 文件并保存，命令行会出现消息，表示检测到 `src/js/log.js` 文件修改后只重新编译 `[log.js](http://log.js/)`。

```
[21:47:25] changed src/js/log.js
[21:47:25] Dist dist/js/log.js
```

你可以访问 [gulp-watch-path](https://github.com/nimojs/gulp-watch-path) 了解更多。

##### stream-combiner2

编辑 `[log.js](http://log.js/)` 文件时，如果文件中有 js 语法错误时，gulp 会终止运行并报错。

当 log.js 缺少 `)`

```
log('gulp-book'
```

并保存文件时出现如下错误，但是错误信息不全面。而且还会让 gulp 停止运行。

```
events.js:85
      throw er; // Unhandled 'error' event
            ^
Error
    at new JS_Parse_Error (/Users/nimojs/Documents/code/gulp-book/demo/chapter7/node_modules/gulp-uglify/node_modules/uglify-js/lib/parse.js:189:18)
...
...
js_error (/Users/nimojs/Documents/code/gulp-book/demo/chapter7/node_modules/gulp-
-book/demo/chapter7/node_modules/gulp-uglify/node_modules/uglify-js/lib/parse.js:1165:20)
    at maybe_unary (/Users/nimojs/Documents/code/gulp-book/demo/chapter7/node_modules/gulp-uglify/node_modules/uglify-js/lib/parse.js:1328:19)

```

应对这种情况，我们可以使用 [Combining streams to handle errors](https://github.com/gulpjs/gulp/blob/master/docs/recipes/combining-streams-to-handle-errors.md) 文档中推荐的 [stream-combiner2](https://github.com/substack/stream-combiner2) 捕获错误信息。

```
var handleError = function (err) {
    var colors = gutil.colors;
    console.log('\n')
    gutil.log(colors.red('Error!'))
    gutil.log('fileName: ' + colors.red(err.fileName))
    gutil.log('lineNumber: ' + colors.red(err.lineNumber))
    gutil.log('message: ' + err.message)
    gutil.log('plugin: ' + colors.yellow(err.plugin))
}
var combiner = require('stream-combiner2')

gulp.task('watchjs', function () {
    gulp.watch('src/js/**/*.js', function (event) {
        var paths = watchPath(event, 'src/', 'dist/')
        /*
        paths
            { srcPath: 'src/js/log.js',
              srcDir: 'src/js/',
              distPath: 'dist/js/log.js',
              distDir: 'dist/js/',
              srcFilename: 'log.js',
              distFilename: 'log.js' }
        */
        gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)

        var combined = combiner.obj([
            gulp.src(paths.srcPath),
            uglify(),
            gulp.dest(paths.distDir)
        ])

        combined.on('error', handleError)
    })
})
```

[watchjs-1 完整代码](https://github.com/nimojs/gulp-book/tree/master/demo/chapter7/watchjs-1.js)

此时当编译错误的语法时，命令行会出现错误提示。而且不会让 gulp 停止运行。

```
changed:src/js/log.js
dist:dist/js/log.js
--------------
Error!
fileName: /Users/nimojs/Documents/code/gulp-book/demo/chapter7/src/js/log.js
lineNumber: 7
message: /Users/nimojs/Documents/code/gulp-book/demo/chapter7/src/js/log.js: Unexpected token eof «undefined», expected punc «,»
plugin: gulp-uglify
```

##### gulp-sourcemaps

JS 压缩前和压缩后比较

```
// 压缩前
var log = function (msg) {
    console.log('--------');
    console.log(msg)
    console.log('--------');
}
log({a:1})
log('gulp-book')

// 压缩后
var log=function(o){console.log("--------"),console.log(o),console.log("--------")};log({a:1}),log("gulp-book");
```

压缩后的代码不存在换行符和空白符，导致出错后很难调试，好在我们可以使用 [sourcemap](http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html) 帮助调试

```
var sourcemaps = require('gulp-sourcemaps')
// ...
var combined = combiner.obj([
    gulp.src(paths.srcPath),
    sourcemaps.init(),
    uglify(),
    sourcemaps.write('./'),
    gulp.dest(paths.distDir)
])
// ...
```

[watchjs-2 完整代码](https://github.com/nimojs/gulp-book/tree/master/demo/chapter7/watchjs-1.js)

此时 `dist/js/` 中也会生成对应的 `.map` 文件，以便使用 Chrome 控制台调试代码 [在线文件示例：src/js/](https://github.com/nimojs/gulp-demo/blob/master/src/js/)

* * *

至此，我们完成了检测文件修改后压缩 JS 的 gulp 任务配置。

有时我们也需要一次编译所有 js 文件。可以配置 `uglifyjs` 任务。

```
gulp.task('uglifyjs', function () {
    var combined = combiner.obj([
        gulp.src('src/js/**/*.js'),
        sourcemaps.init(),
        uglify(),
        sourcemaps.write('./'),
        gulp.dest('dist/js/')
    ])
    combined.on('error', handleError)
})
```

在命令行输入 `gulp uglifyjs` 以压缩 `src/js/` 下的所有 js 文件。

#### 配置 CSS 任务

有时我们不想使用 LESS 或 SASS而是直接编写 CSS，但我们需要压缩 CSS 以提高页面加载速度。

##### gulp-minify-css

按照本章中压缩 JS 的方式，先编写 `watchcss` 任务

```
var minifycss = require('gulp-minify-css')

gulp.task('watchcss', function () {
    gulp.watch('src/css/**/*.css', function (event) {
        var paths = watchPath(event, 'src/', 'dist/')

        gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)

        gulp.src(paths.srcPath)
            .pipe(sourcemaps.init())
            .pipe(minifycss())
            .pipe(sourcemaps.write('./'))
            .pipe(gulp.dest(paths.distDir))
    })
})

gulp.task('default', ['watchjs','watchcss'])
```

##### gulp-autoprefixer

autoprefixer 解析 CSS 文件并且添加浏览器前缀到CSS规则里。 通过示例帮助理解

autoprefixer 处理前：

```
.demo {
    display:flex;
}
```

autoprefixer 处理后：

```
.demo {
    display:-webkit-flex;
    display:-ms-flexbox;
    display:flex;
}
```

你只需要关心编写标准语法的 css，autoprefixer 会自动补全。

在 watchcss 任务中加入 autoprefixer:

```
gulp.task('watchcss', function () {
    gulp.watch('src/css/**/*.css', function (event) {
        var paths = watchPath(event, 'src/', 'dist/')

        gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)

        gulp.src(paths.srcPath)
            .pipe(sourcemaps.init())
            .pipe(autoprefixer({
              browsers: 'last 2 versions'
            }))
            .pipe(minifycss())
            .pipe(sourcemaps.write('./'))
            .pipe(gulp.dest(paths.distDir))
    })
})
```

更多 autoprefixer 参数请查看 [gulp-autoprefixer](https://github.com/sindresorhus/gulp-autoprefixer)

有时我们也需要一次编译所有 css 文件。可以配置 `minifyss` 任务。

```
gulp.task('minifycss', function () {
    gulp.src('src/css/**/*.css')
        .pipe(sourcemaps.init())
        .pipe(autoprefixer({
          browsers: 'last 2 versions'
        }))
        .pipe(minifycss())
        .pipe(sourcemaps.write('./'))
        .pipe(gulp.dest('dist/css/'))
})
```

在命令行输入 `gulp minifyss` 以压缩 `src/css/` 下的所有 .css 文件并复制到 `dist/css` 目录下

#### 配置 Less 任务

参考配置 JavaScript 任务的方式配置 less 任务

```
var less = require('gulp-less')

gulp.task('watchless', function () {
    gulp.watch('src/less/**/*.less', function (event) {
        var paths = watchPath(event, 'src/less/', 'dist/css/')

        gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)
        var combined = combiner.obj([
            gulp.src(paths.srcPath),
            sourcemaps.init(),
            autoprefixer({
              browsers: 'last 2 versions'
            }),
            less(),
            minifycss(),
            sourcemaps.write('./'),
            gulp.dest(paths.distDir)
        ])
        combined.on('error', handleError)
    })
})

gulp.task('lesscss', function () {
    var combined = combiner.obj([
            gulp.src('src/less/**/*.less'),
            sourcemaps.init(),
            autoprefixer({
              browsers: 'last 2 versions'
            }),
            less(),
            minifycss(),
            sourcemaps.write('./'),
            gulp.dest('dist/css/')
        ])
    combined.on('error', handleError)
})

gulp.task('default', ['watchjs', 'watchcss', 'watchless'])
```

#### 配置 Sass 任务

参考配置 JavaScript 任务的方式配置 Sass 任务

```
gulp.task('watchsass',function () {
    gulp.watch('src/sass/**/*', function (event) {
        var paths = watchPath(event, 'src/sass/', 'dist/css/')

        gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)
        sass(paths.srcPath)
            .on('error', function (err) {
                console.error('Error!', err.message);
            })
            .pipe(sourcemaps.init())
            .pipe(minifycss())
            .pipe(autoprefixer({
              browsers: 'last 2 versions'
            }))
            .pipe(sourcemaps.write('./'))
            .pipe(gulp.dest(paths.distDir))
    })
})

gulp.task('sasscss', function () {
        sass('src/sass/')
        .on('error', function (err) {
            console.error('Error!', err.message);
        })
        .pipe(sourcemaps.init())
        .pipe(minifycss())
        .pipe(autoprefixer({
          browsers: 'last 2 versions'
        }))
        .pipe(sourcemaps.write('./'))
        .pipe(gulp.dest('dist/css'))
})

gulp.task('default', ['watchjs', 'watchcss', 'watchless', 'watchsass', 'watchsass'])
```

#### 配置 image 任务

```
var imagemin = require('gulp-imagemin')

gulp.task('watchimage', function () {
    gulp.watch('src/images/**/*', function (event) {
        var paths = watchPath(event,'src/','dist/')

        gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)

        gulp.src(paths.srcPath)
            .pipe(imagemin({
                progressive: true
            }))
            .pipe(gulp.dest(paths.distDir))
    })
})

gulp.task('image', function () {
    gulp.src('src/images/**/*')
        .pipe(imagemin({
            progressive: true
        }))
        .pipe(gulp.dest('dist/images'))
})
```

#### 配置文件复制任务

复制 `src/fonts/` 文件到 `dist/` 中

```
gulp.task('watchcopy', function () {
    gulp.watch('src/fonts/**/*', function (event) {
        var paths = watchPath(event)

        gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)

        gulp.src(paths.srcPath)
            .pipe(gulp.dest(paths.distDir))
    })
})

gulp.task('copy', function () {
    gulp.src('src/fonts/**/*')
        .pipe(gulp.dest('dist/fonts/'))
})

gulp.task('default', ['watchjs', 'watchcss', 'watchless', 'watchsass', 'watchimage', 'watchcopy'])
```

> 参考自: https://github.com/onface/gulp-book
