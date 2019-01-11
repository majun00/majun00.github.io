---
title: Vue-(2)--组件
tags: 
- Vue
categories:
- Vue
---

## 组件

```html

<template id="account">

  {{msg}}

  <a href="#" @click="login">登录</a>  |  <a href="#">注册</a>

</template>

<script type="x-template" id="account1">

  <a href="#">登录1</a>  |  <a href="#">注册1</a>

</script>

<div id="app">

  <account></account>

  <login></login>

  <register></register>

</div>

```

```javascript

new Vue({

el:'#app'

})

// 1.0 定义一个全局组件写法1

// 定义组件

var login = Vue.extend({

   template:'<h1>登录页面</>'

 });

// 注册组件

Vue.component('login',login);

// 2.0 定义一个全局组件写法2：

Vue.component('register',{

  template:'<h1>注册页面</>'

});

// 3.0 定义一个账号组件写法3（推荐写法）

Vue.component('account',{

  template:'#account',

  //注意：在组件中使用data定义的是一个方法!而不是一个对象，否则无效果

  data:function(){

    return {

      msg:'账户组件页面'

    }

  },

  methods:{

    login:function(){

      alert('login')

    }

  }

});

// 注意组件里的内容最好用div包裹

// 注意data变成了方法 

```

### 组件中注册子组件

```html

<body>

  <div id="app">

    <account></account>

    <login></login>

  </div>

</body>

<script>

// 定义一个账号组件

Vue.component('account',{

  template:'<div><h1>账号组件</h1><login></login></div>',

  // 在账号组件中定义一个登录的子组件

  components:{

    'login':{

      template:'<h2>登录子组件</h2>'

    }

  }

});

new Vue({

  el:'#app',

  //局部组件

  components:{

    'login':{

      template:'<h2>登录子组件app</h2>'

    }

  }

});

//注意是components

</script>

```

## 父组件传值子组件

```html

<div id="app">

  <sub1 :data1="msg"></sub1>

</div>

<template id="sub">

  <div>{{data1}}</div>

</template>

<!-- 2.0 通过在父组件中调用子组件的时候利用 :绑定对应的props中定义的参数再讲值传递给子组件即可 -->

<script>

// 我们可以认为对象实例是一个根组件

new Vue({

  el:'#app',

  data:{

    msg:100

  },

  components:{

    'sub1':{

      template:'#sub',

      props:['data1'] //1.0 负责接收父组件传入的值

    }

  }

});

</script>

```

## 子组件传值父组件

```html

<div id="app">

  <sub1 v-on:send="getdata"></sub1>

</div>

<template id="sub">

  <button @click="senddata">发送数据给父组件</button>

</template>

<script>

// 我们可以认为对象实例是一个跟组件

new Vue({

  el:'#app',

  methods:{

    getdata:function(input){

      alert(input);

    }

  },

  components:{

    'sub1':{

      template:'#sub',

      methods:{

        senddata:function(){

          // 将hello传值给父组件

          this.$emit('send','hello');

        }

      }

    }

  }

});

</script>

```

### 组件中利用component中的is来实现组件切换

```html

<body>

  <div id="app">

    <a href="#" @click="cname='login'">登录</a> | 

    <a href="#" @click="cname='register'">注册</a>

    <!-- 利用component标签中的 :is 参数来进行组件的切换 -->

    <component :is="cname"></component>

  </div>

</body>

<script>

// 注册全局的登录组件和注册组件

Vue.component('login',{

  template:'登录页面'

});

Vue.component('register',{

  template:'注册页面'

});

new Vue({

  el:'#app',

  data:{

    cname:'login'

  }

});

</script>

```

### prop

prop 是父组件用来传递数据的一个自定义属性。

父组件的数据需要通过 props 把数据传给子组件，子组件需要显式地用 props 选项声明 "prop"：

```
<div id="app">
    <child message="hello!"></child>
</div>
 
<script>
// 注册
Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 同样也可以在 vm 实例中像 "this.message" 这样使用
  template: '<span>{{ message }}</span>'
})
// 创建根实例
new Vue({
  el: '#app'
})
</script>
```

### 动态prop

类似于用 v-bind 绑定 HTML 特性到一个表达式，也可以用 v-bind 动态绑定 props 的值到父组件的数据中。每当父组件的数据变化时，该变化也会传导给子组件：

```
<div id="app">
    <div>
      <input v-model="parentMsg">
      <br>
      <child v-bind:message="parentMsg"></child>
    </div>
</div>
 
<script>
// 注册
Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 同样也可以在 vm 实例中像 "this.message" 这样使用
  template: '<span>{{ message }}</span>'
})
// 创建根实例
new Vue({
  el: '#app',
  data: {
    parentMsg: '父组件内容'
  }
})
</script>
```

以下实例中将 v-bind 指令将 todo 传到每一个重复的组件中：

```
<div id="app">
    <ol>
    <todo-item v-for="item in sites" v-bind:todo="item"></todo-item>
      </ol>
</div>
 
<script>
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
new Vue({
  el: '#app',
  data: {
    sites: [
      { text: 'Runoob' },
      { text: 'Google' },
      { text: 'Taobao' }
    ]
  }
})
</script>
```

注意: prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是不会反过来。

### prop验证

组件可以为 props 指定验证要求。

prop 是一个对象而不是字符串数组时，它包含验证要求：

```
Vue.component('example', {
  props: {
    // 基础类型检测 （`null` 意思是任何类型都可以）
    propA: Number,
    // 多种类型
    propB: [String, Number],
    // 必传且是字符串
    propC: {
      type: String,
      required: true
    },
    // 数字，有默认值
    propD: {
      type: Number,
      default: 100
    },
    // 数组／对象的默认值应当由一个工厂函数返回
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        return value > 10
      }
    }
  }
})
```

type 可以是下面原生构造器：

*   String
*   Number
*   Boolean
*   Function
*   Object
*   Array

type 也可以是一个自定义构造器，使用 instanceof 检测。

## 自定义事件

父组件是使用 props 传递数据给子组件，但如果子组件要把数据传递回去，就需要使用自定义事件, 我们可以使用 v-on 绑定自定义事件, 每个 Vue 实例都实现了事件接口(Events interface)，即：

*   使用 `$on(eventName)` 监听事件
*   使用 `$emit(eventName)` 触发事件

另外，父组件可以在使用子组件的地方直接用 v-on 来监听子组件触发的事件。

以下实例中子组件已经和它外部完全解耦了。它所做的只是触发一个父组件关心的内部事件。

```
<div id="app">
    <div id="counter-event-example">
      <p>{{ total }}</p>
      <button-counter v-on:increment="incrementTotal"></button-counter>
      <button-counter v-on:increment="incrementTotal"></button-counter>
    </div>
</div>
 
<script>
Vue.component('button-counter', {
  template: '<button v-on:click="increment">{{ counter }}</button>',
  data: function () {
    return {
      counter: 0
    }
  },
  methods: {
    increment: function () {
      this.counter += 1
      this.$emit('increment')
    }
  },
})
new Vue({
  el: '#counter-event-example',
  data: {
    total: 0
  },
  methods: {
    incrementTotal: function () {
      this.total += 1
    }
  }
})
</script>
```
