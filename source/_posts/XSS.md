---
title: 了解web安全-XSS
tags: 
- XSS
- 前端安全性问题
categories:
- 前端安全性问题
---

## XSS攻击：
- 盗用cookie获取页面信息
- 插入内容破坏页面结构
- 插入flash
- 实现DDOS攻击效果(DOS攻击) 分布式拒绝服务攻击 客户端请求占用过多资源 让合法用户得不到服务端响应
- 实现ServerLimitDOS攻击效果 httpRequest过长产生一个4开头的错误 保存在cookie中 某些用户将无法正常访问

## 攻击方式 : 
#### 反射型 
发出请求时，XSS出现在URL中，提交到服务器，服务器解析响应，XSS随响应内容一起传回给浏览器，浏览器解析执行XSS。
```
//index.js
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
	res.set('X-XSS-Protection',0);
  res.render('index', { title: 'Express',xss:req.query.xss });
});

module.exports = router;
```
```
//index.jade
extends layout

block content
  h1= title
  p Welcome to #{title}
  div !{xss}
```
在地址栏输入 localhost:3000/?xss=<iframe scr="[广告链接]"></iframe>

#### 存储型
提交的代码存储在服务器(数据库，内存，文件系统)，下次请求目标页面不用再提交XSS

## XSS防御 : 
#### 编码
对用户输入数据进行HTML Entity编码

#### 过滤
- 移除用户上传DOM属性
- 移除用户上传style节点script节点iframe节点

#### 校正 
- 避免直接对HTML Entity编码
- 使用DOM Parse转换 , 校正不配对的DOM



