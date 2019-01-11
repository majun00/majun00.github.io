---
title: Vue(8)--v-el,v-ref
tags: 
- Vue
categories:
- Vue
---

## v-el,v-ref

### 通过v-el获取到dom对象

### 通过v-ref获取到整个组件的对象

```html

<body>

  <div id="app">

    <button @click="getdom">获取dom对象</button>

    <!-- <div v-el:mydiv id="div1">hello v-el</div> -->

    <div ref="mydiv" id="div1">hello</div>

    <button @click="getcom">获取组件对象</button>

    <login ref="mycom"></login>

  </div>

</body>

<script>

new Vue({

  el:'#app',

  methods:{

    getdom:function(){

      //console.log(this.$els.mydiv.innerHTML);

      console.log(this.$refs.mydiv.innerHTML);

    },

    getcom:function(){

      console.log(this.$refs.mycom.subname);

    }

  },

  components:{

    'login':{

      data:function(){

        return {

          subname:'子组件名称'

        }

      },

      template:'<h1>这是一个子组件</h1>'

    }

  }

});

</script>

```
