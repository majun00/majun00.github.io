---
title: Vue--Vuex状态管理
tags: 
- Vue
categories:
- Vue
---

> Vuex 就是前端为了方便数据的操作而建立的一个” 前端数据库“。模块间是不共享作用域的，那么B 模块想要拿到 A 模块的数据，我们会怎么做？我们会定义一个全局变量，叫aaa 吧，就是window.aaa。然后把A 模块要共享的数据作为属性挂到 B 模块上。这样我们在 B 模块中通过window.aaa 就可以拿到这个数据了。但是问题来了，B 模块拿到了共享的数据，就叫他xxx 吧？得了，名字太混乱了，咱先给它们都改下名字。那个全局变量既然是存东西的，就叫store 吧，共享的数据就叫state 吧，B 模块拿到了 A 模块的数据state，但是这个数据不是一成不变的呀，A 要操作这个数据的。那么我们是不是要在这个数据——state 改变的时候通知一下 B？那写个自定义事件吧。首先，你得能取吧？那么得有一套取数据的 API，我们给他们集中起个名字吧？既然是获取，那就叫getter 吧。我们还得存数据呀，是吧。存数据就是对数据库的修改，这些 API，我们也得给它起个名字，就叫mutation。vuex 生成的仓库也就这么出来了，所以我说 vuex 就是” 前端的数据库“。State 就是数据库。Mutations 就是我们把数据存入数据库的 API，用来修改state 的。getters 是我们从数据库里取数据的 API，既然是取，那么你肯定不能把数据库给改了吧？所以getters 得是一个”纯函数“，就是不会对原数据造成影响的函数。拿到了数据，总要做个处理吧，处理完了再存到数据库中。其实这就是action的过程。当然你也可以不做处理，直接丢到数据库。来看看 vuex 的数据流，通过action处理数据，然后通过mutation 把处理后的数据放入数据库（state）中，谁要用就通过getter从数据库（state）中取。

## 安装

```
npm install --save vuex
```

## 引入

```
import Vuex from 'vuex'
import Vue from 'vue'
Vue.use(Vuex)
```

## 实例化生成store的过程是

```
//store为实例化生成的
import store from './store'

new Vue({
  el: '#app',
  store,
  render: h => h(App)
})
```

```
const mutations = {...};
const actions = {...};
const state = {...};

Vuex.Store({
  state,
  actions,
  mutation
});
```

## State
存储整个应用的状态数据.
想要获取对应的状态你就可以直接使用`this.$store.state`获取，也可以利用`vuex`提供的`mapState`辅助函数将`state`映射到计算属性中去

```
//我是组件
import {mapState} from 'vuex'

export default {
  computed: mapState({
    count: state => state.count
  })
}
```

## Mutations
更改状态，本质就是用来处理数据的函数, 其接收唯一参数值`state`。
`store.commit(mutationName)`是用来触发一个`mutation`的方法。需要记住的是，定义的`mutation`必须是同步函数

```
const mutations = {
  mutationName(state) {
    //在这里改变state中的数据
  }
}
```

在组件中触发：

```
//我是一个组件
export default {
  methods: {
    handleClick() {
      this.$store.commit('mutationName')
    }
  }
}
```

或者使用辅助函数`mapMutations`直接将触发函数映射到`methods`上，这样就能在元素事件绑定上直接使用了

```
import {mapMutations} from 'vuex'

//我是一个组件
export default {
  methods: mapMutations([
    'mutationName'
  ])
}
```

## Actions

`Actions`也可以用于改变状态，不过是通过触发`mutation`实现的，重要的是可以包含异步操作。其辅助函数是`mapActions`与`mapMutations`类似，也是绑定在组件的`methods`上的。如果选择直接触发的话，使用`this.$store.dispatch(actionName)`方法。

```
//定义Actions
const actions = {
  actionName({ commit }) {
    //dosomething
    commit('mutationName')
  }
}
```

在组件中使用

```
import {mapActions} from 'vuex'

//我是一个组件
export default {
  methods: mapActions([
    'actionName',
  ])
}
```

## Getters

有些状态需要做二次处理，就可以使用`getters`。通过`this.$store.getters.valueName`对派生出来的状态进行访问。或者直接使用辅助函数`mapGetters`将其映射到本地计算属性中去。

```
const getters = {
  strLength: state => state.aString.length
}
//上面的代码根据aString状态派生出了一个strLength状态
```

在组件中使用

```
import {mapGetters} from 'vuex'

//我是一个组件
export default {
  computed: mapGetters([
    'strLength'
  ])
}
```

## Plugins

插件就是一个钩子函数，在初始化`store`的时候引入即可。比较常用的是内置的logger插件，用于作为调试使用。

```
import createLogger from 'vuex/dist/logger'
const store = Vuex.Store({
  ...
  plugins: [createLogger()]
})
```

---

# Example

```
npm install --save vuex
```

该步骤完成之后，我们需要在 src 目录下创建名为 store 的目录来存放状态管理相关代码，首先创建 index.js:

```
import Vue from 'vue'
import Vuex from 'vuex'
import axios from 'axios'
Vue.use(Vuex)
const store = new Vuex.Store({
  state: {

  },
  actions: {

  },
  mutations: {

  },
  getters: {

  }
})
export default store

```

然后在 main.js 文件中我们需要将该 Store 实例添加到构造的 Vue 实例中：

```
import store from './store'
/* eslint-disable no-new */
new Vue({
  template: `
  <div>
    <navbar />
    <section class="section">
      <div class="container is-fluid">
        <router-view></router-view>
      </div>
    </section>
  </div>
  `,
  router,
  store,
  components: {
    navbar
  }
}).$mount('#app')

```

然后，我们需要去完善 Store 定义：

```
import Vue from 'vue'
import Vuex from 'vuex'
import axios from 'axios'
Vue.use(Vuex)
const store = new Vuex.Store({
  state: {
    projects: []
  },
  actions: {
    LOAD_PROJECT_LIST: function ({ commit }) {
      axios.get('/secured/projects').then((response) => {
        commit('SET_PROJECT_LIST', { list: response.data })
      }, (err) => {
        console.log(err)
      })
    }
  },
  mutations: {
    SET_PROJECT_LIST: (state, { list }) => {
      state.projects = list
    }
  },
  getters: {
    openProjects: state => {
      return state.projects.filter(project => !project.completed)
    }
  }
})
export default store

```

在本项目中，我们将原本存放在组件内的项目数组移动到 Store 中，并且将所有关于状态的改变都通过 Action 进行而不是直接修改：

```
// /src/components/projectList.vue
<template lang="html">
  <div class="">
    <table class="table">
      <thead>
        <tr>
          <th>Project Name</th>
          <th>Assigned To</th>
          <th>Priority</th>
          <th>Completed</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in projects">
          <td>{{item.name}}</td>
          <td>{{item.assignedTo}}</td>
          <td>{{item.priority}}</td>
          <td><i v-if="item.completed" class="fa fa-check"></i></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import { mapState } from 'vuex'
export default {
  name: 'projectList',
  computed: mapState([
    'projects'
  ])
}
</script>
<style lang="css">
</style>

```

这个模板还是十分直观，我们通过computed对象来访问 Store 中的状态信息。值得一提的是这里的mapState函数，这里用的是简写，完整的话可以直接访问 Store 对象：

```
computed: {
  projects () {
    return this.$store.state.projects
  }
}

```

mapState 是 Vuex 提供的简化数据访问的辅助函数。我们视线回到 project.vue 容器组件，在该组件中调用this.$store.dispatch('LOAD_PROJECT_LIST)来触发从服务端中加载项目列表：

```
<template lang="html">
  <div id="projects">
    <div class="columns">
      <div class="column is-half">
        <div class="notification">
          Project List
        </div>
        <project-list />
      </div>
    </div>
  </div>
</template>
<script>
import projectList from '../components/projectList'
export default {
  name: 'projects',
  components: {
    projectList
  },
  mounted: function () {
    this.$store.dispatch('LOAD_PROJECT_LIST')
  }
}
</script>

```

当我们启动应用时，Vuex 状态管理容器会自动在数据获取之后渲染整个项目列表。现在我们需要添加新的 Action 与 Mutation 来创建新的项目：

```
// under actions:
ADD_NEW_PROJECT: function ({ commit }) {
  axios.post('/secured/projects').then((response) => {
    commit('ADD_PROJECT', { project: response.data })
  }, (err) => {
    console.log(err)
  })
}
// under mutations:
ADD_PROJECT: (state, { project }) => {
  state.projects.push(project)
}

```

然后我们创建一个简单的用于添加新的项目的组件 addProject.vue:

```
<template lang="html">
  <button type="button" class="button" @click="addProject()">Add New Project</button>
</template>
<script>
export default {
  name: 'addProject',
  methods: {
    addProject () {
      this.$store.dispatch('ADD_NEW_PROJECT')
    }
  }
}
</script>

```

该组件会派发某个 Action 来添加组件，我们需要将该组件引入到 projects.vue 中：

```
<template lang="html">
  <div id="projects">
    <div class="columns">
      <div class="column is-half">
        <div class="notification">
          Project List
        </div>
        <project-list />
        <add-project />
      </div>
    </div>
  </div>
</template>
<script>
import projectList from '../components/projectList'
import addProject from '../components/addProject'
export default {
  name: 'projects',
  components: {
    projectList,
    addProject
  },
  mounted: function () {
    this.$store.dispatch('LOAD_PROJECT_LIST')
  }
}
</script>

```

重新运行下该应用会看到服务端返回的创建成功的提示，现在我们添加另一个功能，就是允许用户将某个项目设置为已完成。我们现在添加新的组件 completeToggle.vue:

```
<template lang="html">
  <button type="button" class="button"  @click="toggle(item)">
    <i class="fa fa-undo" v-if="item.completed"></i>
    <i class="fa fa-check-circle" v-else></i>
  </button>
</template>
<script>
export default {
  name: 'completeToggle',
  props: ['item'],
  methods: {
    toggle (item) {
      this.$store.dispatch('TOGGLE_COMPLETED', { item: item })
    }
  }
}
</script>

```

该组件会展示一个用于切换项目是否完成的按钮，我们通过 Props 传入具体的项目信息然后通过触发 TOGGLE_COMPLETED Action 来使服务端进行相对应的更新与相应：

```
// actions
TOGGLE_COMPLETED: function ({ commit, state }, { item }) {
  axios.put('/secured/projects/' + item.id, item).then((response) => {
    commit('UPDATE_PROJECT', { item: response.data })
  }, (err) => {
    console.log(err)
  })
}
// mutations
UPDATE_PROJECT: (state, { item }) => {
  let idx = state.projects.map(p => p.id).indexOf(item.id)
  state.projects.splice(idx, 1, item)
}

```

UPDATE_PROJECT 会触发项目列表移除对应的项目并且将服务端返回的数据重新添加到数组中：

```
app.put('/secured/projects/:id', function (req, res) {
  let project = data.filter(function (p) { return p.id == req.params.id })
  if (project.length > 0) {
    project[0].completed = !project[0].completed
    res.status(201).json(project[0])
  } else {
    res.sendStatus(404)
  }
})

```

最后一步就是将 completeToggle 组件引入到 projectList 组件中，然后将其添加到列表中：

```
// new column in table
<td><complete-toggle :item="item" /></td>
// be sure import and add to the components object

```

## 整理
### 基本用途：
*   将某些data变成组件间公用的状态，组件随时都可以进行访问和响应，解决了`props`传值的链式响应的代码冗余
*   给状态配以公用方法，将状态的变更及时响应并处理

### 基本用法：
/store/store.js
```
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

export const store = new Vuex.Store({
    state: {
        sideBarOpened: false
        //放置公用状态
    },
    getters: {
        changeState: state => {
            //相当于vue实例中的methods,用于处理公用data
            //自vuex 面向组件的数据处理
        }
    },
    mutations: {
        //写法与getters相类似
        //组件想要对于vuex 中的数据进行的处理
        //组件中采用this.$store.commit('方法名') 的方式调用，实现充分解耦
        //内部操作必须在此刻完成(同步)
    },
    actions: {
        //使得mutations能够实现异步调用，实现例如延迟调用
        increment ({ commit }, payload) {
            commit('突变方法名')
            //payload应该是一个对象，可通过模板方法调用传入对象的方式将数据从组件传入vuex
        },
        asyncIncrement({commit}) => {
            setTimeout(() => {
                commit('increment')
            }, 1000)
        }
    },
    modules: {
        //引入某一个state的以上集合的模块，会自动分别填充到上面，使得结构更加清晰
    }
});
```
main.js
```
import { store } from './store/store'
*
*
new Vue({
  el: '#app',
  store,    //注入根组件
  ...
})
```

#### 访问vuex中的数据和方法
```
this.$store.state.数据名
this.$store.getters.方法名
```
受影响组件局部定义计算属性响应变化数据
```
computed: {
     open () {
          return this.$store.state.sideBarOpened
     }
}
```
将 store 中的 getters/mutations 映射到局部(计算属性/方法)使用`mapGetters/mapMutations`辅助函数
```
import { mapGetters } from 'vuex'

computed: {
  // 使用对象展开运算符将 getters 混入 computed 对象中
    ...mapGetters([
        //映射 this.doneTodosCount 为 store.getters.doneTodosCount
      'doneTodosCount',
      //'getter名称',

      // 映射 this.doneCount 为 store.getters.doneTodosCount
        doneCount: 'doneTodosCount'
      // 三个点表示将内部拿出生成键值对，这样使得组件本身的计算属性不受影响
      // 此语法依赖babel-preset-stage-2
    ])
  }
```

### 注意事项：
mutation 必须是同步函数 — devtool要保存快照，方便追踪状态变化
使用 v-model 绑定 vuex 计算属性的时候要设置get 和 set 才能双向绑定
```
computed: {
    value: {
        get () {
            return this.$store.getters.value;
        },
        set (event) {
            this.$store.dispatch('updateValue', event.target.value);
        }
    }
}
```

注意:
1. 当 Vue 组件从 store 中读取状态的时候，若 store中的状态发生变化，那么相应的组件也会相应地得到高效更新。
2. 不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地提交mutations
```
// 如果在模块化构建系统中，请确保在开头调用了 Vue.use(Vuex)
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```
现在，你可以通过 store.state 来获取状态对象，以及通过 store.commit 方法触发状态变更：

```
store.commit('increment')
console.log(store.state.count) // -> 1
```
**每次对数据对象进行操作的时候，都需要进行commit**

### state
Vuex 通过 store 选项，提供了一种机制将状态从根组件『注入』到每一个子组件中（需调用 Vue.use(Vuex)）：
```
const app = new Vue({
  el: '#app',
  // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
```
通过在根实例中注册 store 选项，该 store 实例会注入到根组件下的所有子组件中，且子组件能通过 this.$store访问到。让我们更新下 Counter 的实现：
```
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```
mapState 辅助函数
辅助函数帮助我们生成计算属性 
mapState函数返回的是一个对象。我们如何将它与局部计算属性混合使用呢？通常，我们需要使用一个工具函数将多个对象合并为一个，以使我们可以将最终对象传给 computed 属性。但是自从有了对象展开运算符（现处于 ECMASCript 提案 stage-3 阶段），我们可以极大地简化写法。

### Getters
可以认为是 store 的计算属性
```
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```
会暴露为 store.getters 对象：
```
store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]
```
也可以接受其他 getters 作为第二个参数
mapGetters 辅助函数 
将 store 中的 getters 映射到局部计算属性
```
import { mapGetters } from 'vuex'
export default {
  // ...
  computed: {
  // 使用对象展开运算符将 getters 混入 computed 对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```
你想将一个 getter 属性另取一个名字，使用对象形式：
```
mapGetters({
  // 映射 this.doneCount 为 store.getters.doneTodosCount
  doneCount: 'doneTodosCount'
})
```
