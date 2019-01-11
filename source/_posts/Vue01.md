---
title: Vue-(1)--基础知识
tags: 
- Vue
categories:
- Vue
---

## MVVM

- Model 数据存储

- View 页面视图

- View Model 业务逻辑处理,对数据加工后交给视图处理

解决问题:在一般的业务逻辑处理中,会涉及视图操作,会将业务逻辑和视图代码柔和在一起,因此用MVVM来实现将业务逻辑代码和视图代码完全分离,使用各自职责更清晰,后期代码维护更简单

区别: MVC和MVVM都是要解决如何将数据(M)显示到视图(V)上,MVC的数据中转是通过C控制,C可以主动获取M中数据的更新,同时监控视图的页面变化;MVVM的双向数据绑定不会造成模块的耦合,因为VM的存在,可以对M和V的数据通信进行检测处理** 

## 安装步骤

(node官网重新下)

(`$ npm install -g cnpm --registry=https://registry.npm.taobao.org`)

(`$ cnpm install npm -g`) 

(`$ cnpm install npm@latest -g 或者update`)

(`$ cnpm install webpack -g`)

`$ cnpm install vue`

`$ cnpm install --global vue-cli`

`$ vue init webpack my-project `

(`$ vue init webpack-simple my-project`)

(`$ cnpm install vue-router vue-resource --save`)

(`$ cd my-project` 进入my-project)

`$ cnpm install`

(`$ cnpm install vue-router vue-resource --save`)

`$ npm run dev`

## 目录结构

| 目录/文件 | 说明 |
| --- | --- |
| build | 最终发布的代码存放位置。 |
| config | 配置目录，包括端口号等。我们初学可以使用默认的。 |
| node_modules | npm 加载的项目依赖模块 |
| src | 这里是我们要开发的目录，基本上要做的事情都在这个目录里。里面包含了几个目录及文件：assets: 放置一些图片，如logo等。components: 目录里面放了一个组件文件，可以不用。 App.vue: 项目入口文件，我们也可以直接将组件写这里，而不使用 components 目录。main.js: 项目的核心文件。|
| static | 静态资源目录，如图片、字体等。 |
| test | 初始测试目录，可删除 |
| .xxxx文件 | 这些是一些配置文件，包括语法配置，git配置等。 |
| index.html | 首页入口文件，你可以添加一些 meta 信息或同统计代码啥的。 |
| package.json | 项目配置文件。 |
| README.md | 项目的说明文档，markdown 格式 |

## 系统指令

### 插值表达式

### v-text,v-html

将一个变量的值渲染到指定的元素中

注意：使用v-html渲染数据可能会非常危险，因为它很容易导致 XSS（跨站脚本） 攻击，使用的时候请谨慎

### 条件语句 v-show,v-if

- v-show根据表达式的真假值，切换元素的 display CSS

- v-if根据表达式的值的真假条件来决定是否渲染元素

### v-else 指令(跟在v-if或v-show后面)

随机生成一个数字，判断是否大于0.5，然后输出对应信息：

```
<div id="app">
    <div v-if="Math.random() > 0.5">
      Sorry
    </div>
    <div v-else>
      Not sorry
    </div>
</div>
    
<script>
new Vue({
  el: '#app'
})
</script>
```

### v-else-if 指令

判断 type 变量的值：

```
<div id="app">
    <div v-if="type === 'A'">
      A
    </div>
    <div v-else-if="type === 'B'">
      B
    </div>
    <div v-else-if="type === 'C'">
      C
    </div>
    <div v-else>
      Not A/B/C
    </div>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    type: 'C'
  }
})
</script>
```

### 循环语句 v-for

- 根据数组中的元素遍历指定模板内容生成内容


`<div v-for="(item,index) in items"></div>`

`<div v-for="(val,key) in user"></div>`

`<div v-for="(val,key,index) in user"></div>`

`<div v-for="item in items" :key="item.id">{{ item.text }}</div>`

注意`{{$index}}`可直接显示索引

```

new Vue({

   data:{

       items:[{text:'1'},{text:'2'}],

       user:{uname:'ivan',age:32}

       }

});

```

### 表单 v-model

- 在表单控件或者组件上创建双向绑定

- v-model仅能在如下元素中使用：input select textarea components(Vue中的组件)

##### 修饰符：

  - .lazy取代input监听change事件

  - .number自动将输入的字符串转为数字

  - .trim自动将输入的内容首尾空格去掉

  `<input type="text" v-model.number="age">`

  `<input type="text" v-model.trim="age">`


##### .lazy

在默认情况下， v-model 在 input 事件中同步输入框的值与数据，但你可以添加一个修饰符 lazy ，从而转变为在 change 事件中同步：

```
<!-- 在 "change" 而不是 "input" 事件中更新 -->
<input v-model.lazy="msg" >
```

##### .number

如果想自动将用户的输入值转为 Number 类型（如果原值的转换结果为 NaN 则返回原值），可以添加一个修饰符 number 给 v-model 来处理输入值：

```
<input v-model.number="age" type="number">
```

> 这通常很有用，因为在 type="number" 时 HTML 中输入的值也总是会返回字符串类型。

##### .trim

如果要自动过滤用户输入的首尾空格，可以添加 trim 修饰符到 v-model 上过滤输入：

```
<input v-model.trim="msg">
```

### 单选按钮

```
<div id="app">
  <input type="radio" id="runoob" value="Runoob" v-model="picked">
  <label for="runoob">Runoob</label>
  <br>
  <input type="radio" id="google" value="Google" v-model="picked">
  <label for="google">Google</label>
  <br>
  <span>选中值为: {{ picked }}</span>
</div>
 
<script>
new Vue({
  el: '#app',
  data: {
    picked : 'Runoob'
  }
})
</script>
```

## 复选框

以下实例中演示了复选框的双向数据绑定：

```
<div id="app">
  <p>单个复选框：</p>
  <input type="checkbox" id="checkbox" v-model="checked">
  <label for="checkbox">{{ checked }}</label>
    
  <p>多个复选框：</p>
  <input type="checkbox" id="runoob" value="Runoob" v-model="checkedNames">
  <label for="runoob">Runoob</label>
  <input type="checkbox" id="google" value="Google" v-model="checkedNames">
  <label for="google">Google</label>
  <input type="checkbox" id="taobao" value="Taobao" v-model="checkedNames">
  <label for="taobao">taobao</label>
  <br>
  <span>选择的值为: {{ checkedNames }}</span>
</div>
 
<script>
new Vue({
  el: '#app',
  data: {
    checked : false,
    checkedNames: []
  }
})
</script>
```
### select

```
<div id="app">
  <select v-model="selected" name="fruit">
    <option value="">选择一个网站</option>
    <option value="www.runoob.com">Runoob</option>
    <option value="www.google.com">Google</option>
  </select>
 
  <div id="output">
      选择的网站是: {{selected}}
  </div>
</div>
 
<script>
new Vue({
  el: '#app',
  data: {
    selected: '' 
  }
})
</script>
```

## v-bind

给html元素或者组件动态地绑定一个或多个特性,可以简写成":"

> 注 :
> 1. `:class :src`
> 2. `<a v-bind="{href:'http://itcast.cn/index/'+id}">跳转</a>`

(有些指令可以在其名称后面带一个“参数” (Argument)，中间放一个冒号隔开。例如，`v-bind` 指令用于响应地更新 HTML 特性：

```
<a v-bind:href="url"></a>
```

这里 `href` 是参数，它告诉 `v-bind` 指令将元素的 `href` 特性跟表达式 `url` 的值绑定。)

### :class
##### 对象语法

我们可以传给 `v-bind:class` 一个对象，以动态地切换 class。注意 `v-bind:class` 指令可以与普通的 `class` 特性共存：

```
<div class="static" v-bind:class="{ 'class-a': isA, 'class-b': isB }"></div>
```

```
data: {
  isA: true,
  isB: false
}
```

当 `isA` 和 `isB` 变化时，class 列表将相应地更新。例如，如果 `isB` 变为 `true`，class 列表将变为 `"static class-a class-b"`。

你也可以直接绑定数据里的一个对象：

```
<div v-bind:class="classObject"></div>
```

```
data: {
  classObject: {
    'class-a': true,
    'class-b': false
  }
}
```

##### 数组语法

我们可以把一个数组传给 `v-bind:class`，以应用一个 class 列表：

```
<div v-bind:class="[classA, classB]">
```

```
data: {
  classA: 'class-a',
  classB: 'class-b'
}
```

如果你也想根据条件切换列表中的 class，可以用三元表达式：

```
<div v-bind:class="[classA, isB ? classB : '']">
```

此例始终添加 `classA`，但是只有在 `isB` 是 `true` 时添加 `classB` 。

### 绑定内联样式

##### 对象语法

CSS 属性名可以用驼峰式（camelCase）或短横分隔命名（kebab-case）：

```
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

```
data: {
  activeColor: 'red',
  fontSize: 30
}
```

直接绑定到一个样式对象通常更好，让模板更清晰：

```
<div v-bind:style="styleObject"></div>
```

```
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

同样的，对象语法常常结合返回对象的计算属性使用。

##### 数组语法

`v-bind:style` 的数组语法可以将多个样式对象应用到一个元素上：

```
<div v-bind:style="[styleObjectA, styleObjectB]">
```

### 事件处理器 v-on

- v-on绑定事件监听,可以使用"@"替代"v-on:"

- 此外提供了很多事件修饰符来辅助实现一些功能

  + .stop 调用 event.stopPropagation()

  + .prevent 调用 event.preventDefault()

  + .capture 添加事件侦听器时使用 capture 模式

  + .self 只当事件是从侦听器绑定的元素本身触发时才触发回调

  + .{keyCode | keyAli> 只当事件是从侦听器绑定的元素本身触发时才触发回调

  + .native 监听组件根元素的原生事件

- Vue还允许为v-on在监听键盘事件时添加按键修饰符

    - .enter .tab .delete (捕获 “删除” 和 “退格” 键) .esc .space .up .down .left .right

    - 还可以自定义按键别名

        + 在Vue2.0 中默认的按键修饰符是存储在 Vue.config.keyCodes中

        + 如`Vue.config.keyCodes.f1 = 112` `@keydown.f1="add"`

##### 事件修饰符

Vue.js 为 v-on 提供了事件修饰符来处理 DOM 事件细节，如：event.preventDefault() 或 event.stopPropagation()。

Vue.js通过由点(.)表示的指令后缀来调用修饰符, `.stop  .prevent  .capture .self .once`

```
<!-- 阻止单击事件冒泡 -->
<a v-on:click.stop="doThis"></a>
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联  -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件侦听器时使用事件捕获模式 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当事件在该元素本身（而不是子元素）触发时触发回调 -->
<div v-on:click.self="doThat">...</div>

<!-- click 事件只能点击一次，2.1.4版本新增 -->
<a v-on:click.once="doThis"></a>
```

##### 按键修饰符

Vue 允许为 v-on 在监听键盘事件时添加按键修饰符：

```
<!-- 只有在 keyCode 是 13 时调用 vm.submit() -->
<input v-on:keyup.13="submit">
```

记住所有的 keyCode 比较困难，所以 Vue 为最常用的按键提供了别名：

```
<!-- 同上 -->
<input v-on:keyup.enter="submit">
<!-- 缩写语法 -->
<input @keyup.enter="submit">
```

> 全部的按键别名：`.enter  .tab .delete (捕获 "删除" 和 "退格" 键) .esc .space .up .down .left .right .ctrl .alt .shift .meta`

```
<div id="app">
  <input v-model="newTodo" v-on:keyup.enter="addTodo">
  <ul>
    <li v-for="todo in todos">
      <span>{{ todo.text }}</span>
      <button v-on:click="removeTodo($index)">X</button>
    </li>
  </ul>
</div>
```

```
new Vue({
  el: '#app',
  data: {
    newTodo: '',
    todos: [
      { text: 'Add some todos' }
    ]
  },
  methods: {
    addTodo: function () {
      var text = this.newTodo.trim()
      if (text) {
        this.todos.push({ text: text })
        this.newTodo = ''
      }
    },
    removeTodo: function (index) {
      this.todos.splice(index, 1)
    }
  }
})
```
