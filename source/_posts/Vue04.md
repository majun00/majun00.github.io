---
title: Vue-(4)--自定义指令与过滤器
tags: 
- Vue
categories:
- Vue
---

## 过滤器

```
<!-- 在两个大括号中 -->
{{ message | capitalize }}

<!-- 在 v-bind 指令中 -->
<div v-bind:id="rawId | formatId"></div>
```

```
<div id="app">
  {{ message | capitalize }}
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    message: 'runoob'
  },
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
})
</script>
```

过滤器可以串联：
```
{{ message | filterA | filterB }}
```

过滤器是 JavaScript 函数，因此可以接受参数：
```
{{ message | filterA('arg1', arg2) }}
```

这里，message 是第一个参数，字符串 'arg1' 将传给过滤器作为第二个参数， arg2 表达式的值将被求值然后传给过滤器作为第三个参数。

### 自定义过滤器

#### 自定义私有过滤器

```
new Vue({

  el:'#app',  

  filters:{       

    '过滤器名称':function(管道符号|左边参数的值,参数1,参数2,....) {

    return 对管道符号|左边参数的值做处理以后的值

    })    

  }

});
```
```javascript

new Vue({

    el:'#app',

    data:{

        time:new Date()

    },

    filters:{

    //定义在 VM中的filters对象中的所有过滤器都是私有过滤器,这种过滤器只有在当前vue对象el指定的监管的区域有用

        datefmt:function(input,splicchar){

            // input是自定义过滤器的默认参数，input的值永远都是取自于 | 左边的内容

            var date = new Date(input); 

            var year = date.getFullYear();

            var m = date.getMonth() + 1;

            var d = date.getDate();         

            var fmtStr = year+splicchar+m +splicchar+d;

            return fmtStr;

        }

    }

});

```

```html

<div id="app">{{ time | datefmt('...') }}</div>

```

#### 自定义全局过滤器

- 可以用全局方法 Vue.filter() 注册一个全局自定义过滤器，它接收两个参数：过滤器ID 和过滤器函数。过滤器函数以值为参数，返回转换后的值

- 定义格式：
```
Vue.filter('过滤器名称', function (管道符号|左边参数的值,其他参数1,> 其他参数2,....) {

  return 对管道符号|左边参数的值做处理以后的值

})
```

- 使用方法

Vue2.0
```
<span>{{ msg | 过滤器名称('参数1' '参数2' ....) }}</span>
```
```javascript

Vue.filter('datefmt',function(input,splicchar){

    var date = new Date(input); 

    var year = date.getFullYear();

    var m = date.getMonth() + 1;

    var d = date.getDate();         

    var fmtStr = year+splicchar+m +splicchar+d;

    return fmtStr;

});    

new Vue({

    el:'#app',

    data:{

        time:new Date()

    }

});      

```

```html        

<div id="app">{{ time | datefmt('-') }}</div>

```

## 自定义指令

除了默认设置的核心指令( v-model 和 v-show ), Vue 也允许注册自定义指令。

下面我们注册一个全局指令 v-focus, 该指令的功能是在页面加载时，元素获得焦点：

```
<div id="app">
    <p>页面载入时，input 元素自动获取焦点：</p>
    <input v-focus>
</div>
 
<script>
// 注册一个全局自定义指令 v-focus
Vue.directive('focus', {
  // 当绑定元素插入到 DOM 中。
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})
// 创建根实例
new Vue({
  el: '#app'
})
</script>
```

我们也可以在实例使用 directives 选项来注册局部指令，这样指令只能在这个实例中使用：
```
<div id="app">
  <p>页面载入时，input 元素自动获取焦点：</p>
  <input v-focus>
</div>
 
<script>
// 创建根实例
new Vue({
  el: '#app',
  directives: {
    // 注册一个局部的自定义指令 v-focus
    focus: {
      // 指令的定义
      inserted: function (el) {
        // 聚焦元素
        el.focus()
      }
    }
  }
})
</script>
```

### 自定义元素指令(Vue1.0)

### 自定义属性指令

- 定义指令：
```
Vue.directive('指令ID，不需要增加v-前缀',function(){

    //实现指令的业务

    this.el //代表使用这个指令的元素对象

});
```
- 使用指令(当做一个元素的属性使用)：<input type="text" v-指令ID />

```javascript

Vue.directive('focus',function(){

    this.el.focus(); 

});

```

```html    

<input type="text" v-focus />

```

```javascript

Vue.directive('color', {

    bind: function (el,binding) {

        // 自定义指令所在的元素对象是el

        // 自定义指令后面的表达式是binding.expression,它的值是 binding.value

        el.style.color = binding.value;

    }

});

```

```html    

<input type="text" v-color="color1" />

```
