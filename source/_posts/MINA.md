---
title: 小程序开发
tags: 
- MINA
categories:
- MINA
---

> 因新工作主要负责微信小程序这一块，最近的重心就移到这一块，该博客是对微信小程序整体的整理归纳以及标明一些细节点，初学者建议直接看官方文档。

## 目录结构及配置

### app.json

- pages: 接受一个数组，每一项都是字符串，来指定小程序由哪些页面组成。每一项代表对应页面的【路径+文件名】信息，数组的第一项代表小程序的初始页面。小程序中新增/减少页面，都需要对 pages 数组进行修改。文件名不需要写文件后缀

- 参数说明:
  - window: 用于设置小程序的状态栏、导航条、标题、窗口背景色
    - navigationBarBackgroundColor 导航栏背景颜色
    - navigationBarTextStyle 导航栏标题颜色，仅支持 black/white
    - navigationBarTitleText 导航栏标题文字内容
    - navigationStyle 导航栏样式，仅支持 default/custom，custom 模式可自定义导航栏，只保留右上角胶囊状的按钮
    - backgroundColor 窗口的背景色
    - backgroundTextStyle 下拉 loading 的样式，仅支持 dark/light
    - backgroundColorTop 顶部窗口的背景色，仅 iOS 支持
    - backgroundColorBottom 底部窗口的背景色，仅 iOS 支持
    - enablePullDownRefresh 是否开启下拉刷新
    - onReachBottomDistance 页面上拉触底事件触发时距页面底部距离，单位为 px
  - tabbar
    - color tab 上的文字默认颜色
    - selectedColor tab 上的文字选中时的颜色
    - backgroundColor tab 的背景色
    - borderStyle tabbar 上边框的颜色， 仅支持 black/white
    - list tab 的列表，最少 2 个、最多 5 个 tab
      - pagePath 页面路径，必须在 pages 中先定义
      - texttab 上按钮文字
      - iconPath 图片路径，icon 大小限制为 40kb，建议尺寸为 81px \* 81px，当 postion 为 top 时，此参数无效，不支持网络图片
      - selectedIconPath 选中时的图片路径，icon 大小限制为 40kb，建议尺寸为 81px \* 81px ，当 postion 为 top 时，此参数无效
    - position 可选值 bottom、top
  - networkTimeout: 可以设置各种网络请求的超时时间
    - request wx.request 的超时时间，单位毫秒，默认为：60000
    - connectSockewx.connectSocket 的超时时间，单位毫秒，默认为：60000
    - uploadFile wx.uploadFile 的超时时间，单位毫秒，默认为：60000
    - downloadFile wx.downloadFile 的超时时间，单位毫秒，默认为：60000

### page.json

- 每一个小程序页面也可以使用.json 文件来对本页面的窗口表现进行配置，只是设置 app.json 中的 window 配置项的内容，页面中配置项会覆盖 app.json 的 window 中相同的配置项
- disableScroll 设置为 true 则页面整体不能上下滚动；只在 page.json 中有效，无法在 app.json 中设置该项

## 逻辑层(App Service)

- 增加 App 和 Page 方法，进行程序和页面的注册。
- 增加 getApp 和 getCurrentPages 方法，分别用来获取 App 实例和当前页面栈。
- 每个页面有独立的作用域，并提供模块化能力。
- 由于框架并非运行在浏览器中，所以 JavaScript 在 web 中一些能力都无法使用，如 document，window 等。
- 开发者写的所有代码最终将会打包成一份 JavaScript，并在小程序启动的时候运行，直到小程序销毁。类似 ServiceWorker，所以逻辑层也称之为 AppService。

### App

- App()函数用来注册一个小程序。接受一个 object 参数，其指定小程序的生命周期函数等
- 开发者可以添加任意的函数或数据到 Object 参数中，用 this 可以访问
- App() 必须在 app.js 中注册，且不能注册多个
- 全局的 getApp()函数可以用来获取到小程序实例，不要在定义于 App()内的函数中调用 getApp()，使用 this 就可以拿到 app 实例

- 参数说明
  - onLaunch 生命周期函数--当小程序初始化完成时，会触发 onLaunch（全局只触发一次）
  - onShow 生命周期函数--当小程序启动，或从后台进入前台显示，会触发 onShow
  - onHide 生命周期函数--当小程序从前台进入后台，会触发 onHide
  - onError 当小程序发生脚本错误，或者 api 调用失败时，会触发 onError 并带上错误信息
  - onPageNotFound 当小程序出现要打开的页面不存在的情况，会带上页面信息回调该函数
    - path 不存在页面的路径
    - query 打开不存在页面的 query
    - isEntryPage 是否本次启动的首个页面（例如从分享等入口进来，首个页面是开发者配置的分享页面）
  - onLaunch, onShow 参数
    - path 打开小程序的路径
    - query 打开小程序的 query
    - scene 打开小程序的场景值
    - shareTicket
    - referrerInfo 当场景为由从另一个小程序或公众号或 App 打开时，返回此字段
    - referrerInfo.appId 来源小程序或公众号或 App 的 appId
    - referrerInfo.extraData 来源小程序传过来的数据，scene=1037 或 1038 时支持

### Page

- Page()函数用来注册一个页面。接受一个 object 参数，其指定页面的初始数据、生命周期函数、事件处理函数等

- 参数说明
  - data 页面的初始数据
  - onLoad 生命周期函数--监听页面加载
    - 可以在 onLoad 中获取打开当前页面所调用的 query 参数
  - onReady 生命周期函数--监听页面初次渲染完成
    - 对界面的设置如 wx.setNavigationBarTitle 请在 onReady 之后设置
  - onShow 生命周期函数--监听页面显示
  - onHide 生命周期函数--监听页面隐藏
  - onUnload 生命周期函数--监听页面卸载
  - onPullDownRefresh 页面相关事件处理函数--监听用户下拉动作
  - onReachBottom 页面上拉触底事件的处理函数
  - onShareAppMessage 用户点击右上角转发
  - onPageScroll 页面滚动触发事件的处理函数
  - onTabItemTap 当前是 tab 页时，点击 tab 时触发
  - setData 函数用于将数据从逻辑层发送到视图层（异步），同时改变对应的 this.data 的值（同步）
  - onPullDownRefresh: 下拉刷新
    - 需要在 app.json 的 window 选项中或页面配置中开启 enablePullDownRefresh。
    - 当处理完数据刷新后，wx.stopPullDownRefresh 可以停止当前页面的下拉刷新。
  - onReachBottom: 上拉触底
    - 可以在 app.json 的 window 选项中或页面配置中设置触发距离 onReachBottomDistance。
    - 在触发距离内滑动期间，本事件只会被触发一次。
  - onPageScroll: 页面滚动
  - onShareAppMessage: 用户转发
    - 只有定义了此事件处理函数，右上角菜单才会显示“转发”按钮，用户点击转发按钮的时候会调用
    - 此事件需要 return 一个 Object，用于自定义转发内容

### 路由

- navigateTo,redirectTo,navigateBack,switchTab,reLaunch
- navigateTo, redirectTo 只能打开非 tabBar 页面，switchTab 只能打开 tabBar 页面，reLaunch 可以打开任意页面
- getCurrentPages() 函数用于获取当前页面栈的实例，以数组形式按栈的顺序给出，第一个元素为首页，最后一个元素为当前页面
- 调用页面路由带的参数可以在目标页面的 onLoad 中获取

### 模块化

- 在 JavaScript 文件中声明的变量和函数只在该文件中有效；不同的文件中可以声明相同名字的变量和函数，不会互相影响
- 通过全局函数 getApp() 可以获取全局的应用实例，如果需要全局的数据可以在 App() 中设置
- 可以将一些公共的代码抽离成为一个单独的 js 文件，作为一个模块，模块只有通过 module.exports 或者 exports 才能对外暴露接口
- 在需要使用这些模块的文件中，使用 require(path) 将公共代码引入，require 暂时不支持绝对路径
- 小程序目前不支持直接引入 node_modules , 开发者需要使用到 node_modules 时候建议拷贝出相关的代码到小程序的目录中

## 视图层(view)

### WXML

#### 数据绑定

- 使用 Mustache 语法，和 vue 的局别是可作为控制属性控制 class
- 花括号和引号之间如果有空格，将最终被解析成为字符串

#### 数组渲染(wx:for)

- 默认数组的当前项的下标变量名默认为 index(可使用 wx:for-item 指定名称)，数组当前项的变量名默认为 item(可使用 wx:for-index 指定名称)
- `wx:key` 如果列表中项目的位置会动态改变或者有新的项目添加到列表中，并且希望列表中的项目保持自己的特征和状态（如 `<input/>` 中的输入内容，`<switch/>` 的选中状态），需要使用 wx:key 来指定列表中项目的唯一的标识符。当数据改变触发渲染层重新渲染的时候，会校正带有 key 的组件，框架会确保他们被重新排序，而不是重新创建，以确保使组件保持自身的状态，并且提高列表渲染时的效率。
  - 字符串，代表在 for 循环的 array 中 item 的某个 property，该 property 的值需要是列表中唯一的字符串或数字，且不能动态改变
  - 保留关键字 `*this` 代表在 for 循环中的 item 本身，这种表示需要 item 本身是一个唯一的字符串或者数字
- wx:for 的值为字符串时，会将字符串解析成字符串数组

#### 条件渲染(wx:if)

- `wx:if` `wx:ifel` `wx:else` 使用后两个注意要是同级，可使用 block 进行包装

#### 模板(template)

- 使用 name 属性，作为模板的名字, 然后在 `<template/>` 内定义代码片段
- 使用 is 属性，声明需要的使用的模板，然后将模板所需要的 data 传入，is 属性可以使用 Mustache 语法
- 模板拥有自己的作用域，只能使用 data 传入的数据以及模版定义文件中定义的 `<wxs />` 模块

#### 事件

- bind 事件绑定不会阻止冒泡事件向上冒泡，catch 事件绑定可以阻止冒泡事件向上冒泡
- target 触发事件的源组件，currentTarget 事件绑定的当前组件
- 通过 detail 可获取自定义事件所携带的数据，通过 dataset 在组件中可以定义数据

#### 引用

- import
  - import 可以在该文件中使用目标文件定义的 template
  - import 有作用域的概念，即只会 import 目标文件中定义的 template，而不会 import 目标文件 import 的 template
- include
  - include 可以将目标文件除了 `<template/><wxs/>` 外的整个代码引入，相当于是拷贝到 include 位置

### WXS

- WXS 是微信小程序的一套脚本语言，语法和 js 并不完全一致，在 ios 上比 JS 运行速度快，但不能调用其他 js 文件中定义的函数和小程序的 API
- WXS 中的变量均为值的引用
- 没有声明的变量直接赋值使用，会被定义为全局变量
- WXS 代码可以编写在 wxml 文件中的 `<wxs>` 标签内，或以 .wxs 为后缀名的文件内
- 每一个 .wxs 文件和 `<wxs>` 标签都是一个单独的模块。
- 每个模块都有自己独立的作用域。即在一个模块里面定义的变量与函数，默认为私有的，对其他模块不可见。
- 每个 wxs 模块均有一个内置的 module 对象，一个模块要想对外暴露其内部的私有变量与函数，只能通过 module.exports 实现
- 在.wxs 模块中引用其他 wxs 文件模块，可以使用 require 函数
- 引用 wxs 模块必须使用相对路径(不论是 require 还是`<wsx>`的 src 引用)，且 wxs 模块均为单例，wxs 模块在第一次被引用时，会自动初始化为单例对象。多个页面，多个地方，多次引用，使用的都是同一个 wxs 模块对象。如果一个 wxs 模块在定义之后，一直没有被引用，则该模块不会被解析与运行
- `<wsx>` 标签
  - module 属性: 当前 `<wxs>` 标签的模块名，必填字段
    - 在单个 wxml 文件内，建议其值唯一。有重复模块名则按照先后顺序覆盖（后者覆盖前者）。不同文件之间的 wxs 模块名不会相互覆盖。
    - 其模块名首字母不能为数字
  - src 属性: 用来引用其他的 wxs 文件模块，仅当本标签为单闭合标签或标签的内容为空时有效
- `<wxs>` 模块只能在定义模块的 WXML 文件中被访问到。使用 `<include>` 或 `<import>` 时，`<wxs>` 模块不会被引入到对应的 WXML 文件中。
- `<template>` 标签中，只能使用定义该 `<template>` 的 WXML 文件中定义的 `<wxs>` 模块。

### WXSS

- 设计师最好使用 iphone6 尺寸的设计稿
- app.wxss 的样式为全局样式
- 样式导入 `@import '相对路径' ;`

## 自定义组件

- 要编写一个自定义组件，首先需要在 json 文件中进行自定义组件声明（将 component 字段设为 true 可这一组文件设为自定义组件）
- 使用已注册的自定义组件前，首先要在页面的 json 文件中进行引用声明
- 因为 WXML 节点标签名只能是小写字母、中划线和下划线的组合，所以自定义组件的标签名也只能包含这些字符。
- 在组件 wxss 中不应使用 ID 选择器、属性选择器和标签名选择器
- 自定义组件的 wxml 节点结构在与数据结合之后，将被插入到引用位置内

### 组件模板

- 可以使用数据绑定，这样就可以向子组件的属性传递动态数据
- 在组件模板中可以提供一个 `<slot>` 节点，用于承载组件引用时提供的子节点
- 默认情况下，一个组件的 wxml 中只能有一个 slot，需要使用多 slot 时，可以在组件 js 中声明启用，以不同的 name 来区分，使用时，用 slot 属性来将节点插入到不同的 slot 上

### 组件样式

- 组件对应 wxss 文件的样式，只对组件 wxml 内的节点生效
- 可以在 Component 中用 externalClasses 定义段定义若干个外部样式类
- 组件和引用组件的页面不能使用 id 选择器（#a）、属性选择器（[a]）和标签名选择器
- 组件和引用组件的页面中使用后代选择器（.a .b）在一些极端情况下会有非预期的表现
- 子元素选择器（.a>.b）只能用于 view 组件与其子节点之间，用于其他组件可能导致非预期的情况
- 继承样式，如 font 、 color ，会从组件外继承到组件内。
- 除继承样式外， app.wxss 中的样式、组件所在页面的的样式对自定义组件无效
- 组件可以指定它所在节点的默认样式，使用 :host 选择器

### Component 构造器

- 在 properties 定义段中，属性名采用驼峰写法（propertyName）；在 wxml 中，指定属性值时则对应使用连字符写法（component-tag-name property-name="attr value"），应用于数据绑定时采用驼峰写法（attr="{{propertyName}}"）
- 使用 this.data 可以获取内部数据和属性值，但不要直接修改它们，应使用 setData 修改
- 生命周期函数无法在组件方法中通过 this 访问到
- 属性名应避免以 data 开头，即不要命名成 dataXyz 这样的形式，因为在 WXML 中， data-xyz="" 会被作为节点 dataset 来处理，而不是组件属性
- 在一个组件的定义和使用时，组件的属性名和 data 字段相互间都不能冲突（尽管它们位于不同的定义段中）
- bug : 对于 type 为 Object 或 Array 的属性，如果通过该组件自身的 this.setData 来改变属性值的一个子字段，则依旧会触发属性 observer ，且 observer 接收到的 newVal 是变化的那个子字段的值， oldVal 为空， changedPath 包含子字段的字段名相关信息

- 参数说明

  - properties 组件的对外属性，可包含三个字段， type 表示属性类型、 value 表示属性初始值、 observer 表示属性值被更改时的响应函数
  - data 组件的内部数据
  - methods 组件的方法，包括事件响应函数和任意的自定义方法
  - behaviors 组件间代码复用
  - created 在组件实例进入页面节点树时执行，注意此时不能调用 setData
  - attached 在组件实例进入页面节点树时执行
  - ready 在组件布局完成后执行，此时可以获取节点信息
  - moved 在组件实例被移动到节点树另一个位置时执行
  - detached 在组件实例被从页面节点树移除时执行
  - relations 组件间关系定义
  - externalClasses 组件接受的外部样式类

  - is 组件的文件路径
  - id 节点 id
  - dataset 节点 dataset

  - setData 设置 data 并执行视图层渲染
  - hasBehavior 检查组件是否具有 behavior （检查时会递归检查被直接或间接引入的所有 behavior）
  - triggerEvent String name, Object detail, Object options 触发事件
  - createSelectorQuery 创建一个 SelectorQuery 对象，选择器选取范围为这个组件实例内
  - selectComponent 使用选择器选择组件实例节点，返回匹配到的第一个组件实例对象
  - selectAllComponents 使用选择器选择组件实例节点，返回匹配到的全部组件实例对象组成的数组
  - getRelationNodes 获取所有这个关系对应的所有关联节点

### 组件通信：

- 1.  WXML 数据绑定：用于父组件向子组件的指定属性设置数据
- 2.  事件：用于子组件向父组件传递数据，可以传递任意数据
  - 监听事件：事件系统是组件间通信的主要方式之一。自定义组件可以触发任意的事件，引用组件的页面可以监听这些事件。
  - 触发事件：自定义组件触发事件时，需要使用 triggerEvent 方法，指定事件名、detail 对象和事件选项
    - bubbles 事件是否冒泡
    - composed 事件是否可以穿越组件边界，为 false 时，事件将只能在引用组件的节点树上触发，不进入其他任何组件内部
    - capturePhase 事件是否拥有捕获阶段
- 3.  如果以上两种方式不足以满足需要，父组件还可以通过 this.selectComponent 方法获取子组件实例对象，这样就可以直接访问组件的任意数据和方法

### behaviors

- 每个 behavior 可以包含一组属性、数据、生命周期函数和方法，组件引用它时，它的属性、数据和方法会被合并到组件中，生命周期函数也会在对应时机被调用
- 每个组件可以引用多个 behavior 。 behavior 也可以引用其他 behavior
- behavior 需要使用 Behavior() 构造器定义
- 组件和它引用的 behavior 中可以包含同名的字段，对这些字段的处理方法如下：
  - 如果有同名的属性或方法，组件本身的属性或方法会覆盖 behavior 中的属性或方法，如果引用了多个 behavior ，在定义段中靠后 behavior 中的属性或方法会覆盖靠前的属性或方法；
  - 如果有同名的数据字段，如果数据是对象类型，会进行对象合并，如果是非对象类型则会进行相互覆盖；
  - 生命周期函数不会相互覆盖，而是在对应触发时机被逐个调用。如果同一个 behavior 被一个组件多次引用，它定义的生命周期函数只会被执行一次。
- 内置 behaviors: 自定义组件可以通过引用内置的 behavior 来获得内置组件的一些行为
  - behaviors: ['wx://form-field'] 使自定义组件有类似于表单控件的行为。 form 组件可以识别这些自定义组件，并在 submit 事件中返回组件的字段名及其对应字段值。这将为它添加以下两个属性
    - name 在表单中的字段名
    - value 在表单中的字段值

### 组件间关系

- 在组件定义时加入 relations 定义段可以定义组件关系，必须在两个组件定义中都加入 relations 定义
- 如果这两个组件都有同一个 behavior，则在 relations 关系定义中，可使用这个 behavior 来代替组件路径作为关联的目标节点
- relations 定义段
  - type 目标组件的相对关系，可选的值为 parent 、 child 、 ancestor 、 descendant
  - linked 关系生命周期函数，当关系被建立在页面节点树中时触发，触发时机在组件 attached 生命周期之后
  - linkChanged 关系生命周期函数，当关系在页面节点树中发生改变时触发，触发时机在组件 moved 生命周期之后
  - unlinked 关系生命周期函数，当关系脱离页面节点树时触发，触发时机在组件 detached 生命周期之后
  - target 如果这一项被设置，则它表示关联的目标节点所应具有的 behavior，所有拥有这一 behavior 的组件节点都会被关联

### 抽象节点

- 有时，自定义组件模版中的一些节点，其对应的自定义组件不是由自定义组件本身确定的，而是自定义组件的调用者确定的。这时可以把这个节点声明为“抽象节点”
- 抽象节点可以指定一个默认组件，当具体组件未被指定时，将创建默认组件的实例。默认组件可以在 componentGenerics 字段中指定
- 节点的 generic 引用 generic:xxx="yyy" 中，值 yyy 只能是静态值，不能包含数据绑定。因而抽象节点特性并不适用于动态决定节点名的场景

## 插件

### 开发插件

- 新建插件类型的项目后，如果创建示例项目，则项目中将包含两个目录：
  - plugin 目录：插件代码目录
    - 包含若干个自定义组件、页面，和一组 js 接口
    - plugin.json
      - 主要说明有哪些自定义组件可以供插件外部调用，并标识哪个 js 文件是插件的 js 接口文件
    - 插件的 js 接口文件 index.js 中可以 export 一些 js 接口，插件的使用者可以使用 requirePlugin 来获得这些接口
  - miniprogram 目录：放置一个小程序，用于调试插件
  - doc 目录，用于放置插件开发文档
- 插件执行页面跳转的时候，只能使用 navigator 组件。当插件跳转到自身页面时， url 应设置为这样的形式：plugin-private://PLUGIN_APPID/PATH/TO/PAGE 。需要跳转到其他插件时，也可以这样设置 url
- 插件没有体验版。插件会同时有多个线上版本，由使用插件的小程序决定具体使用的版本号
- 插件开发文档必须放置在插件项目根目录中的 doc 目录下，引用到的图片资源不能是网络图片，必须放在这个目录下，在管理后台的基本设置中预览、发布插件文档

### 使用插件

- 使用插件前要在 app.json 中声明需要使用的插件，plugins 定义段中可以包含多个插件声明，每个插件声明中都必须指明插件的 appid 和需要使用的版本号
- 如果需要使用插件的 js 接口，可以使用 requirePlugin 方法
- 在 json 文件定义需要引入的自定义组件时，使用 plugin:// 协议即可
  - 页面中的 this.selectComponent 接口无法获得插件的自定义组件实例对象
  - wx.createSelectorQuery 等接口的 >>> 选择器无法选入插件内部
- 需要跳转到插件页面时， url 应使用 plugin:// 前缀

### 插件功能页

- 功能页是 插件所有者小程序 中的一个特殊页面。插件所有者小程序，指的是与插件 AppID 相同的小程序。新增或改变这个字段时，需要这个小程序发布新版本，才能在正式环境中使用插件功能页
- 启用插件功能页时，需要在插件所有者小程序 app.json 文件中添加 functionalPages 定义段，其值为 true
- 在插件需要登录时，可以在插件的自定义组件中放置一个 `<functional-page-navigator>` 组件。用户在点击这个区域时，会自动跳转到插件所有者小程序的功能页。功能页会提示用户进行登录或其他相应的操作。操作结果会以组件事件的方式返回
- 在使用支付功能页时，插件所有者小程序需要提供一个函数来响应支付请求。这个响应函数应当写在小程序根目录中的 functional-pages/request-payment.js 文件中，名为 beforeRequestPayment 。如果不提供这段代码，将通过 fail 事件返回失败
- 注意：功能页函数不应 require 其他非 functional-pages 目录中的文件，其他非 functional-pages 目录中的文件也不应 require 这个目录中的文件。这样的 require 调用在未来将不被支持
- 这个目录和文件应当被放置在插件所有者小程序代码中（而非插件代码中），它是插件所有者小程序的一部分（而非插件的一部分）。 如果需要新增或更改这段代码，需要发布插件所有者小程序，才能在正式版中生效；需要重新预览插件所有者小程序，才能在开发版中生效
- 如果使用插件的小程序就是插件所有者小程序，插件功能页不能使用

## 分包加载

- 某些情况下，开发者需要将小程序划分成不同的子包，在构建时打包成不同的分包，用户在使用时按需进行加载。
- 在构建小程序分包项目时，构建会输出一个或多个功能的分包，其中必定含有一个主包，所谓的主包，即放置默认启动页面/TabBar 页面，以及一些所有分包都需用到公共资源/JS 脚本，而分包则是根据开发者的配置进行划分。
- 在小程序启动时，默认会下载主包并启动主包内页面，如果用户需要打开分包内某个页面，客户端会把对应分包下载下来，下载完成后再进行展示。
- 目前小程序分包大小有以下限制：整个小程序所有分包大小不超过 8M; 单个分包/主包大小不能超过 2M

### 打包原则

- 声明 subPackages 后，将按 subPackages 配置路径进行打包，subPackages 配置路径外的目录将被打包到 app（主包） 中
- app（主包）也可以有自己的 pages（即最外层的 pages 字段）
- subPackage 的根目录不能是另外一个 subPackage 内的子目录
- 首页的 TAB 页面必须在 app（主包）内

### 引用原则

- packageA 无法 require packageB JS 文件，但可以 require app、自己 package 内的 JS 文件
- packageA 无法 import packageB 的 template，但可以 require app、自己 package 内的 template
- packageA 无法使用 packageB 的资源，但可以使用 app、自己 package 内的资源

## 多线程 Worker

> 一些异步处理的任务，可以放置于 Worker 中运行，待运行结束后，再把结果返回到小程序主线程。Worker 运行于一个单独的全局上下文与线程中，不能直接调用主线程的方法。 Worker 与主线程之间的数据传输，双方使用 Worker.postMessage() 来发送数据，Worker.onMessage() 来接收数据，传输的数据并不是直接共享，而是被复制的

1.  配置 Worker 信息: 在 app.json 中可配置 Worker 代码放置的目录，目录下的代码将被打包成一个文件
2.  添加 Worker 代码文件
3.  编写 Worker 代码
4.  在主线程中初始化 Worker
5.  主线程向 Worker 发送消息

- 注意
  - Worker 最大并发数量限制为 1 个，创建下一个前请用 Worker.terminate() 结束当前 Worker
  - Worker 内代码只能 require 指定 Worker 路径内的文件，无法引用其它路径
  - Worker 的入口文件由 wx.createWorker() 时指定，开发者可动态指定 Worker 入口文件
  - Worker 内不支持 wx 系列的 API
  - Workers 之间不支持发送消息

## 兼容

> 可以通过 wx.getSystemInfo 或者 wx.getSystemInfoSync 获取到小程序的基础库版本号。也可以通过 wx.canIUse 详情 来判断是否可以在该基础库版本下直接使用对应的 API 或者组件

## 运行机制

> 小程序启动会有两种情况，一种是「冷启动」，一种是「热启动」。 假如用户已经打开过某小程序，然后在一定时间内再次打开该小程序，此时无需重新启动，只需将后台态的小程序切换到前台，这个过程就是热启动；冷启动指的是用户首次打开或小程序被微信主动销毁后再次打开的情况，此时小程序需要重新加载启动。

### 更新机制

> 小程序冷启动时如果发现有新版本，将会异步下载新版本的代码包，并同时用客户端本地的包进行启动，即新版本的小程序需要等下一次冷启动才会应用上。 如果需要马上应用最新版本，可以使用 wx.getUpdateManager API 进行处理。

### 运行机制

> 小程序没有重启的概念。当小程序进入后台，客户端会维持一段时间的运行状态，超过一定时间后（目前是 5 分钟）会被微信主动销毁。当短时间内（5s）连续收到两次以上收到系统内存告警，会进行小程序的销毁

## 性能优化

### setData 优化

#### 工作原理

> 小程序的视图层目前使用 WebView 作为渲染载体，而逻辑层是由独立的 JavascriptCore 作为运行环境。在架构上，WebView 和 JavascriptCore 都是独立的模块，并不具备数据直接共享的通道。当前，视图层和逻辑层的数据传输，实际上通过两边提供的 evaluateJavascript 所实现。即用户传输的数据，需要将其转换为字符串形式传递，同时把转换后的数据内容拼接成一份 JS 脚本，再通过执行 JS 脚本的形式传递到两边独立环境。而 evaluateJavascript 的执行会受很多方面的影响，数据到达视图层并不是实时的

#### 错误操作

1.  频繁的去 setData: Android 下用户在滑动时会感觉到卡顿，操作反馈延迟严重，因为 JS 线程一直在编译执行渲染，未能及时将用户操作事件传递到逻辑层，逻辑层亦无法及时将操作处理结果及时传递到视图层；渲染有出现延时，由于 WebView 的 JS 线程一直处于忙碌状态，逻辑层到页面层的通信耗时上升，视图层收到的数据消息时距离发出时间已经过去了几百毫秒，渲染的结果并不实时；

2.  每次 setData 都传递大量新数据: 由 setData 的底层实现可知，我们的数据传输实际是一次 evaluateJavascript 脚本过程，当数据量过大时会增加脚本的编译执行时间，占用 WebView JS 线程，

3.  后台态页面进行 setData: 当页面进入后台态（用户不可见），不应该继续去进行 setData，后台态页面的渲染用户是无法感受的，另外后台态页面去 setData 也会抢占前台页面的执行。

### 图片资源优化

> 目前图片资源的主要性能问题在于大图片和长列表图片上，这两种情况都有可能导致 iOS 客户端内存占用上升，从而触发系统回收小程序页面。除了内存问题外，大图片也会造成页面切换的卡顿。当前我们建议开发者尽量减少使用大图片资源

### 代码包大小的优化

> 开发者在实现业务逻辑同时也有必要尽量减少代码包的大小，因为代码包大小直接影响到下载速度，从而影响用户的首次打开体验。除了代码自身的重构优化外，还可以从这两方面着手优化代码大小：

#### 控制代码包内图片资源

> 小程序代码包经过编译后，会放在微信的 CDN 上供用户下载，CDN 开启了 GZIP 压缩，所以用户下载的是压缩后的 GZIP 包，其大小比代码包原体积会更小。不同小程序之间的代码包压缩比差异也挺大的，部分可以达到 30%，而部分只有 80%，而造成这部分差异的一个原因，就是图片资源的使用。GZIP 对基于文本资源的压缩效果最好，在压缩较大文件时往往可高达 70%-80% 的压缩率，而如果对已经压缩的资源（例如大多数的图片格式）则效果甚微。

#### 及时清理没有使用到的代码和资源

> 在日常开发的时候，我们可能引入了一些新的库文件，而过了一段时间后，由于各种原因又不再使用这个库了，我们常常会只是去掉了代码里的引用，而忘记删掉这类库文件了。目前小程序打包是会将工程下所有文件都打入代码包内，也就是说，这些没有被实际使用到的库文件和资源也会被打入到代码包里，从而影响到整体代码包的大小。

> 参考自https://developers.weixin.qq.com/miniprogram/dev/index.html
