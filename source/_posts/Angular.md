---
title: Angular 1.0
tags: 
- Angular
categories:
- Angular
---

# Angular
### 简介
- 一款非常优秀的前端高级 JS 框架
- 最早由Misko Hevery等人创建
- 09被Google收购
- 可以轻松构建 SPA 应用程序
- 其核心就是通过指令扩展HTML,通过表达式绑定数据到HTML

### SPA
- single page application 单页应用程序

```html
//根据url中锚点值的变化，动态的请求不同的数据
<a href="#find"></a>
```
```javascript
window.addEventListener("hashchange",function(){
//hashchange监听url中锚点值变化的事件
    var hash= location.hash;
    //location.hash, location.href
    switch(hash){
        case '#find':
            $.get('find.json',function(data){
                console.log(data);
            },'json');
        break;
        case:...;
    }
})
```

### 指令
- 指令:在angular中以ng-开头的html标签属性
    + ng-app: 选择angular去管理哪一部分的html代码, 管理的是ng-app所在元素的子元素及其本身
    + ng-model: 指定一个属性值，这个属性就表示当前的value值,只能用在input中和select中
    + ng-init: 可以对数据进行初始化操作，给一个默认值
    + ng-click: 注册点击事件

```html
<!-- 约定ng-app -->
<body ng-app>
    <!-- 获取值ng-model -->
    <input type="text" ng-model="val">
    <!-- 添加点击事件ng-click -->
    <input type="text" value="+1" ng-click="myVal-0+1">
    <div>{{val}}</div>
    <!-- 初始化ng-init -->
    <div ng-init="val=1"></div>
</body>
<!-- 引包 -->
<script src="./libs/angular.js"></script>
```

### 模块
- 创建模块`var app = angular.module('模块名',[])`
- 如果不依赖其他模块，也需要提供一个空的数组
- 需要在ng-app指令的属性值上写我们的模块名
- 模块的划分方式
    1. 按照项目的功能去划分模块
    2. 按照项目中文件的类型去划分模块

### 控制器
- 创建控制器`app.controller('控制器的名字',function($scope){})`
- 要在它所在标签或者父标签上加上ng-controller指令,ng-controller的值就是我们想要显示的数据模型所在控制器的名字

### 双向数据绑定
- 数据模型($scope.属性)的改变，会影响内容的显示(文本框的值)
- 改变文本框的值，对应的数据模型发生了改变

```html
<body ng-app="hello">
    <input type="text" ng-model="name">
    <span>{{name}}</span>
    <input type="button" value="打印值" ng-click="getName()">
    <input type="button" value="改变为小红" ng-click="setName()">
    <script src="./libs/angular.js"></script>
    <script>
        // 创建模块
        var app=angular.module('hello',[]);
        // 创建控制器
        app.controller('helloController',function($scope){
            // 初始化数据模型
            $scope.name="小明";
            // 初始化行为模型
            $scope.getName=function(){
                console.log($scope.name)
            };
            $scope.setName=function(){
                $scope.name="小红"
            }
        })
    </script>
</body>
```

### MVC 思想
- M : Model: 存储，获取数据
- V : View 视图，把数据呈现给用户
- C : Controller 做一些控制和调度的操作
- MVC只是一种思想，只是约定了我们代码应该如何去组织,让我们代码的每一部分都有一个明确的职责,用利于后期的维护性

```html
<body ng-app="app">
  <!-- view 开始 -->
  <table ng-controller="demoController">
    <tr><td>用户名: <input type="text" ng-model="username"></td></tr>
    <tr><td>密码: <input type="text" ng-model="userpwd"></td></tr>
    <tr><td>用户类型:
        <select ng-model="type">
          <option value="tmp01">学生</option>
          <option value="tmp02">老师</option>
          <option value="tmp03">校长</option>
        </select>
    </td></tr>
    <tr><td>注册协议: <input type="checkbox" ng-model="isAgree"></td></tr>
    <tr><td><input type="button" value="注册" ng-click="register()"></td></tr>
    <tr><td>{{msg}}</td></tr>
  </table>
  <!-- View 结束 -->
  <script src="libs/angular.js"></script>
  <script>
    var app = angular.module('app',[])
    // Controller 开始
    app.controller('demoController',function($scope){
      // 初化数据模型
      $scope.username = '';
      $scope.usertype = '';
      $scope.userpwd = '';
      $scope.isagree = false;
      $scope.msg = ''
      // 初始化行为模型
      $scope.register = function(){
        if($scope.userpwd.length<6){
          $scope.msg='密码太短,不安全!';
          return
        }
        // 准备存储数据
        // 调用用Model里的方法
        var u = new User($scope.username,$scope.userpwd,$scope.type);
        var result = u.save();
        if(result){
          $scope.msg = '注册成功!'
        }else{
          $scope.msg = '注册失败！'
        }
      }
    })
    // Controller 结束
    // Model 开始
    function User(name,pwd,type){
      this.name = name;
      this.pwd = pwd;
      this.type = type
    }
    User.prototype.save = function(){
      // 判断用户名是否存在
      var str = localStorage.getItem('myusers')||'[]';
      // 注意'[]'
      var arr = JSON.parse(str);
      for (var i = 0; i < arr.length; i++) {
        if(arr[i].name===this.name){
          return 
        }
      }
      arr.push({name:this.name,pwd:this.pwd,type:this.type});
      localStorage.setItem('myusers',JSON.stringify(arr));
      return true
    }
  </script>
</body>
```

### Angular开发流程回顾
- 1.引包:引入angular.js文件
- 2.约定:加上ng-app指令，告诉angular要管理页面哪一部分代码
- 3.创建模块:在js中创建模块,给页面中的ng-app指令一个值，这个值就是这个模块的模块名
- 4.创建控制器:在js中创建控制器,需要在页面上加上ng-controller指令，指令的值为控制器的名字
- 5.建模,(数据模型,行为模型)根据页面结构，抽象出具体的js对象.
- 6.初始化,通过$scope做一些初始化操作
- 7.通过`ng-model , ng-click , 插值表达式` 把$scope的属性在页面展示出来
- 8.在js写一些具体的逻辑

### augular.element (不推崇)
- (jqLite对象) 类似于jq,但是要求传入的参数是一个原生的dom对象，而不是选择器

### $scope
- 视图和控制器之间的数据桥梁,用于在视图和控制器之间传递数据,用来暴露数据模型（数据，行为）
> 正是因为$scope在Angular中大量使用甚至盖过了C（控制器）的概念，所以很多人把Angular称之为MVVM框架,$scope 实际上就是MVVM中所谓的VM（视图模型）

## 控制器
### 传统的方式创建控制器
```javascript
    // 创建控制器(1.2.x版本)
    // angular会把全局的函数当作控制器
    function demoController($scope){
      $scope.name = '小明'
    }
```

### 面向对象的方式创建控制器
```html
<!-- 这里的obj 代表控制器中回调函数new 出的对象 -->
<div ng-controller="demoController as obj">
  <p>{{myname}}</p>
  <p>{{obj.name}}</p>
</div>
```
```javascript
    var app = angular.module('myApp', [])
    // angular会把这二个参数当作构造函数使用
    app.controller('demoController', function($scope){
      $scope.myname='小红'
      this.name = '小明'
    })
```

### 安全的方式创建控制器
- 为了避免压缩后代码无法运行
```javascript
    // 把第二个参数改为一个数组,在数组把我们需要的参数的名字写上
    // 回调函数就写在数组的最后一个元素上
    // 数据中传入的元素的顺序,要和function的中顺序一一对应
    app.controller('demoController',['$scope','$log',function($scope,$log){
      $scope.msg = 'hello World!'
      $log.log('哈哈哈哈！')
      //注意$log.log()
    }])
// 注意是'$scope'
```

## 指令
### ng-bind
- `<p ng-bind="msg"></p>` 
> 解决表达式闪烁问题,浏览器不会把标签的属性显示出来,angular会把ng-bind对应的数据显示到所在标签中间

### ng-cloak
- `<div class="ng-cloak">{{msg}}</div>` 
> 也可以解决表达闪烁问题,注意ng-cloak是样式,我们要先给ng-cloak设置display:none,angular在解析表达式之后会把页面上的ng-cloak样式移除,这样ng-cloak对应的样式就不生效了

### ng-repeat
- 能够把一组数据直接渲染到页面上,不需要我们拼接字符串
- ng-repeat="item in users",item对应是遍历users时的每一条数据,users是我们要遍历的数据(是一个数组)

```html
<body ng-app="myApp">
  <div ng-controller="demoController">
    <ul>
      <li ng-repeat="item in users" >
        {{item.name}} {{item.age}}
      </li>
    </ul>
  <ul>
    <li ng-repeat="item in obj">
      {{item.name}} {{item.age}}
    </li>
  </ul>
  <ul>
    <li ng-repeat="item in arr track by $index">
      {{item}} {{$index}}
    </li>
  </ul>
  </div>
  <script src="libs/angular.js"></script>
  <script>
    var app = angular.module('myApp', [])
    app.controller('demoController',['$scope',function($scope){
      $scope.users = [
        {name:'小明',age:18},
        {name:'小红',age:18},
        {name:'小朋',age:28},
        {name:'小月',age:19},
        {name:'小黑',age:18},
        {name:'小白',age:20}
      ]
      $scope.obj = {
        xm:{name:'小明',age:18},
        xh:{name:'小红',age:28},
        xt:{name:'小天',age:10},
        xy:{name:'小月',age:38}
      }
      $scope.arr = [1,2,3,4,5,1]
    }])
    // 注意:item in arr track by $index
  </script>
</body>

```

- ng-repeat 在遍历时会提供以下值:
    + $odd : 为true时，表明当前数据是否是第[奇]数条
    + $even: 为true时，表明当前数据是否是第[偶]数条
    + $first: 为true时，表明当前数据是否是第1条
    + $last: 为true时，表明当前数据是否是最后一条
    + $middle: 为true时，表明当前数据是否是中间的数据

```html
      <li class="{{ $odd ?'red':'green'}}" ng-repeat="item in data">
        {{item.name}},{{item.age}}
      </li>
```

### ng-class
- 动态的添加class样式,以对象的形式书写，angular会把属性值为true的属性名当作样式添加到class

```html
    <!--  ng-class,动态的添加class样式,
      以对象的形式书写，angular会把属性值为true的属性名当作样式添加到class-->
    <li ng-class="{red:item.age>=20, green:item.age>=10&&item.age<20,blue:item.age<10}" ng-repeat="item in data">
      {{item.name}},{{item.age}}
    </li>
    <!-- 注意格式ng-class="{classname:true,classname2:true,...}" -->
```

### ng-show/ng-hide
- ng-show,控制元素的显示或隐藏,值为true时显示，为false隐藏,ng-hide作用是相反的(只是设置display:none来隐藏元素)

```html
<body ng-app="myApp">
  <div ng-controller="demoController">
    <p ng-show="isShowing">hello</p>
    <button ng-click="showOrHide()">显示/隐藏</button>
  </div>
  <script src="libs/angular.js"></script>
  <script>
    var app = angular.module('myApp', [])
    app.controller('demoController', ['$scope', function($scope){
        $scope.isShowing= true;
        $scope.showOrHide=function(){
          $scope.isShowing = !$scope.isShowing
        }
    }])
  </script>
</body>
```

### ng-if
- 与ng-show用法一致,注意当值为false时,会将当前dom元素移除

### ng-switch
- 当ng-switch-when对应的值等于ng-switch对应值时，当前dom元素就显示

```html
<body ng-app="myApp">
<div ng-controller="demoController">
    <div ng-switch="name">
        <div ng-switch-when="小明">
            我是小明
        </div>
    </div>
</div>
<script src="libs/angular.js"></script>
<script>
    var app = angular.module('myApp', [])
    app.controller('demoController',['$scope', function($scope){
        $scope.name = '小明'
    }])
    // 注意ng-switch-when
</script>
```

### 其他常用指令
- ng-model:双向数据绑定
- ng-checked：单选/复选是否选中(单向数据绑定,界面操作不会影响数据模型的值)
- ng-selected：是否选中(单向数据绑定)
- ng-disabled：是否禁用
- ng-readonly：是否只读

### 常用事件指令
- ng-blur：失去焦点
- ng-focus：获得焦点 
- ng-change：内容改变
- ng-copy：复制
- ng-click: 单击
- ng-dblclick：双击
- ng-submit：form表单提单

### 指令的其他使用方式
- 兼容H5,在使用angular指令时,只需要在原先的指令前加上data-或x-,如:data-ng-app,x-ng-click

## todomvc案例
### todomvc功能分析
1. 任务的展示
2. 添加任务
3. 删除任务
4. 修改任务内容
5. 切换任务完成与否的状态
6. 批量切换任务完成与否的状态
7. 显示未完成的任务数
8. 清除所有已完成任务
    - 注意： 在循环删除数组长度，会导致循环条件改变，也会导致元素原来的索引改变
9. 切换显示不同状态的任务

### 过滤器(filter)
- 格式化数据
- 过滤数据(filter)
- currency,date
```html
  <!-- 在数据模型后加上|  再加上过滤器的名字 
        也可以在过滤器名字后指定参数，参数是写在冒号后面的-->
  <p>{{money | currency : '￥'}}</p>
  <p>{{myDate | date : 'yyyy年MM月dd日 HH:mm:ss'}}</p>
```
- lowercase 转换成小写,uppercase 转换成大写,number 四舍五入
- limitTo
```html
  <!-- limitTo第一个参数，表明显示多少个字，第二个参数表示，从第几个字开始显示(索引从0开始) -->
  <p>{{msg | limitTo : 5 : 2}}...</p>
```
- orderBy 及 json
```html
 <!--  json格式化显示json数据，参数指定缩近的长度 -->
 <pre>{{myJson | json : 8}}</pre>
 <!-- orderBy对数据进行排序，参数，给+号就按正序排，- 就按倒序排 -->
 <span ng-repeat="item in arr | orderBy:'-'">{{item}},</span>
```
- 在js中使用过滤器
```javascript
     app.controller('filterController', ['$scope','$filter',function($scope,$filter){// $filter 需要在控制器的回调中传入
        $scope.money = 9998;
        // 可以调用不同的过滤器得到相应的结果
        // 参数是一个过滤器的名字
        // 返回值是一个方法
        // 后面括号里的第一个参数是需要处理的数据
        // 后面括号里的的第二个参数是当前过滤器本身需要的参数
        // $filter('过滤器名称')(需要过滤的数据,filter 参数)
        $scope.result =  $filter('currency')($scope.money,'￥')
    }])
```
- filter
> `数据模型 | filter : {key:value}`
```html
<body ng-app="filterApp" ng-controller="fitlerController">
  <div>
    <input type="text" ng-model="search">
    <ul>
      <li ng-repeat="item in todos | filter : {completed:true} ">
        {{item.name}},{{item.completed}}
      </li>
      <!-- <li ng-repeat="item in todos | filter : search ">
        {{item.name}},{{item.completed}}
      </li> -->
    </ul>
    <!--  如果指定一个布尔值，或者字符串就是全文匹配 -->
    <!-- 数据精确过滤  参数：1、scope的属性，2、自定的字符串  3、自定的对象 {}-->
    <!-- 会到对应的todos中寻找，如果当前元素有completed属性且值 为true就会被显示出来。（只会到completed属性中寻找） -->
  </div>
  <script src="libs/angular.js"></script>
  <script>
      var app = angular.module('filterApp', [])
      app.controller('fitlerController', ['$scope',function($scope){
          $scope.search='';
          $scope.todos = [
              {id:1,name:'吃饭',completed:true},
              {id:2,name:'睡觉吃饭',completed:true},
              {id:3,name:'打豆豆',completed:false},
              {id:4,name:'学习', completed:true},
              {id:5,name:'喝水,true',completed:false}
          ]
      }])
  </script>
</body>
```

### $watch 监视数据模型的变化
```html
<body ng-app="hello">
  <div ng-controller="helloController">
    <input type="text" ng-model="name">
    <input type="text" ng-model="age">
    <button>测试</button>
  </div>
  <script src="libs/angular.js"></script>
  <script>
    var app = angular.module('hello', [])
    app.controller('helloController',['$scope',function($scope){
      $scope.name = '小明'
      $scope.age = 18
      // $watch可以用来监视数据模型的变化
      // 第一个参数: 数据模型对应的名字(字符串形式)
      // 第二个参数: 相应的数据模型变化就会调用这个函数
      // 默认会直接执行一次回调函数
      $scope.$watch('name',function(now,old){
        // 第一个参数是变化后的值
        // 第二个参数是变化前的值
        console.log(now,old)
      })

      $scope.getAge = function(){
        return $scope.age
      }
      // 也能够监视$scope.属性中的方法的返回值
      $scope.$watch('getAge()',function(now,old){
        console.log(now,old)
      })
      // $watch监视的是$scope的属性，如果是一个普通变量是无法监视的
      // function getName(){
      //   return $scope.name
      // }
      // $scope.tmp = getName
      // $scope.$watch('tmp()',function(now,old){
      //   console.log(now,old)
      // })
    }])
  </script>
</body>
```

## 自定义属性
```html
<body ng-app="directiveApp">
  <!-- 以属性形式使用 -->
  <!-- <div my-btn></div> -->
  <!-- 以样式名形式使用 -->
  <!-- <div class="my-btn"></div> -->
  <!-- 以自定义标签形式使用 -->
  <my-btn mystyle="red">咦</my-btn>
<script id='tpl' type='text/ng-template'>
  <div>
    <!-- <p class="tmp"> 我是在模板里定义的p,没有写ng-transclude</p> -->
    <p class="{{mystyle}}">hello</p>
    <p ng-transclude>hey</p>
  </div>
</script>
<script src="libs/angular.js"></script>
<script>
    var app = angular.module('directiveApp', [])
    // 创建自定义指令 directive
    // 第一个参数：是指令的名字,必须是驼峰命名法,使用时把大写改成小写，在原来大写前加上-
    // 第二个参数：和控制器的第二个参数类似
    app.directive('myBtn', [function(){
        // 在这里直接return 一个对象就可以了
        return {
            // template:'hello'
            // templateUrl:'template.html'
            templateUrl:'tpl',
            restrict:'ACE',
            replace:true,
            transclude: true,
            scope:{
              // tmp:'@mystyle',
              mystyle:'@',
            },
            link:function(scope,element,attributes){
                scope.msg="hello"
                console.log(element)
                element.on('click',function(){
                    alert('你点我了!')
                })
                console.log(attributes)
            }
        }
    }])
    //注意directive,type='text/ng-template',transclude
</script>
</body>
```

### 自定义指令中回调里返回的对象的属性
- template: 需要提供一个html字符串,会被添加到当前页面使用了自定义指令的位置

- templateUrl:
    - 需要提供一个html文件路径，angular会发请求，请求对应的文件，把文件内容作为模板插入到自定义指令中间
    - 提供一个script标签的id, angular会把script标签中的内容作为模板插入到自定义指令中间,注意要改变script标签的type="text/ng-template"

- restrict: 需要提供一个字符串，限制自定义指令的使用形式
    - A : Attribute 表示只能以属性的形式使用
    - C : Class     表示只能以类样式名的形式使用
    - E : Element   表示只能以自定义标签的形式使用
    - ACE : 如果希望多种方式合适，就把对应值组合在一起

- replace：需要提供一个布尔值，为true时，模板会被用来替换自定义指令所在标签，否则是插入到自定义指令中间

- transclude: 需要一个布尔值, 为true时会将自定义标签中的内容插入到模板中拥有ng-transclude指令的标签中间

- scope：需要一个对象: 可以用来获取自定义指令的属性值,
    -  给当前对象添加一个属性(如:tmp),属性值以@开头，后面跟上自定义指令的属性名
       然后就可以在模板中使用{{tmp}} 来得到对应的属性值,注意''
       + `scope: { tmp:'@mystyle'}` ` {{tmp}}`
       + `scope: { mystyle:'@'}`     `{{mystyle}}`

- link: 需要一个function 这是function在angular解析到相应指令时就会执行一次,
    + scope      ：类似于$scope,只不过scope的属性 **只能在模板中使用**
    + element    : 自定义指令所在标签对应的对象(jqLite),指向模板最外层的对象,如果replace为treu,指向的就是自定义指令所在标签
    + attributes : 包含了自定义指令所在标签的必有属性

### 服务
- 创建服务
```javascript
  var app = angular.module('service',[])
  // 第一个参数：服务的名字
  // 第二个参数里的function: 
  // angular会把这个function当作构建函数，angular会帮助我们new这个构建函数，然后得到一个对象
  app.service('MyService', [function(){
    this.name = '小明'
  }])
```
- 使用服务
```javascript
  var app = angular.module('todosApp', ['service'])
  app.controller('todosController',['MyService',function(MyService){
    // 这个MyService就是，对应的'MyService'时的回调函数new出的对象
    console.log(MyService)
}])
```

## 路由
- 根据url中不同的锚点值，在页面显示不同的内容

### 路由使用
```html
<body ng-app="myApp">
<div ng-view></div>
<script src="libs/angular.js"></script>
<script src="libs/angular-route.js"></script>
<script>
    var app=angular.module('myApp',['ngRoute'])
    //注意['ngRoute']

    // $location.url()可以得到url中锚点值#后的部分
    // 配置路由规则
    app.config(['$routeProvider',function($routeProvider){
    // 第一个参数：对应的url中锚点值
    // 当angular检测到url中锚点变为/a,就会把template对应的内容插入到页面拥有ng-view指令的标签中
        $routeProvider
            .when('/a',{
                template:'<div>{{msg}}</div>',
                controller:'demoController'
                // 指定一个控制器的名字,
                // 当前url中锚点值为/a时就会执行控制器中的回调函数
                // 我们可以直接在template/templateUrl对应的模板中使用该控制器中对应的$scope属性值
            })
            .when('/b',{
                template:'<div>hello</div>'
            })
    }])
    app.controller('demoController',['$scope',function($scope){
        $scope.msg="hi";
    }])
</script>
</body>
```

### 路由参数
```html
<body ng-app="myApp">
  <div ng-view></div>
  <script src="libs/angular.js"></script>
  <script src="libs/angular-route.js"></script>
  <script>
    var app=angular.module('myApp',['ngRoute']);
    app.config(['$routeProvider',function($routeProvider){
      $routeProvider.when('/company/:myname?',{
        //在原有规则中可以加冒号:,:号后跟上一个名字(相当于变量名，当前位置可以写成任意的值,但是必须要有) ?号表示当前位置如果没有值，也可以匹配到
        template:'<div>{{msg}}</div>',
        controller:'demoController'
      })
      .otherwise({redirectTo:'/company/a'})
      // 当不匹配when方法对应的规则时，就会匹配otherwise对应的规则
    }])
    app.controller('demoController',['$scope','$routeParams',function($scope,$routeParams){
      $scope.data={
        a:'hello',
        b:'hi',
        c:'hey'
      };
      $scope.msg=$scope.data[$routeParams.myname]
      // 在控制器中可以通过$routeParams得到对应的值
    }])
  </script>
</body>
```
