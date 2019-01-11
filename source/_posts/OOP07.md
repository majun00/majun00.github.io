---
title: OOP-(7)--补充知识
tags: 
- 面向对象
categories:
- JS
- JS_OOP
---

## 其他补充知识

### bind绑定函数调用的对象实现方法调用

> 使用 `函数.bind(对象)` 会返回一个函数,调用返回的函数,实现 对象调用方法 的效果

	function fn(){
 		 console.log(this);
	};

	var o = {
	  name:'zs',
	};

	var f = fn.bind(o); // 给函数fn绑定了对象

	f(); // 虽然是函数调用模式,但是打印的this是绑定的对象,实现了函数的方法调用模式



### Object.prototype成员介绍

- `constructor`:指向构造函数
- `hasOwnProperty`:判断属性是否为对象自己所拥有(即不包括原型链上的)

		function Person () {
			this.name = 'zs';
		}
		Person.prototype.age = 19;
	
		var p = new Person();
	
		console.log( p.hasOwnProperty( 'name' ) ); // p 是否含有 name 属性 name
	
		console.log( p.hasOwnProperty( 'age' ) ); // p 是否含有 age 属性 false


- `propertyIsEnumerable`:判断对象的属性是否可以枚举,不包括原型链上的属性


- `o1.isPrototypeOf(o2)`:判断对象o1是否是对象o2的原型对象

		function Person () {
	    	this.name = 'zs';
		}
		Person.prototype.age = 19;
		
		var p = new Person();
		
		console.log(Person.prototype.isPrototypeOf(p));


### 包装对象

- 三种基本包装类型: `Number` `String` `Boolean`

- 包装类型出现的目的:在开发中常常会使用基本数据类型, 但是基本数据类型没有方法, 因此 js 引擎会在需要的时候**自动**的将基本类型转换成对象类型.

- **实现原理**: 基本类型调用方法时,解释器首先将基本类型转换成对应的对象类型, 然后调用方法. 
	方法执行结束后, 这个对象就被立刻回收
	
- 在 apply 和 call 调用的时候, 也会有转换发生. 上下文调用的第一个参数必须是对象. 如果传递的是数字就会自动转换成对应的包装类型



### getter和setter读写器

> getter和setter读写器本质是一种**语法糖**: 为了方便开发而给出的语法结构


	// 基本语法 使用get 和 set 关键字
	var o = (function () {
		var num = 123;
		return {
				
			// get 名字 () { 逻辑体 }
			get num () {
				return num;
			}

			// set 名字 ( v ) { 逻辑体 }
			set num ( v ) {
				num = v;
			}
		};
	})();

- 闭包原始结构

		var o = (function () {
			var num = 123;
			return {
				get_num: function () {
					return num;
				},
				set_num: function ( v ) {
					num = v;
				}
			};
		})();
	
		// 获得数据
		o.get_num();			
	
		// 设置
		o.set_num( 456 );

- 使用读写器改写

		var o = (function () {
			var num = 13;
			return {					
				get num () {
					console.log( '执行 getter 读写器了' );
					return num;
				},
	
				set num ( v ) {
					console.log( '执行 setter 读写器了' );
					if ( v < 0 || v > 150 ) {
						console.log( '赋值超出范围, 不成功 ' );
						return;
					}	
					num = v;
				}
			};
		})();

		// 获取数据
		console.log( o.num );
	
		// 设置数据
		o.num = 33;
	
		console.log( o.num );
