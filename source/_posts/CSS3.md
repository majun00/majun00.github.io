---
title: 回顾CSS3
tags: 
- CSS3
categories:
- CSS3
---

# CSS3
### 属性选择器
- E[attr]
- E[attr=val]
- E[attr*=val] 属性值里包含val字符并且在“任意”位置
- E[attr^=val] 属性值里包含val字符并且在“开始”位置
- E[attr$=val] 属性值里包含val字符并且在“结束”位置

### 伪类选择器:
- a:hover a:link a:active a:visited
- 兄弟伪类：
	- +：获取当前元素的相邻的满足条件的元素
	- ~：获取当前元素的满足条件的兄弟元素
- E:first-child 查找E这个元素的父元素的第一个子元素E
- E:last-child 最后一个子元素
- E:nth-child(n) 第n个子元素
- E:nth-last-child(n) 同E:nth-child(n) 相似，只是倒着计算
- E:nth-child(even) 所有的偶数
- E:nth-child(odd) 所有的奇数
> 	1. 相对于当前指定元素的父元素,查找的类型必须是指定的类型
> 	2. 指定索引位置 nth-child(从1开始的索引||关键字||表达式),默认取值范围为0~子元素的长度,但是当n<=0时,选取无效,如E:nth-of-type(-n+5):前五个
- E:first-of-type
- E:last-of-type
- E:nth-of-type(n)
- E:nth-of-type(even)
- E:nth-of-type(odd)
> 	在查找的时候只会查找满足类型条件的元素，过渡掉其它类型的元素
- E:empty 选中没有任何子节点的E元素，注意，空格也算子元素
- E:target 当目标元素被触发为当前锚链接的目标时,调用此伪类样式

### 伪元素选择器:
- E::before E::after
>	content:""
>	display:block float position
- E::first-letter 文本的第一个字母或字
- E::first-line 文本第一行
> 	如果设置了::first-letter,那么无法同时设置它的样式
- E::selection 可改变选中文本的样式
> 	只能设置显示的样式，而不能设置内容大小

### 颜色 RGBA HSLA
- transparent 不可调节透明度，始终完全透明
- opacity 只能针对整个盒子设置透明度,子盒子及内容会继承父盒子的透明度
- rgba 来控制颜色,相对opacity,不具有继承性

### text-shadow
- text-shadow: X-Offset Y-Offset Blur Color
	- X-Offset表示阴影的水平偏移距离，其值为正值时阴影向右偏移，如果其值为负值时，阴影向左偏移；
	- Y-Offset是指阴影的垂直偏移距离，如果其值是正值时，阴影向下偏移,反之其值是负值时阴影向顶部偏移；
	- Blur是指阴影的模糊程度，其值不能是负值，如果值越大，阴影越模糊，反之阴影越清晰，如果不需要阴影模糊可以将Blur值设置为0；
	- Color是指阴影的颜色，其可以使用rgba色

### box-shadow
- box-shadow: h-shadow v-shadow blur spread color inset
	- h 水平方向的偏移值
	- v 垂直方向的偏移值
	- spread 阴影的尺寸,值越大，阴影的扩散面积越大
	- inset 将外部阴影 (outset) 改为内部阴影

### border-radius

### box-sizing盒模型
- content-box 对象的实际宽度等于设置的width值和border、padding之和
- border-box 对象的实际宽度就等于设置的width值，即使定义有border和padding也不会改变对象的实际宽度

### background:linear-gradient()
- background:linear-gradient(方向，开始颜色 位置，颜色2 位置，...);
`background: linear-gradient(to right,red 0%,red 50%,blue 50%,blue 100%);`
- 方向：
	- to top: 0deg
	- to right: 90deg
	- to bottom: 180deg --默认值
	- to left: 270deg

### background:radial-gradient()
- background:radial-gradient(形状 大小 坐标,颜色1，颜色2...)
`background: radial-gradient(red,blue);`
`background: radial-gradient(circle,red,blue);`
`background: radial-gradient(at left top,red,blue);`
`background: radial-gradient(red,red 50%,blue 50%,blue);`
`background: radial-gradient(circle farthest-side at 50px 50px,red,blue);`
- shape
	- circle 产生正方形的渐变色
	- ellipse 适配当前的形状
- 坐标at position 
	- 默认在正中心,可以赋值坐标(参照元素的左上角),也可以赋值关键字(left center right top bottom)
- 大小size
	- closest-side 最近边
	- farthest-side 最远边
	- closest-corner 最近角
	- farthest-corner 最远角

- repeating-linear-gradient
- repeating-radial-gradient
`background: repeating-linear-gradient(45deg,#fff 0%,#fff 10%,#000 10%,#000 20%);}`
`background: repeating-radial-gradient(circle closest-side at center center,#fff 0%,#fff 10%,#000 10%,#000 20%);`

## background
### background-repeat
- round 会将图片进行缩放之后再平铺
- space 图片不会缩放平铺，只是会在图片之间产生相同的间距值

### background-attachment
- fixed 背景图片的位置固定不变
- scroll 当滚动容器的时候，背景图片也会跟随滚动
滚动当前容器的内容时:
- local 背景图片会跟随内容一起滚动
- scroll 背景图片不会跟随内容一起滚动

### background-size
- background-size:auto(原始图片大小)||percentage(百分比)||cover(铺满容器)||contain(放进整个图片)
> 注:percentage:是参照父容器的百分比

### background-origin设置背景坐标的原点
- border-box 从border的位置开始填充背景，会与border重叠
- padding-box 从padding的位置开始填充背景，会与padding重叠
- content-box 从内容的位置开始填充背景

### background-clip设置内容的裁切:设置的是裁切，控制的是显示
- border-box 其实是显示border及以内的内容
- padding-box 其实是显示padding及以内的内容
- content-box 其实是显示content及以内的内容

### border-image
- border-image: source slice / width/outset repeat;
`border-image: url("images/border1.png") 27 / 27px /0px round;`
- border-image-source
- border-image-slice 设置四个方向上的裁切距离,fill:做内容的内部填充,如border-- image-slice: 27 fill;
- border-image-width 图片边框的宽度
- border-image-outset 扩展边框(不常用)
- border-image-repeat 图像边框是否应平铺(repeated)、铺满(rounded)或拉伸(stretched)

### transition
- transition:属性名称 过渡时间 时间函数 延迟;
`transition: left 2s linear 0s;`
`transition: left 2s linear 0s,background-color 5s linear 0s;`
- transition-property:添加过渡效果的样式属性名称
- transition-duration:过渡效果的耗时,以秒做为单位
- transition-delay:过渡效果的延迟
- transition-timing-function:设置时间函数--控制运动的速度
	- linear 规定以相同速度开始至结束的过渡效果
	- ease 规定慢速开始，然后变快，然后慢速结束的过渡效果
	- ease-in 规定以慢速开始的过渡效果
	- ease-out 规定以慢速结束的过渡效果
	- ease-in-out 规定以慢速开始和结束的过渡效果
	- cubic-bezier(n,n,n,n) 在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值

> 1. 过渡效果执行完毕之后，默认会还原到原始状态
> 2. steps():可以让过渡效果分为指定的几次来完成

## transform
### transform:translate() 移动
`transform: translate(100px);`
`transform: translate(400px,500px);`
`transform: translateX(300px);`
`transform: translateY(300px);`
> 1. 移动是参照元素的左上角,执行完毕之后会恢复到原始状态
> 2. 如果只有一个参数就代表x方向,如果有两个参数就代表x/y方向

### transform:scale() 缩放
`transform: scale(2);`
`transform: scale(2,1);`
`transform:scaleX(0.5);`
`transform:scaleY(0.5);`
> 1. 1指不缩放，>1.01放大,<0.99缩小,参照元素的几何中心
> 2. 如果只有一个参数，就代表x和y方向都进行相等比例的缩放,如果有两个参数，就代表x/y方向

### transform:rotate()
`transform:rotate(-90deg);`

### transform:skew()
`transform:skew(-30deg);`
`transform:skew(30deg,-30deg);`
`transform:skewX(30deg);`
`transform:skewY(30deg);`
> 1. 如果角度为正，则往当前轴的负方向斜切，反之，则往当前轴的正方向斜切
> 2. 如果只有一个参数就代表x方向,如果有两个参数就代表x/y方向

> 1. 可添加过渡效果,如transition: transform 2s;
> 2. 可同时添加多个transform属性值,如transform:translateX(700px) rotate(-90deg);
> 3. 设置旋转轴心transform-origin:x y或关键字left top right bottom center
> `transform-origin: left top;`

### transform 3D
- transform:translate3d(x,y,z)
- transform:translateX(length),translateY(length),translateZ(length)

- transform:scale3d(number,number,number) 
- transform:scaleX(),scaleY(),scaleZ()

- transform:rotate3d(x,y,z,angle)
- transform:rotateX(angle),rotateY(angle),rotateZ(angle)

> 1. transform-style：使被转换的子元素保留其 3D 转换(需要设置在父元素中) 
	- flat 子元素将不保留其3D位置-平面方式
	- preserve-3d 子元素将保留其3D位置-立体方式
> 2. perspective(length) perspective-origin


### animation 
- animation-name: name; 指定动画名称
- animation-duration: 2s; 设置动画的总耗时
- animation-iteration-count: 1; 设置动画的播放次数，默认为1次,可以指定具体的数值，也可以指定infinite(无限次)
- animation-direction: alternate; 设置交替动画
- animation-delay: 2s; 设置动画的延迟
- animation-timing-function: linear; 设置动画的时间函数
- animation-play-state 设置动画的播放状态pause(暂停) running
- animation-fill-mode:both; 设置动画结束时的状态,默认情况下，动画执行完毕之后，会回到原始状态
	+ forwards:会保留动画结束时的状态，在有延迟的情况下，并不会立刻进行到动画的初始状态
	+ backwards:不会保留动画结束时的状态，在添加了动画延迟的前提下，如果动画有初始状态，那么会立刻进行到初始状态
	+ both:会保留动画的结束时状态，在有延迟的情况下也会立刻进入到动画的初始状态

```
@keyframes name{
 from{transform: translate(0,0) rotate(45deg);}
 50%{transform: translate(0,500px);}
 to{transform: translate(500px,600px);}
}
```

### 多列布局
- column-count: 属性设置列的具体个数
- column-width: 属性控制列的宽度
- column-gap: 两列之间的缝隙间隔
- column-rule: 规定列之间的宽度、样式和颜色
- column-span: 规定元素应横跨多少列(n:指定跨n列  all:跨所有列)

### 伸缩布局
- display:flex; 设置父容器为盒子：会使每一个子元素自动变成伸缩项,当子元素的宽度和大于父容器宽度的时候，子元素会自动平均收缩

- justify-content: 设置子元素的排列方式
	- flex-start 让子元素从父容器的起始位置开始排列
	- flex-end 让子元素从父容器的结束位置开始排列
	- center 让子元素从父容器的中间位置开始排列
	- space-between 左右对齐父容器的开始和结束，中间平均分页，产生相同的间距
	- space-around 将多余的空间平均的分页在每一个子元素的两边

- flex-flow: flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap
	- flex-direction: 设置子元素的排列方向：就是设置主轴方向，默认主轴方向是row(水平方向)
		- row:水平排列方向，从左到右
		- row-reverse:水平排列方向，从右到左
		- column:垂直排列方向，从上到下
		- column-reverse：垂直排列方向，从下到上
	- flex-wrap: 控制子元素是否换行显示，默认不换行
		- nowrap 不换行--则收缩
		- wrap 换行
		- wrap-reverse 翻转，原来是从上到下，翻转后就是从下到上来排列 

- flex: 设置当前伸缩子项占据剩余空间的比例值
> flex-grow,flex-shrink和flex-basis的简写，默认值为0 1 auto
	+ flow-grow:可以来扩展子元素的宽度：设置当前元素应该占据剩余空间的比例值
	> 比例值计算 ：当前空间的flex-grow/所有兄弟元素的flex-grow的和
	> flex-grow的默认是0:说明子元素并不会去占据剩余的空间
	+ flex-shrink:定义收缩比例，通过设置的值来计算收缩空间
	> 比例值计算 ：当前空间的flex-shrink/所有兄弟元素的flex-shrink的和
	> 默认值为1

- align-items:设置子元素(伸缩项)在侧轴方向上的对齐方式
	- center 设置在侧轴方向上居中对齐
	- flex-start 设置在侧轴方向上顶对齐
	- flex-end 设置在侧轴方向上底对齐
	- stretch 拉伸：让子元素在侧轴方向上进行拉伸，填充满整个侧轴方向(默认值)
	- baseline 文本基线
	- align-self: 设置单个元素在侧轴方向上的对齐方式(注意设置在子元素上)
