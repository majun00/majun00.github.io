---
title: Node模板引擎学习(3)--EJS
tags: 
- NodeJS
- EJS
categories:
- NodeJS
---

> jade虽然简洁受很多大神推崇并且也是官方推荐, 但我个人觉得可读性太差了, 个人倾向朴实的EJS,这里两者都进行学习,至于Handlebars,Swig如果公司要用再学习吧

# EJS
> EJS几乎不要学习成本, 举两个例子差不多就能用了,如下

```
<% if (user) { %>
  <h2><%= user.name %></h2>
<% } %>
```
```
<ul>  
    <% for(var i=0; i<supplies.length; i++) { %>  
        <li>  
            <a href='supplies/<%= supplies[i] %>'>  
                <%= supplies[i] %>  
            </a>  
        </li>  
    <% } %>  
</ul>
```
注意 :
`<%` Scriptlet' 标签, 用于控制流，没有输出

`<%=` 向模板输出值（带有转义）

`<%-` 向模板输出没有转义的值

`<%#` 注释标签，不执行，也没有输出

`<%%` 输出字面的 '<%'

`%>` 普通的结束标签

`-%>` Trim-mode ('newline slurp') 标签, 移除随后的换行符

## 在Express中设置模板引擎
```
app.set('views', './views');  // 指定模板文件存放位置
```
```
app.set('view engine', 'jade')  // 设置默认的模板引擎
```
```
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');
```
```
app.engine( 'ejs', require('ejs')._express )
```
注意： _express函数是许多模板引擎提供的回调函数。但是这个函数只能在默认的文件扩展名上工作。但是，有种情况我们使用的不是对应模板引擎的扩展名的怎么办呢？这时不能再调用_express函数。在这种情况下我们可以使用一个替代的函数，例如： 在EJS中提供了renderFile函数来完成相同的功能。
```
app.engine( '.html', require('ejs').renderFile );  // 注册html模板引擎 
app.set('view engine', 'html');  // 将模板引擎换成html
```
```
app.engine('.html',ejs.__express)
app.set('view engine','html)
```

