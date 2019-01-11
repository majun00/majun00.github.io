---
title: Vue(7)--过渡动画
tags: 
- Vue
categories:
- Vue
---

## 过渡动画

### transition

### Vue中过渡动画的几种常用写法

- 利用css控制过渡动画

vue2.0

```html

<style>

.show-enter-active,.show-leave-active{

  transition:all 0.4s ease;

  padding-left: 10px;

}

.show-enter,.show-leave-active{

  padding-left: 100px;

}

</style>

<transition name="show">

  <div v-show="isshow"></div>

</transition>

```

- 利用animate.css控制过渡动画

vue2.0

```html

<link rel="stylesheet" href="../animate.css">

<style type="text/css">

  .show{

    transition:all 0.4s ease;

  }

</style>

<transition enter-active-class='fadeInRight' 

      leave-active-class="fadeOutRight" >

  <div v-show="isshow" class="animated" class="show"></div>

</transition>

```

- 利用钩子函数控制过渡动画

Vue2.0

```html

<style>

  .show{

    transition:all 0.3s ease;

  }

</style>

<transition @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter">

  <div v-if="isshow" class="show">hello vuejs</div>

</transition>

```

```javascript

methods:{

  toggle:function(){

    this.isshow = !this.isshow;

  },

  beforeEnter:function(el){

    el.style.transform = "translate(100px,0)";

  },

  enter:function(el,done){

    el.offsetWidth;

    el.style.transform = "translate(0px,0)";

    done();

  },

  afterEnter:function(el){

    this.isshow =  !this.isshow;

  }

}

```

1、过渡动画进入

   before-enter      过渡动画进入之前，一般在这个方法中定义目标元素的初始位置

   enter             过渡动画进入中，在这个方法中定义目标元素的结束位置

   after-enter       过渡动画结束后，通常在这个方法里面重置初始值

   enter-cancelled   取消过渡动画时被调用

2、过渡动画离开

   before-leave      动画离开之前触发    

   leave             过渡动画进入中触发

>    after-leave       过渡动画离开结束后

   leave-cancelled   取消过渡动画时被调用
