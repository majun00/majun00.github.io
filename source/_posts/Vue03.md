---
title: Vue-(3)--路由
tags: 
- Vue
categories:
- Vue
---

## 路由
HTML 代码 :
```
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
 
<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- 使用 router-link 组件来导航. -->
    <!-- 通过传入 `to` 属性指定链接. -->
    <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
```

JS代码 :
```
// 0. 如果使用模块化机制编程，導入Vue和VueRouter，要调用 Vue.use(VueRouter)
 
// 1. 定义（路由）组件。
// 可以从其他文件 import 进来
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }
 
// 2. 定义路由
// 每个路由应该映射一个组件。 其中"component" 可以是
// 通过 Vue.extend() 创建的组件构造器，
// 或者，只是一个组件配置对象。
// 我们晚点再讨论嵌套路由。
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]
 
// 3. 创建 router 实例，然后传 `routes` 配置
// 你还可以传别的配置参数, 不过先这么简单着吧。
const router = new VueRouter({
  routes // （缩写）相当于 routes: routes
})
 
// 4. 创建和挂载根实例。
// 记得要通过 router 配置参数注入路由，
// 从而让整个应用都有路由功能
const app = new Vue({
  router
}).$mount('#app')
 
// 现在，应用已经启动了！
```

### vue2.0的路由参数定义实现url的传值

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

  router : router //开启路由对象

});

</script>

```

### vue2.0中嵌套路由的写法

```html

<body>

  <div id="app">

    <router-link to="/account/register">注册</router-link>

    <router-link to="/account/login">登录</router-link>

    <router-view></router-view>

  </div>

</body>

<script>

// 1.0 准备组件

var App = Vue.extend({});

var account = Vue.extend({

  template:'<div><h1>账号组件</h1><router-view></router-view></div>'

});

var login = Vue.extend({

  template:'<h1>登录</h1>'

});

var register = Vue.extend({

  template:'<h1>注册</h1>'

});

// 2.0 实例化路由对象

var router = new VueRouter({

  routes:[

    {

      path:'/account',

      component:account,

      children:[

        {

          path:'login',

          component:login

        },

        {

          path:'register',

          component:register

        }

      ]

    }

  ]

});

// 3.0 使路由生效

new Vue({

  el :'#app',

  router:router

})

</script>

```
