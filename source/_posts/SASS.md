---
title: 学习Sass
tags: 
- Sass
categories:
- Sass
---

> css扩展语言 面向对象 变量 嵌套 混合 导入

## 安装Sass和Compass
1. sass基于Ruby语言开发而成，因此安装sass前需要安装Ruby。window下安装SASS首先需要安装Ruby，先从官网下载Ruby并安装。安装过程中请注意勾选Add Ruby executables to your PATH添加到系统环境变量。安装完成后需测试安装有没有成功,运行CMD输入以下命令：
```
$ ruby -v

//如安装成功会打印

ruby 2.2.2p95 (2015-04-13 revision 50295) [i386-mingw32]
```

2. 因为国内网络的问题导致gem源间歇性中断因此我们需要更换gem源。（使用淘宝的gem源https://ruby.taobao.org/）如下：
```
//1.删除原gem源
gem sources --remove https://rubygems.org/

//2.添加国内淘宝源
gem sources -a https://ruby.taobao.org/

//3.打印是否替换成功
gem sources -l

//4.更换成功后打印如下
*** CURRENT SOURCES ***
https://ruby.taobao.org/
```

3. Ruby自带一个叫做RubyGems的系统，用来安装基于Ruby的软件。
```
//安装如下(如mac安装遇到权限问题需加 sudo gem install sass)
gem install sass
gem install compass
//安装完成之后通过运行下面的命令来确认应用已经正确地安装到了电脑中：

sass -v
Sass 3.x.x (Selective Steve)

compass -v
Compass 1.x.x (Polaris)
Copyright (c) 2008-2015 Chris Eppstein
Released under the MIT License.
Compass is charityware.
Please make a tax deductable donation for a worthy cause: http://umdf.org/compass
```

4. sass常用更新、查看版本、sass命令帮助等命令：
```
//更新sass
gem update sass

//查看sass版本
sass -v

//查看sass帮助
sass -h
```

## 编译sass
> sass编译有很多种方式，如命令行编译模式、sublime插件SASS-Build、编译软件koala、前端自动化软件codekit、Grunt打造前端自动化工作流grunt-sass、Gulp打造前端自动化工作流gulp-ruby-sass等。

#### 1. 使用变量;
sass使用$符号来标识变量
1. 变量声明;
`$highlight-color: #F90;`
与CSS属性不同，变量可以在css规则块定义之外存在。当变量定义在css规则块内，那么该变量只能在此规则块内使用。如果它们出现在任何形式的{...}块中（如@media或者@font-face块），情况也是如此。(即存在作用域)

2. 变量引用;
```
$highlight-color: #F90;
$highlight-border: 1px solid $highlight-color;
.selected {
  border: $highlight-border;
}

//编译后
.selected {
  border: 1px solid #F90;
}
```

3. 变量名用中划线还是下划线分隔;
使用中划线的方式更为普遍，但两种用法相互兼容。用中划线声明的变量可以使用下划线的方式引用，反之亦然。

#### 2. 嵌套CSS 规则;
```
#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  #content aside { background-color: #EEE }
}
```
1. 父选择器的标识符&;
```
article a {
  color: blue;
  &:hover { color: red }
}
article a { color: blue }
article a:hover { color: red }
```
还有另外一种用法，你可以在父选择器之前添加选择器。举例来说，当用户在使用IE浏览器时，你会通过JavaScript在<body>标签上添 加一个ie的类名，为这种情况编写特殊的样式如下：
```
#content aside {
  color: red;
  body.ie & { color: green }
}
/*编译后*/
#content aside {color: red};
body.ie #content aside { color: green }
```

2. 群组选择器的嵌套;
```
.container {
  h1, h2, h3 {margin-bottom: .8em}
}
nav, aside {
  a {color: blue}
}
```

3. 子组合选择器和同层组合选择器：>、+和~;
- 子组合选择器>选择一个元素的直接子元素
- 用同层相邻组合选择器+选择header元素后紧跟的p元素
- 用同层全体组合选择器~，选择所有跟在article后的同层article元素
```
article {
  ~ article { border-top: 1px dashed #ccc }
  > section { background: #eee }
  dl > {
    dt { color: #333 }
    dd { color: #555 }
  }
  nav + & { margin-top: 0 }
}
```

4. 嵌套属性;
```
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}
// 编译后
nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}
```
嵌套属性的规则是这样的：把属性名从中划线-的地方断开，在根属性后边添加一个冒号:，紧跟一个{ }块，把子属性部分写在这个{ }块中。
```
nav {
  border: 1px solid #ccc {
  left: 0px;
  right: 0px;
  }
}
// 编译后
nav {
  border: 1px solid #ccc;
  border-left: 0px;
  border-right: 0px;
}
```

#### 3. 导入SASS文件;
1. 使用SASS导入部分文件;
当通过@import把sass样式分散到多个文件时，你通常只想生成少数几个css文件。那些专门为@import命令而编写的sass文件，并不需要生成对应的独立css文件，这样的sass文件称为局部文件。对此，sass有一个特殊的约定来命名这些文件。此约定即：sass局部文件的文件名以下划线开头。这样，sass就不会在编译时单独编译这个文件输出css，而只把这个文件用作导入。当你@import一个局部文件时，还可以不写文件的全名，即省略文件名开头的下划线。举例来说，你想导入themes/_night-sky.scss这个局部文件里的变量，你只需在样式表中写@import "themes/night-sky";。

2. 默认变量值;
当一些样式需要在多个页面甚至多个项目中使用时，这非常有用。在这种情况下，有时需要在你的样式表中对导入的样式稍作修改，sass有一个功能刚好可以解决这个问题，即默认变量值。使用sass的!default标签可以实现这个目的。它很像css属性中!important标签的对立面，不同的是!default用于变量，含义是：如果这个变量被声明赋值了，那就用它声明的值，否则就用这个默认值。
```
$fancybox-width: 400px !default;
.fancybox {
width: $fancybox-width;
}
```
在上例中，如果用户在导入你的sass局部文件之前声明了一个$fancybox-width变量，那么你的局部文件中对$fancybox-width赋值400px的操作就无效。如果用户没有做这样的声明，则$fancybox-width将默认为400px。

3. 嵌套导入;
sass允许@import命令写在css规则内。这种导入方式下，生成对应的css文件时，局部文件会被直接插入到css规则内导入它的地方。
举例说明，有一个名为_blue-theme.scss的局部文件，内容如下：
```
aside {
  background: blue;
  color: white;
}
// 然后把它导入到一个CSS规则内，如下所示：

.blue-theme {@import "blue-theme"}

// 生成的结果跟你直接在.blue-theme选择器内写_blue-theme.scss文件的内容完全一样。

.blue-theme {
  aside {
    background: blue;
    color: #fff;
  }
}
```
被导入的局部文件中定义的所有变量和混合器，也会在这个规则范围内生效。这些变量和混合器不会全局有效，这样我们就可以通过嵌套导入只对站点中某一特定区域运用某种颜色主题或其他通过变量配置的样式。

4. 原生的CSS导入;
在下列三种情况下会生成原生的CSS@import，尽管这会造成浏览器解析css时的额外下载：
- 被导入文件的名字以.css结尾；
- 被导入文件的名字是一个URL地址（比如http://www.sass.hk/css/css.css），由此可用谷歌字体API提供的相应服务；
- 被导入文件的名字是CSS的url()值。
> 这就是说，你不能用sass的@import直接导入一个原始的css文件，因为sass会认为你想用css原生的@import。但是，因为sass的语法完全兼容css，所以你可以把原始的css文件改名为.scss后缀，即可直接导入了。

#### 4. 静默注释;
```
body {
  color: #333; // 这种注释内容不会出现在生成的css文件中
  padding: 0; /* 这种注释内容会出现在生成的css文件中 */
}
```

####5. 混合器;
混合器使用@mixin标识符定义，实现大段样式的重用
```
@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
```
然后就可以在你的样式表中通过@include来使用这个混合器。@include调用会把混合器中的所有样式提取出来放在@include被调用的地方。
```
notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}

//sass最终生成：
.notice {
  background-color: green;
  border: 2px solid #00aa00;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
```
1. 何时使用混合器;
判断一组属性是否应该组合成一个混合器，一条经验法则就是你能否为这个混合器想出一个好的名字。

2. 混合器中的CSS规则;
```
@mixin no-bullets {
  list-style: none;
  li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
  }
}
ul.plain {
  color: #444;
  @include no-bullets;
}
ul.plain {
  color: #444;
  list-style: none;
}
ul.plain li {
  list-style-image: none;
  list-style-type: none;
  margin-left: 0px;
}
```

3. 给混合器传参;
```
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
a {
  @include link-colors(blue, red, green);
}

//Sass最终生成的是：

a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }
// sass允许通过语法$name: value的形式指定每个参数的值。这种形式的传参，参数顺序就不必再在乎了，只需要保证没有漏掉参数即可：

a {
    @include link-colors(
      $normal: blue,
      $visited: green,
      $hover: red
  );
}
```

4. 默认参数值;
为了在@include混合器时不必传入所有的参数，我们可以给参数指定一个默认值。参数默认值使用$name: default-value的声明形式，默认值可以是任何有效的css属性值，甚至是其他参数的引用，如下代码：
```
@mixin link-colors($normal,$hover: $normal,$visited: $normal){
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
```
如果像下边这样调用：@include link-colors(red) $hover和$visited也会被自动赋值为red。

#### 6. 使用选择器继承来精简CSS;
选择器继承是说一个选择器可以继承为另一个选择器定义的所有样式。这个通过@extend语法实现，如下代码:
```
//通过选择器继承继承样式
.error {
  border: 1px solid red;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```
在上边的代码中，.seriousError将会继承样式表中任何位置处为.error定义的所有样式。以class="seriousError" 修饰的html元素最终的展示效果就好像是class="seriousError error"。相关元素不仅会拥有一个3px宽的边框，而且这个边框将变成红色的，这个元素同时还会有一个浅红色的背景，因为这些都是在.error里边定义的样式。.seriousError不仅会继承.error自身的所有样式，任何跟.error有关的组合选择器样式也会被.seriousError以组合选择器的形式继承，如下代码:
```
//.seriousError从.error继承样式
.error a{  //应用到.seriousError a
  color: red;
  font-weight: 100;
}
h1.error { //应用到hl.seriousError
  font-size: 1.2rem;
}
```
如上所示，在class="seriousError"的html元素内的超链接也会变成红色和粗体。

1. 何时使用继承;
当一个元素拥有的类（比如说.seriousError）表明它属于另一个类（比如说.error），这时使用继承再合适不过了。

2. 继承的高级用法;
假如一条样式规则继承了一个复杂的选择器，那么它只会继承这个复杂选择器命中的元素所应用的样式。举例来说， 如果.seriousError@extend.important.error ， 那么.important.error 和h1.important.error 的样式都会被.seriousError继承， 但是.important或者.error下的样式则不会被继承。这种情况下你很可能希望.seriousError能够分别继承.important或者.error下的样式。如果一个选择器序列（#main .seriousError）@extend另一个选择器（.error），那么只有完全匹配#main .seriousError这个选择器的元素才会继承.error的样式，就像单个类 名继承那样。拥有class="seriousError"的#main元素之外的元素不会受到影响。像#main .error这种选择器序列是不能被继承的。这是因为从#main .error中继承的样式一般情况下会跟直接从.error中继承的样式基本一致，细微的区别往往使人迷惑。

3. 继承的工作细节;
4. 使用继承的最佳实践;
值得一提的是，只要你想，你完全可以放心地继承有后代选择器修饰规则的选择器，不管后代选择器多长，但有一个前提就是，不要用后代选择器去继承。

> 参考自https://www.sass.hk
