---
title: Vue-(9)--watch,computed
tags: 
- Vue
categories:
- Vue
---

## watch,computed

我们可以使用 methods 来替代 computed，效果上两个都是一样的，但是 computed 是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值。而使用 methods ，在重新渲染的时候，函数总会重新调用执行。

```
<div id="example">
  a={{ a }}, b={{ b }}
</div>
```

```
var vm = new Vue({
  el: '#example',
  data: {
    a: 1
  },
  computed: {
    // 一个计算属性的 getter
    b: function () {
      // `this` 指向 vm 实例
      return this.a + 1
    }
  }
})
```

```
<div id="demo">{{fullName}}</div>
```

```
var vm = new Vue({
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  }
})

vm.$watch('firstName', function (val) {
  this.fullName = val + ' ' + this.lastName
})

vm.$watch('lastName', function (val) {
  this.fullName = this.firstName + ' ' + val
})
```

上面代码是命令式的重复的。跟计算属性对比：

```
var vm = new Vue({
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

### 计算 setter

计算属性默认只是 getter，不过在需要时你也可以提供一个 setter：

```
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
//
```

## computed setter

computed 属性默认只有 getter ，不过在需要时你也可以提供一个 setter ：

```
var vm = new Vue({
  el: '#app',
  data: {
    name: 'Google',
    url: 'http://www.google.com'
  },
  computed: {
    site: {
      // getter
      get: function () {
        return this.name + ' ' + this.url
      },
      // setter
      set: function (newValue) {
        var names = newValue.split(' ')
        this.name = names[0]
        this.url = names[names.length - 1]
      }
    }
  }
})
// 调用 setter， vm.name 和 vm.url 也会被对应更新
vm.site = '菜鸟教程 http://www.runoob.com';
document.write('name: ' + vm.name);
document.write('<br>');
document.write('url: ' + vm.url);
```

```html

<body>

  <div id="app">

    <router-link to="/login">登录</router-link>

    <router-link to="/register/itcast">注册</router-link>

    <router-view></router-view>

  </div>

</body>

<script>

// 1.0 准备组件

var App = Vue.extend({});

var login = Vue.extend({

  template:'<div><h1>登录</h1></div>'

});

var register = Vue.extend({

  template:'<div><h1>注册{{name}}</h1></div>',

  data:function(){

    return {

      name:''

    }

  },

  created:function(){

    this.name = this.$route.params.name1;

  }

});

// 2.0 实例化路由规则对象

var router = new VueRouter({

  routes:[

    {path:'/',redirect:'/login'},

    {path:'/login',component:login},

    {path:'/register/:name1',component:register}

  ]

});

// 3.0 开启路由对象

new Vue({

  el :'#app',

  router : router, //开启路由对象

  watch:{

    '$route':function(newroute,oldroute){

      // console.log(newroute,oldroute);

      // 将来就可以在这个函数中获取到当前的路由规则字符串是谁了

      // 那么就可以针对一些特定的页面做一些特定的处理

    }

  }

});

</script>

```

```html

<body>

  <div id="app">

    <input type="text" v-model="firstName">

    <input type="text" v-model="lastName">

    {{fullName}}

  </div>

</body>

<script>

new Vue({

  el:'#app',

  data:{

    firstName:'heima',

    lastName:'itcast'

  },

  computed:{

    fullName:function(){

      return this.firstName + this.lastName;

    }

  }

});

</script>

```
