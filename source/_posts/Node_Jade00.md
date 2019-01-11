---
title: Node模板引擎学习(1)--Jade
tags: 
- NodeJS
- Jade
categories:
- NodeJS
---

# Jade
## 安装：
```
npm install jade –global
```
安装后本地随便新建一个sample.jade文件，执行：
```
jade sample.jade
```
就能将其翻译成标准的sample.html源文件了
命令参数：
`-P`（大写，命令参数是大小写敏感的）。Jade默认编译出来的html源文件里是没有缩进的，不便于开发。加上-P参数后，编译出来的html源文件里就有缩进了：
```
jade -P sample.jade
```
`-w`用来watch监视jade文件，每次改动保存后自动编译成html文件，省去手动执行命令的麻烦：
```
jade -P -w sample.jade
```
`-O`用来给jade文件传递对象或JSON文件，用以替换模板内的变量：
```
jade -P -w sample.jade -O sample.json
```

### 标签和属性
省略尖括号，直接写标签名。标签间的嵌套关系用换行加空格来实现。紧接在标签名后加上.xx或#xx，就能给标签添加css类名和id。

标签名后第一个空格后面的内容会被编译成标签内的文本内容

标签属性的正统写法应该是写入()括号内

### 多行文本

在每行前加上|竖线符

用这种写法，可以用Jade语法来嵌套标签, 不仅可用于p标签等，也常见于style和script标签 

```
p
  span 第1行文本
  | 第2行文本
  | 第3行文本
  | 第4行文本

//编译出来的结果
<p><span>第1行文本</span>第2行文本
  第3行文本
  第4行文本
<p>
```

```
script.
  console.log("open mind");
  console.log("learning quick");
  console.log("work hard");
```

### 变量
变量声明很简单，前面加上-横杠。使用变量只要#{变量名}就行了。例如：

```
- var cs = 'UTF-8'
meta(charset='#{cs}')

//编译出来的结果
<meta charset="UTF-8">
```

```
input(value='#{aaa}')
input(value=aaa)

//编译出来的结果
<input value="undefined">
<input>
```

如果不想HTML转码，可以将#改成!叹号

可以前面加\反斜杠来让Jade引擎不编译变量

可以在标签后面紧接=等号（不转义用!=）来输出变量

可以看出用#{}如果变量未定义，将会编译成undefined作为初始值。但用=等号来编译变量的话，如果变量未定义就忽略

有了变量就能轻松实现前后端分离。数据保存在JSON文件里。前端用Jade模板制作页面，在需要显示数据处用变量来实现。例如sample.json文件里：
```
{
  "charset": "UTF-8",
  "title": "My First Jade Page"
}
```
sample.jade文件里：
```
doctype html
html
  head
    meta(charset='#{charset}')
  body
    h1.titleClass#titleID #{title}
```
执行命令：`jade -P -w sample.jade -O sample.json`后Jade文件里的变量被自动替换。编译出来的sample.html：
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
  </head>
  <body>
    <h1 id="titleID" class="titleClass">My First Jade Page</h1>
  </body>
</html>
```

### 语句
*   if-else
*   unless
*   case-when
*   for-in
*   each-in
*   while
```
- var author = 'Jack';
if author
  p 作者：#{author}
else
  p 无作者

//编译出来的结果
<p>作者：Jack</p>
```
Jade还支持unless语句，它是if-else的反向，写法都一样，用的不多就不举例了。
Jade里的case-when语句就是JavaScript里的switch-case语句（不知为何…）
```
- var authors = ['Jack', 'Bill'];case authors[0]
  when 'Jack'
    p 作者是Jack
  when 'Bill'
    p 作者是Bill
  default
    p 无作者

//编译出来的结果
<p>作者是Jack</p>
```
循环遍历用for-in（注意上面的if-else，case-when语句前不用像变量那样加上-横杠，但for的前面要加上-横杠。如果漏写-横杠，会被解析为标签）：
```
- var person = {name:'Jack', gender: 'Male'}
- for (var prop in person)
  p= person[prop]

//编译出来的结果
<p>Jack</p>
<p>Male</p>
```
```
循环遍历也可以用each-in（each前的-横杠加不加均可）：- var employee = {name:'Jack', gender: 'male'}
- each value, key in person
  p #{key}: #{value}
- var language = ['Java', 'JavaScript', 'C++']
ul
  each item in language
    li #{item}

//编译出来的结果
<p>name: Jack</p>
<p>gender: male</p>
<ul>
  <li>Java</li>
  <li>JavaScript</li>
  <li>C++</li>
</ul>
```
循环遍历也可以用while（同样前面加不加-横杠均可）：
```
- var n = 0
ul
  while n < 4
    li= n++

//编译出来的结果
<ul>
  <li>0</li>
  <li>1</li>
  <li>2</li>
  <li>3</li>
</ul>
```

### Mixin
```
mixin personInfo(name, hobbies)
  p #{name}'s hobbies:
    ul.hobby
      each hobby in hobbies
        li= hobby
+personInfo('Jack', ['movie', 'music'])

//编译出来的结果
<p>Jack's hobbies:</p>
<ul class="hobby">
  <li>movie</li>
  <li>music</li>
</ul>
```

### 模板
Jade用block和extends来实现模板的继承

block真正的作用在于占位，供子文件继承，可以理解为传统OO语言里的虚函数。父文件里定义的block，子文件里用extends来继承并重写

例如每个文件的页头都一样，就body里内容不一样，可以写一个header.jade：
```
doctype html
html
  head
    meta(charset='#{charset}')
    block scripts
      script(src='jquery.js')
      script(src='underscore.js')
      script(src='backbone.js')
  body
    block content
      p please write content
```
每个页面的header都长这样。而body里定义了block content，这里block可以理解为一个占位符placeholder，需要继承它的文件重写block content。根据业务需求写页面主体部分，例如sample.jade改成这样：
```
extends header

block content
  h1.titleClass#titleID #{title}
  a(href='http://www.jackzxl.net', target='_blank') 我的主页
  ……
```
在sample.jade里，开头用extends表明和header.jade的继承关系。然后根据业务重写header.jade里的block content。

(执行`jade -P -w sample.jade -O sample.json`就能看到和之前一模一样的页面。引擎加载流程是：解析sample.jade，发现开头有extends，就去解析header.jade，将其编译成html。此时html里的body里是`<p>please write content</p>`。解析完header.jade就继续解析sample.jade，发现block content，于是会将定义在header.jade里的block content替换掉。最终输出的是正确的页面内容，而不是`<p>please write content</p>`。（但如果你执行的是`jade -P -w header.jade -O sample.json`会发现body里内容为`<p>please write content</p>`）)

除继承外还可以用include包含。Include会将内容无脑全拷贝进去。例如上面的sample.jade第一行extends header改成include header。

与extends不同，include就是无脑将另一个文件里的内容直接拷贝进去，不像block + extends可以重写block。 

注意：include包括extends如果省略后缀名，Jade默认该文件时.jade会进行编译。但如果另一个文件里写的是原生的html，需要写明后缀名为.html（例如include xx.html），明确告诉Jade不要编译。

### 注释
Jade里加上//就能添加注释，用双斜杠的注释会被输出到html源码里。

如果不想在html源码里输出注释，用//-，在双斜杠后加一横杠。

html里还可以写注释型的条件语句，常用于兼容IE。Jade里你同样可以写这些条件语句，例如将上面header.jade改成能识别IE89，应用不同的class：
```
doctype html
<!--[if IE 8]><html class='ie8'><![endif]-->
<!--[if IE 9]><html class='ie9'><![endif]-->
<!--[if !IE]><!--><html><!--<![endif]-->
head
  meta(charset='#{charset}')
  block scripts
    script(src='jquery.js')
    script(src='underscore.js')
    script(src='backbone.js')
body
  block content
  p please write content
</html>
```
上面因为有了条件语句，所以html标签用尖括号括了起来，因此最下面要手动加上来闭合标签。而且Jade是空格缩进敏感的，需要将原先的head和body包括里面内容，全往前缩进2个空格。

### 块注释
块注释也是支持的：
```
body
  //
    #content
      h1 Example
```
渲染为：
```
<body>
  <!--
  <div id="content">
    <h1>Example</h1>
  </div>
  -->
</body>
```
Jade 同样很好的支持了条件注释：
```
body
  //if IE
    a(href='http://www.mozilla.com/en-US/firefox/') Get Firefox
```
渲染为：
```
<body>
  <!--[if IE]>
    <a href="http://www.mozilla.com/en-US/firefox/">Get Firefox</a>
  <![endif]-->
</body>
```

### 过滤器
Jade同样兼容其他模块，例如写博客爱用的markdown，写css爱用的less，还有coffeeScript等。只要npm安装好后，用:冒号+模块名就能声明使用这些模块，例如：
```
:markdown
  ...
:less
  ...
:coffee
 ...
```
