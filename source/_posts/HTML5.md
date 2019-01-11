---
title: 回顾HTML5
tags: 
- HTML5
categories:
- HTML5
---

### 新增的标签
- 多媒体标签
    - audio
        - src,url 要播放的URL
        - autoplay 自动播放
        - preload 在页面加载时进行加载,并预备播放
        > 如果使用 "autoplay",则忽略该属性
        - loop 循环播放
        - controls 向用户显示控件,比如播放按钮
    - video
        - src,url
        - autoplay
        - preload
        - loop
        - controls
        - width height 设置视频播放器的宽度高度
        > 一般只设置宽度或者高度
        - poster 指定视频还没有完全下载完毕,或者用户没有点击播放前显示的封面,默认显示当前视频文件的第一副图像

        > 因为不同的浏览器所支持的视频格式不一样,为了保证用户能够看到视频,我们可以提供多个视频文件让浏览器自动选择
        ```html
        <video controls>
            <source src="mp3/flv.flv" type="video/flv">
            <source src="mp3/mp4.mp4" type="video/mp4">
        </video>
        ```

- fieldset 带标题的框
> 用法:form fieldset legend(为fieldset元素定义标题)
- progress 进度条, meter 度量器 用于标示级别
    - high:number 定义度量的值位于哪个点,被界定为高的值
    - low:number 定义度量的值位于哪个点,被界定为低的值
    - max:number 定义最大值,默认值是 1
    - min:number 定义最小值,默认值是 0
    - optimum:number 定义什么样的度量值是最佳的值
    - value:number 定义度量的值
- datalist 下拉列表(不常用)
- keygen (不常用)
- output 用于展示内容,只能展示,不能进行编辑(不常用)

### 新增的表单属性:
- placeholder 占位符
- required 验证条件,必填项
- pattern 正则表达式,验证表单
- autofocus 获取焦点
- autocomplete 自动完成
> on:打开,off:关闭,
> 必须成功提交过,且添加autocomplete的元素必须有name属性
- multiple 文件上传多选或多个邮箱地址
- novalidate 关闭验证,可用于form标签
- form 指定表单项属于哪个form
> 处理复杂表单时会需要,当某个表单元素并没有包含在form中时,可在属性中用form属性指定表单id

### 新增的表单输入类型
- search： 搜索框
- email : 提供了默认的电子邮箱的完整验证
- tel： 能够在移动端打开数字键盘
- number： 只能输入数字(value=""max=""min="")
- url： 只能输入url格式(必须包含http://)
- file
- time：时间
- date：日期
- range： 范围,可以进行拖动(value=""max=""min="")
- color：拾色器,可通过value进行取值
- datetime： 时间日期(不常用)
- month： 月份
- week： 星期

### 新增的表单事件：
- oninput 用户输入内容时触发,可用于移动端输入字数统计
- oninvalid 验证不通过时触发

### DOM扩展
1. 获取元素：
    - document.querySelector('selector')
    > 获取单个元素,如果获取的元素不止一个,那么只会返回满足条件的第一个元素。如果是类选择器,必须添加.,如果是id选择器,必须添加# ,否则当成标签处理
    - document.querySelectorAll('selector')
    > 获取满足条件的所有元素,以类数组形式存在
2. 类名操作：
    - classList.add('class')
    - classList.remove('class')
    - classList.toggle('class')
    - classList.contains('class') 检测是否存在指定class
    - classList.item(n) 获取样式
3. 自定义属性
```html
<p data-school-name="itcast"></p>
```
```javascript
var value=p.dataset["schoolName"];
```

### 网络接口
- ononline:网络连通的时候触发这个事件
- onoffline:网络断开时触发
```javascript
window.addEventListener("online",function(){
    alert("网络连通了");
});
window.addEventListener("offline",function(){
    alert("网络断开了");
})
```

### 全屏接口
- requestFullScreen():开启全屏显示
- cancelFullScreen():退出全屏显示
- fullScreenElement:是否是全屏状态
> 
1. 不同浏览器需要添加不同的前缀chrome:webkit   firefox:moz   ie:ms   opera:o
2. document.fullscreenElement||document.webkitFullscreenElement||document.mozFullScreenElement||document.msFullscreenElement 注意此时的拼写

```javascript
window.onload=function(){
    var div=document.querySelector("div");
    /*全屏操作*/
    document.querySelector("#full").onclick=function(){
        if(div.requestFullScreen){
            div.requestFullScreen();
        }
        else if(div.webkitRequestFullScreen){
            div.webkitRequestFullScreen();
        }
        else if(div.mozRequestFullScreen){
            div.mozRequestFullScreen();
        }
        else if(div.msRequestFullScreen){
            div.msRequestFullScreen();
        }
    }
    /*退出全屏*/
    document.querySelector("#cancelFull").onclick=function(){
        if(document.cancelFullScreen){
            document.cancelFullScreen();
        }
        else if(document.webkitCancelFullScreen){
            document.webkitCancelFullScreen();
        }
        else if(document.mozCancelFullScreen){
            document.mozCancelFullScreen();
        }
        else if(document.msCancelFullScreen){
            document.msCancelFullScreen();
        }
    }
    /*判断是否是全屏状态*/
    document.querySelector("#isFull").onclick=function(){
        if(document.fullscreenElement || document.webkitFullscreenElement || document.mozFullScreenElement || document.msFullscreenElement){
            alert(true);
        }
        else{
            alert(false);
        }
    }
}
```

### FileReader 读取文件内容
> 
    - .readAsText():读取文本文件,返回文本字符串,默认编码是UTF-8
    - .readAsBinaryString():读取任意类型的文件,返回二进制字符串
    这个方法不是用来读取文件展示给用户看,而是存储文件
    例如：读取文件的内容,获取二进制数据,传递给后台,后台接收了数据之后,再将数据存储
    - .readAsDataURL():
    读取文件,获取一段以data开头的字符串,
    这段字符串的本质就是DataURL,
    DataURL是一种将文件嵌入到文档的方案,
    (一般就是指图像或者能够嵌入到文档的文件格式)DataURL是将资源转换为base64编码的字符串形式,
    并且将这些内容直接存储在url中,优化网站的加载速度和执行效率

FileReader提供一个完整的事件模型,用来捕获读取文件时的状态
> 
    - onabort:读取文件中断片时触发
    - onerror:读取错误时触发
    - onload:文件读取成功完成时触发
    - onloadend:读取完成时触发,无论成功还是失败
    - onloadstart:开始读取时触发
    - onprogress:读取文件过程中持续触发
    - abort():中断读取

```html
<form action="">
    文件： <input type="file" name="myFile" id="myFile" onchange="getFileContent();"> <br>
    <input type="submit">
</form>
<img src="" alt="">
```
```javascript
var div = document.querySelector("div");
function getFileContent() {
    var file = document.querySelector("#myFile").files[0];
    //文件存储在file表单元素的files属性中,它是一个数组
    var reader = new FileReader();
    //创建文件读取对象
    reader.readAsDataURL(file);
    //读取文件,获取DataURL
    //需要传递一个参数 binary large object:文件(图片或者其它可以嵌入到文档的类型)
    //读取完文件之后,它会将读取的结果存储在文件读取对象的result中,void说明没有任何的返回值
    reader.onload = function () {
        document.querySelector("img").src = reader.result;
    }
}
```

### 拖拽接口
- 应用于拖拽元素:
    + ondragstart 拖拽开始时调用
    + ondragleave 鼠标离开拖拽元素时调用
    + ondrag 整个拖拽过程都会调用--持续
    + ondragend 拖拽结束时调用
- 应用于目标元素:
    + ondragenter 当拖拽元素进入时调用
    + ondragleave 当鼠标离开目标元素时调用
    + ondragover 当停留在目标元素上时调用
    + ondrop 当在目标元素上松开鼠标时调用
 
> 注: 在h5中,如果想拖拽元素,就必须为元素添加标签属性draggable="true",图片和超链接默认就可以拖拽

### dataTransfer
1. 通过dataTransfer来实现数据的存储
e.dataTransfer.setData(format,data); 
format:数据的类型：text/html text/uri-list
Data:数据:一般来说是字符串值
`e.dataTransfer.setData("text/html", e.target.id);`
2. 通过e.dataTransfer.setData存储的数据,只能在drop事件中获取
e.dataTransfer.getData(format,data);
`var id = e.dataTransfer.getData("text/html");`
`e.target.appendChild(document.getElementById(id));`

```javascript
document.ondragstart = function (e) {
    e.target.style.opacity = 0.5;
    e.dataTransfer.setData("text/html", e.target.id);
}
document.ondragend = function (e) {
    e.target.style.opacity = 1;
}
document.ondragover = function (e) {
    e.preventDefault();
    //浏览器默认会阻止ondrop事件,我们必须在ondragover中阻止浏览器的默认行为
}
document.ondrop = function (e) {
    var id = e.dataTransfer.getData("text/html");
    e.target.appendChild(document.getElementById(id));
}
```

## 地理位置接口 (不常用)
```javascript
var x = document.getElementById("demo");
function getLocation() {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(success, error, option)
    } else {
        x.innerHTML = "Geolocation is not supported by this browser."
    }
}
function success(position) { //注意position
    x.innerHTML = position.coords.latitude + position.coords.longitude;
}
function error(error) {
    switch (error.code) {
        case error.PERMISSION_DENIED:
            x.innerHTML = "User denied the request for Geolocation."
            break;
        case error.POSITION_UNAVAILABLE:
            x.innerHTML = "Location information is unavailable."
            break;
        case error.TIMEOUT:
            x.innerHTML = "The request to get user location timed out."
            break;
        case error.UNKNOWN_ERROR:
            x.innerHTML = "An unknown error occurred."
            break;
    }
}
getLocation();
```
> 注意:
    1. navigator.geolocation.getCurrentPosition(showPosition,showError,{});
    或navigator.geolocation.getCurrentPosition(success,error,option);
    navigator.watchPosition(successCallback, errorCallback, options)
    2.option:可以设置获取数据的方式
        - enableHighAccuracy:true/false:是否使用高精度
        - timeout:设置超时时间,单位ms
        - maximumAge:可以设置浏览器重新获取地理信息的时间间隔,单位是ms
    3.
        - position.coords.latitude 纬度
        - position.coords.longitude 经度
        - position.coords.accuracy 精度
        - position.coords.altitude 海拔高度

## sessionStorage,localStorage
- window.sessionStorage
> 
1. 存储容量5mb左右
2. 本质是存储在当前页面的内存中,其它页面和浏览器无法获取数据
3. 生命周期为关闭当前页面,关闭页面,数据会自动清除

- window.localStorage
> 
1. 存储容量20mb左右
2. 不同浏览器不能共享数据,但是在同一个浏览器的不同窗口中可以共享数据
3. 永久生效,它的数据是存储在硬盘上,并不会随着页面或者浏览器的关闭而清除.如果想清除,必须手动清除

- .setItem(key,value):设置数据,以键值对的方式存储
- .getItem(key):通过指定的键获取对应的值内容
- .removeItem(key):删除指定的键及对应的值内容
- .clear():清空所有存储内容

## 应用缓存
```html
<html lang="en" manifest="demo.appcache">
```
```
CACHE MANIFEST
#上面一句代码必须是当前文档的第一句
#后面写注释

#需要缓存的文件清单列表
CACHE:
#下面就是需要缓存的清单列表
../images/l1.jpg
../images/l2.jpg
# *:代表所有文件

#配置每一次都需要重新从服务器获取的文件清单列表
NETWORK:
../images/l3.jpg

#配置如果文件无法获取则使用指定的文件进行替代
FALLBACK:
../images/l4.jpg ../images/banner_1.jpg
# /:代表所有文件
```



