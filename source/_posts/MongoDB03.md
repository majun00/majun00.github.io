---
title: MongoDB学习(3)--常用操作符
tags: 
- MongoDB
categories:
- MongoDB
---

# MongoDB 条件操作符
条件操作符用于比较两个表达式并从mongoDB集合中获取数据。

MongoDB中条件操作符有：
*   (>) 大于 - $gt
*   (<) 小于 - $lt
*   (>=) 大于等于 - $gte
*   (<= ) 小于等于 - $lte

**我们使用的数据库名称为"hello" 我们的集合名称为"col"，以下为我们插入的数据。**
为了方便测试，我们可以先使用以下命令清空集合 "col" 的数据：
```
db.col.remove({})
```
插入以下数据
```
>db.col.insert({
    title: 'PHP 教程', 
    description: 'PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。',
    by: '教程',
    url: 'http://www.hello.com',
    tags: ['php'],
    likes: 200
})
```
```
>db.col.insert({title: 'Java 教程', 
    description: 'Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。',
    by: '教程',
    url: 'http://www.hello.com',
    tags: ['java'],
    likes: 150
})
```
```
>db.col.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '教程',
    url: 'http://www.hello.com',
    tags: ['mongodb'],
    likes: 100
})
```
使用find()命令查看数据：
```
> db.col.find()
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb" ], "likes" : 100 }
```

## MongoDB (>) 大于操作符 - $gt
如果你想获取 "col" 集合中 "likes" 大于 100 的数据，你可以使用以下命令：
```
db.col.find({"likes" : {$gt : 100}})
```
类似于SQL语句：
```
Select * from col where likes > 100;
```
输出结果：
```
> db.col.find({"likes" : {$gt : 100}})
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "java" ], "likes" : 150 }
> 
```

## MongoDB（>=）大于等于操作符 - $gte
如果你想获取"col"集合中 "likes" 大于等于 100 的数据，你可以使用以下命令：
```
db.col.find({likes : {$gte : 100}})
```
类似于SQL语句：
```
Select * from col where likes >=100;
```
输出结果：
```
> db.col.find({likes : {$gte : 100}})
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb" ], "likes" : 100 }
> 
```

## MongoDB (<) 小于操作符 - $lt
如果你想获取"col"集合中 "likes" 小于 150 的数据，你可以使用以下命令：
```
db.col.find({likes : {$lt : 150}})
```
类似于SQL语句：
```
Select * from col where likes < 150;
```
输出结果：
```
> db.col.find({likes : {$lt : 150}})
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb" ], "likes" : 100 }
```

## MongoDB (<=) 小于操作符 - $lte
如果你想获取"col"集合中 "likes" 小于等于 150 的数据，你可以使用以下命令：
```
db.col.find({likes : {$lte : 150}})
```
类似于SQL语句：
```
Select * from col where likes <= 150;
```
输出结果：
```
> db.col.find({likes : {$lte : 150}})
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb" ], "likes" : 100 }
```

## MongoDB 使用 (<) 和 (>) 查询 - $lt 和 $gt
如果你想获取"col"集合中 "likes" 大于100，小于 200 的数据，你可以使用以下命令：
```
db.col.find({likes : {$lt :200, $gt : 100}})
```
类似于SQL语句：
```
Select * from col where likes>100 AND  likes<200;
```
输出结果：
```
> db.col.find({likes : {$lt :200, $gt : 100}})
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "java" ], "likes" : 150 }
> 
```

```
$gt -------- greater than  >

$gte --------- gt equal  >=

$lt -------- less than  <

$lte --------- lt equal  <=

$ne ----------- not equal  !=

$eq  --------  equal  =
```

# MongoDB $type 操作符
$type操作符是基于BSON类型来检索集合中匹配的数据类型，并返回结果。

MongoDB 中可以使用的类型如下表所示：
Double	1	 
String	2	 
Object	3	 
Array	4	 
Binary data	5	 
Undefined	6	已废弃。
Object id	7	 
Boolean	8	 
Date	9	 
Null	10	 
Regular Expression	11	 
JavaScript	13	 
Symbol	14	 
JavaScript (with scope)	15	 
32-bit integer	16	 
Timestamp	17	 
64-bit integer	18	 
Min key	255	Query with -1.
Max key	127	 

**我们使用的数据库名称为"hello" 我们的集合名称为"col"，以下为我们插入的数据。**
简单的集合"col"：
```
>db.col.insert({
    title: 'PHP 教程', 
    description: 'PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。',
    by: '教程',
    url: 'http://www.hello.com',
    tags: ['php'],
    likes: 200
})
>db.col.insert({title: 'Java 教程', 
    description: 'Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。',
    by: '教程',
    url: 'http://www.hello.com',
    tags: ['java'],
    likes: 150
})
>db.col.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '教程',
    url: 'http://www.hello.com',
    tags: ['mongodb'],
    likes: 100
})
```
使用find()命令查看数据：
```
> db.col.find()
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb" ], "likes" : 100 }
```

## MongoDB 操作符 - $type 实例
如果想获取 "col" 集合中 title 为 String 的数据，你可以使用以下命令：
```
db.col.find({"title" : {$type : 2}})
```
输出结果为：
```
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb" ], "likes" : 100 }
```

>参考自:http://www.runoob.com/
