---
title: OOP-(1)--基础知识回顾
tags: 
- 面向对象
categories:
- JS
- JS_OOP
---

## JavaScript-多范式编程语言

> js是一个基于对象的多范式编程语言

### 多范式编程

- 面向过程
- 面向对象
- 函数式(链式编程):以点语法为基本规则

## 面向对象基本概念

### 面向过程

- 一步一步 前因后果

### 面向对象

- 只求结果 不管实现过程,实际上就是对面向过程中的每一步进行封装

### 面向对象三大特征

- **封装**:将面向过程每一步进行推进:对同种对象的属性和方法进行抽象，成为一个**类**(js中没有类的概念，实际上也是一个对象)，然后通过**类**的方法和属性来访问**类**
- **继承**:在封装的基础上，将同类事物继续抽象，继承时，子类拥有父类的属性和方法，同时拥有自己特有的属性和方法
- **多态**:不同对象对同一事物做出不同的回应，通常出现在继承后对方法的重写

> 封装 继承 多态 都是对具体事务的**抽象** 

> **抽象**是实现面向对象编程的基本思想

> **抽象**抽取事务主要特征对事务进行描述

## 什么是对象

> 万物皆对象

### 数据集

- 很多数据放到一起:例如数组中的数据，对象中的数据 都是将零散的数据放到一个数据集中,这个数据集就是指数组这个对象以及Object这个对象
 
### 功能集

- 函数集&方法集:重复的代码封装到函数中，将所有的函数放到一个对象中成为对象的方法，此对象就可以看做是一个函数集

## 数据类型

### 基本数据类型(值类型)

	// 基本数据类型
	string number boolean
	
	// 检测基本数据类型
	var str = “123”;
	console.log(typeof str);// string

### 复杂数据类型（引用数据类型）

	// Object(数组，时间，函数，正则...)
	
	// 检测复杂数据类型
	1.使用typeof不能检测到真实类型(函数例外)
	
	var arr = [ 1, 2, 3 ];
	var fn = function () {};
	var o = { name: 'itcast' };

	console.log( typeof arr ); // object
	console.log( typeof fn ); // function
	console.log( typeof o ); // object
	console.log( typeof new Date() ); // object
	
	2.使用Object.prototype.tostring.apply()
	
	console.log(  Object.prototype.toString.apply( arr ) );//[object Array]
	console.log(  Object.prototype.toString.apply( fn ) ); //[object Function]
	console.log(  Object.prototype.toString.apply( o ) ); // [object Object]
	console.log(  Object.prototype.toString.apply( new Date() ) );// [object Date]

	console.log(  Object.prototype.toString.apply( null ) );// [object Null]
	console.log(  Object.prototype.toString.apply( undefined ) ); // [object Undefined]


### 空类型

	null undefined

## 引用类型的拷贝

	var p = {
		name: '张三',
		age: 19,
		gender: '男',
		clone: function () {
			// 目的是为了完成对自己的拷贝
			var temp = {};
			temp.name = this.name;
			temp.age = this.age;
			temp.gender = this.gender;
			temp.clone = this.clone;
			return temp;
		}
	};

	var p1 = p.clone();

	p.name = '李四';

	var p2 = p1.clone();
	p1.name = '王五';

### 浅拷贝

- 拷贝引用:如果对象的属性也是一个引用类型,拷贝的时候不重新创建一个新的对象来实现该属性的拷贝,那么就是浅拷贝
- 对象的直接赋值就是浅拷贝

### 深拷贝

- 拷贝数据:将两个对象完全从内存中隔离开,就是深拷贝。即每一个引用属性,以及引用属性的引用属性,全部拷贝出来.
- 遍历对象的每一项数据,实现复制就是深拷贝


## 构造函数(构造器)


### 对象的动态特性

- js中的对象如果没有设置指定的属性,只需要利用赋值就可以给该对象提供该属性

		var o = {}; // 没有添加任何属性的对象
		o.name = "zxx"; // 赋值时动态添加了name属性
		
### 点语法&关联数组语法

- 点语法

		var o = {};
		o.name = 'zxx';// 点语法赋值
		console.log(o.name);// 点语法取值
		
- 关联数组语法

		var o = {};
		o['name'] = 'zxx'; // 关联数组语法赋值
		console.log(o['name']);// 关联数组语法取值
		
> name这个变量尽量少用,系统中存在一个变量叫name,有可能会冲突
		

### 工厂函数模式

	function Person(name,age,gender){
		var p = {};
		p.name = name;
		p.age = age;
		p.gender = gender;
		return p; 
	}

	var p1 = Person('zs',18,'男');
	var p2 = Person('ls',16,'女');

### 构造函数模式

	// 构造器定义:1.不需要再函数内部显示创建对象 2.属性和方法绑定到this上 3.不需要return
	function Person(name,age,gender){
		this.name = name;
		this.age = age;
		this.gender = gender;
	}
	
	// 构造器创建对象 使用 new 创建对象
	var p = new Person(‘zs’,18,'男');

## 异常与捕获

### 常见异常

	Uncaught ReferenceError 未捕获的引用错误
	Uncaught TypeError 未捕获的类型错误
	Uncaught SyntaxError 未捕获的语法错误

### 异常捕获语法
	
	try{
		// 正常执行的代码
	}catch(e){
		// 出现异常执行
	}
	
	********
	try{
		// 正常执行的代码
	}catch(e){
		// 出现异常执行
	}finally{
		// 无论是否出现异常 try-catch执行完毕 都会执行这里面的代码
		// 一般用于释放资源
	}

### 自定义抛出异常

- 语法: `throw new Error( 错误的消息 )`

		console.log( '123' );
		try {
			var e1 = new Error( '用户自己定义的异常对象' );
			throw e1;
		} catch ( e ) {
			console.log( e1 === e );
		}
		console.log( '456' );


## DOM操作复习

### 获取元素
#### 获取指定元素

**`document.getElementById`: 通过id寻找一个元素（找到的是一个元素对象） 该方法只能被document对象调用（同一个文档中id不能重复）**

	<div id="box"></div>
	var box = document.getElementById(“box”);

**`document.getElementsByTagName`: 通过标签名寻找一类元素（找到的是由元素对象组成的伪数组,既可以被document调用，又可以被元素对象调用，被元素对象调时表示在该元素对象内部执行查找**

	<div class="cl" id=“cl”>
        <div class="cl2"></div>
        <div class="cl2"></div>
    </div>
    <div class="cl"></div>
    <div class="cl"></div>

    var divs = document.getElementsByTagName("div");// 获取页面上所有的div,divs是一个伪数组
    var cl = document.getElementById("cl");// 获取id为cl的元素
    var cl2s = cl.getElementsByTagName("div");// 获取cl元素下面所有的div标签,cl2s是一个伪数组
    
    
**`document.getElementsByClassName`: 通过类名寻找包含这个类名的元素**

	 <div class="cl main" id=“cl”>
        <p class="cl fl"></div>
        <span class="cl"></div>
    </div>
    <a class="cl"></a>

    var cls = document.getElementsByClassName("cl");//获取到的是一个伪数组,里面装的是div p span a这四个元素对象(只要标签中的class中有传入的类名就能获取到)
    
#### 获取子元素&兄弟元素&父元素

**`element.childNodes`: 获取指定元素的子节点,包括文本节点、元素节点等**
**`element.children`: 获取指定元素的子元素,只会获取元素节点**
**`element.nextSibling`: 获取指定元素的下一个兄弟节点**
**`element.nextElementSibling`: 获取指定元素的下一个兄弟元素**
**`element.previousSibling`: 获取指定元素的上一个兄弟节点**
**`element.previousElementSibling`: 获取指定元素的上一个兄弟元素**
**`element.parentNode`: 获取元素的父节点,父节点肯定是一个元素**

	<ul id="list">
        <li id="firstlink"><a href="javascript:void(0)">首页</a></li>
        <li><a href="javascript:void(0)">播客</a></li>
        <li><a href="javascript:void(0)">博客</a></li>
        <li><a href="javascript:void(0)">相册</a></li>
        <li><a href="javascript:void(0)">关于</a></li>
        <li id="lastlink"><a href="javascript:void(0)">帮助</a></li>

	</ul>

	// 通过id获取元素对象
	var ul = document.getElementById("list");
	// 通过标签名获取元素数组
	var lis = ul.getElementsByTagName("li");
	// 不关心层级 只找指定标签
	// 缺点:如果内部还有li 也会找到
	
	var nodes = ul.childNodes;
	//子节点 只找子级
	//缺点:除了我们想要的元素节点 还会获取到其他节点
	
	var children = ul.children;//只获取ul下的子元素
	
	var link = document.getElementById("firstlink");
	var nextNodeSibling = link.nextSibling; // 获取到的是换行(文本节点)
	var nextElementSibling = link.nextElementSibling;//获取到的是下一个li标签
	
	var parentNode = link.parentNode;// 获取到的是ul

#### 获取第一&最后一个子元素

**`element.firstChild`: 获取指定元素下面的第一个子节点**
**`element.firstElementChild`: 获取指定元素下面的第一个子元素**
**`element.lastChild`: 获取指定元素下面的最后一个子节点**
**`element.lastElementChild`: 获取指定元素下面的最后一个子元素**

	var firstNode = ul.firstChild;// 获取到的是换行(文本节点)
	var firstElement = ul.firstElementChild;// 获取到的是firstlink
	var lastNode = ul.lastChild;// 获取到的是换行(文本节点)
	var lastElement = ul.lastElementChild;// 获取到的是lastlink

### 节点操作

#### 克隆节点 - cloneNode()

- **`element.cloneNode()`: 复制element节点**
- **参数:布尔值**
	- **true代表深层克隆,把当前节点和内部所有节点都复制一份**
	- **false代表浅层克隆,只复制当前节点**

			<div id="father">
	    	<div id="son"><div/>
			</div>

			var father = document.getElementById("father");
			var son = document.getElementById("son");
			var clone = son.cloneNode(true);// 把son这个div复制一份 复制出来的clone和son没有任何关系了
	
#### 添加节点 - appendChild()

**`father.appendChild(son)`:将son节点追加到father内部的最后位置**

	<div id="father">
   	<div id="son"><div/>
	</div>
	<div id="demo"></div>

	var father = document.getElementById("father");
	var demo = document.getElementById("demo");
	var clone = demo.cloneNode(true);// 将demo克隆一份 
	father.appendChild(clone);// 将克隆出来的clone追加到father中

	// 此时页面结构应该为
	<div id="father">
	    <div id="son"><div/>
	    <div id="demo"></div>
	</div>
	<div id="demo"></div>
	
	/*追加克隆节点对原节点不会产生影响*/
	
	//如果代码如下 则会将demo节点直接移动到father节点下
	father.appendChild(demo);// demo是页面上存在的节点
	
	// 此时页面结构应该为
	<div id="father">
	    <div id="son"><div/>
	    <div id="demo"></div>
	</div>

#### 插入节点 - insertBefore()

**`father.inserBefore(son1,son2)`: 将son1插入到father节点下的son2前面**

	<div id="father">
   	<div id="son"><div/>
	</div>
	<div id="demo"></div>

	var father = document.getElementById("father");
	var son = document.getElementById("son");
	var demo = document.getElementById("demo");
	
	father.inserBefore(son,demo);//会直接将demo节点移动到father下的son前面       
	
	/*插入克隆出来的节点也不会对原节点产生影响*/
	
#### 移除节点 - removeChild()

**`father.removeChild(son)`: 将father下的son节点移除**

	<div id="father">
  		<div id="son"><div/>
	</div>

	var father = document.getElementById("father");
	var son = document.getElementById("son");
	
	father.removeChild(son);// 直接将son节点删除

### 创建结构

#### document.write()

**`document.write("内容")` 特点:只能被document调用,而且会覆盖页面上原有内容**

	document.write("<a href="http://www.baidu.com">百度</a>")
	// 可以在页面上创建一个a标签,而且会覆盖页面上原有的所有内容
	
#### element.innerHtml

**`element.innerHtml`特点:往页面添加html标签,可以限定范围**

	<div id="box"></div>

    var box = document.getElementById("box");
    box.innerHtml = "<a href="http://www.baidu.com">百度</a>";

    // 追加后的结构为
    <div id="box">
        <a href="http://www.baidu.com">百度</a>
    </div>
    
#### document.createElement()

**`document.createElement("内容")`特点:动态创建标签,添加到页面需要配合appendChild使用**

	<div id="box"></div>
	var box = document.getElementById("box");
	var input = document.createElement("input");
	input.type = "text";
	box.appendChild(input);
	
### 标签的自定义属性操作

#### setAttribute()

**设置标签属性: `setAttribute()`**

	<div id="box"></div>
	var box = document.getElementById("box");
	box.setAttribute("id","aaa");// 有规定的属性可以设置
	box.setAttribute("bbb","ccc");// 没有规定的属性也可以设置
	
#### getAttribute()

**获取标签属性:`getAttribute()`**

	<div id="box"></div>
	var box = document.getElementById("box");
	box.getAttribute("id");// 有规定的可以获取
	box.getAttribute("aaa"); // 没有规定的也可以获取
	
#### removeAttribute()

**移除标签属性:`removeAttribute()`**

	<div id="box"></div>
	var box = document.getElementById("box");
	box.removeAttribute("id"); // 有规定的可以删除
	box.removeAttribute("aaa"); // 没有规定的也可以删除

### 兼容方法

#### innerText兼容写法

**获取文本**

	function getInnerText(element) {
	    // 能力检测 判断是否有这一属性
	    if (typeof element.innerText === "string") {
	        return element.innerText;
	    } else {
	        return element.textContent;
	    }
	}
	
**设置文本**

	function setInnerText(element, content) {
	    // 能力检测 判断是否有这一属性
	    if (typeof element.innerText === "string") {
	        element.innerText = content;
	    } else {
	        element.textContent = content;
	    }
	}
	
#### 获取上一个&下一个兄弟元素的兼容写法

**获取下一个兄弟元素兼容写法**

	// 获取下一个兄弟元素
	function getNextElement(element){
	    if(element.nextElementSibling){
	        //能找到nextElementSibling这个属性 就可以直接使用
	        return element.nextElementSibling;
	    }else{
	        var next = element.nextSibling;// 获取下一个兄弟节点
	        // 如果next就是想要的下一个兄弟元素 就直接返回 如果不是 就一直找
	        while(next&&next.nodeType !==1){//next找到了而且不是想要的元素节点就继续找
	            next = next.nextSibling;
	        }
	        return next;
	    }
	}
	
**获取上一个兄弟元素兼容写法**

	// 获取上一个兄弟元素
	function getPreviousElement(element){
	    if(element.previousElementSibling){
	        return element.previousElementSibling;
	    }else{
	        var previous = element.previousSibling;
	        while(previous&&previous.nodeType !== 1){
	            previous = element.previousSibling;
	        }
	        return previous;
	    }
	}
	
#### 获取第一个子元素&最后一个子元素的兼容写法

**获取第一个子元素兼容写法**

	// 获取element的第一个子元素
	function getFirstElement(element){
	    // 判断是否支持这一写法
	    if(element.firstElementChild){
	        return element.firstElementChild;
	    }else{
	        // 先找到第一个节点
	        var first = element.firstChild;
	        // 如果这个节点存在而且这个节点不是元素节点
	        while(first&&first.nodeType !== 1){
	            // 从这个节点向后继续找下一个兄弟节点
	            first = first.nextSibling;
	        }
	        return first;
	    }
	}

**获取最后一个子元素兼容写法**

	// 获取element的最后一个子元素
	function getLastElement(element){
	    // 判断是否支持这一写法
	    if(element.lastElementChild){
	        return element.lastElementChild;
	    }else{
	        // 先找到最后一个个节点
	        var last = element.lastChild;
	        // 如果这个节点存在而且这个节点不是元素节点
	        while(last&&last.nodeType !== 1){
	            // 从这个节点向前继续找上一个兄弟节点
	            last = last.previousSibling;
	        }
	        return last;
	    }
	}
	
#### 通过类名获取元素对象的兼容方法

	function getElementsByClassName(element, className) {
        var filterArr = [];
        var elements = element.getElementsByTagName("*");
        for (var i = 0; i < elements.length; i++) {
            var nameArr = elements[i].className.split(" ");
            for (var j = 0; j < nameArr.length; j++) {
                if (nameArr[j] === className) {
                    filterArr.push(elements[i]);
                    break;
                }
            }
        }
        return filterArr;
    }
    
### 事件

	var box = document.getElementById("box");
	
	box.onclick = function(){
	    // 鼠标单击事件
	}
	
	box.dblclick = function(){
	    // 鼠标双击事件
	}
	
	box.onmouseover = function(){
	    // 鼠标移入事件
	}
	
	box.onmouseout = function(){
	    // 鼠标移出事件
	}
	
	
	box.onfocus = function(){
	    // 聚焦事件
	}
	
	box.onblur = function(){
	    // 失去焦点事件
	}
	
	box.onkeyup = function(){
	    // 按键弹起事件
	}
	
	box.onkeydown = function(){
	    // 按键按下事件
	}

