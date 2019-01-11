---
title: MongoDB学习--知识点整理
tags: 
- MongoDB
categories:
- MongoDB
---

## 创建数据库
```
> use hello
switched to db hello
> db
hello
> show dbs
local  0.078GB
test   0.078GB
> db.hello.insert({"name":"教程"})
WriteResult({ "nInserted" : 1 })
> show dbs
local   0.078GB
hello  0.078GB
test    0.078GB
```
---

## 删除数据库
```
> show dbs
local   0.078GB
hello  0.078GB
test    0.078GB
> use hello
switched to db hello
> db.dropDatabase()
{ "dropped" : "hello", "ok" : 1 }
> show dbs
local  0.078GB
test   0.078GB
```
---

## 删除集合
```
> use hello
switched to db hello
> show tables
site
> db.site.drop()
true
> show tables
```
---

## 插入文档
```
>db.col.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '教程',
    url: 'http://www.hello.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
> db.col.find()
{ "_id" : ObjectId("56064886ade2f21f36b03134"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb", "database", "NoSQL" ], "likes" : 100 }
> 
```
也可以将数据定义为一个变量
```
> document=({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '教程',
    url: 'http://www.hello.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
});
{
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "教程",
        "url" : "http://www.hello.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
> db.col.insert(document)
WriteResult({ "nInserted" : 1 })
```
插入文档你也可以使用 db.col.save(document) 命令

 插入单条数据
```
> var document = db.collection.insertOne({"a": 3})
> document
{
        "acknowledged" : true,
        "insertedId" : ObjectId("571a218011a82a1d94c02333")
}
```
插入多条数据
```
> var res = db.collection.insertMany([{"b": 3}, {'c': 4}])
> res
{
        "acknowledged" : true,
        "insertedIds" : [
                ObjectId("571a22a911a82a1d94c02337"),
                ObjectId("571a22a911a82a1d94c02338")
        ]
}
```
---

## 更新文档
```
>db.col.insert({
    title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '教程',
    url: 'http://www.hello.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
>db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })   # 输出信息
> db.col.find().pretty()
{
        "_id" : ObjectId("56064f89ade2f21f36b03136"),
        "title" : "MongoDB",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "教程",
        "url" : "http://www.hello.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
```
如果你要修改多条相同的文档，则需要设置 multi 参数为 true。
```
>db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}},{multi:true})
```
```
use test
db.test_collection.insert( [
{"name":"abc","age":"25","status":"zxc"},
{"name":"dec","age":"19","status":"qwe"},
{"name":"asd","age":"30","status":"nmn"},
] )
```
更新单个文档
```
> db.test_collection.updateOne({"name":"abc"},{$set:{"age":"28"}})
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
> db.test_collection.find()
{ "_id" : ObjectId("59c8ba673b92ae498a5716af"), "name" : "abc", "age" : "28", "status" : "zxc" }
{ "_id" : ObjectId("59c8ba673b92ae498a5716b0"), "name" : "dec", "age" : "19", "status" : "qwe" }
{ "_id" : ObjectId("59c8ba673b92ae498a5716b1"), "name" : "asd", "age" : "30", "status" : "nmn" }
>
```
更新多个文档
```
> db.test_collection.updateMany({"age":{$gt:"10"}},{$set:{"status":"xyz"}})
{ "acknowledged" : true, "matchedCount" : 3, "modifiedCount" : 3 }
> db.test_collection.find()
{ "_id" : ObjectId("59c8ba673b92ae498a5716af"), "name" : "abc", "age" : "28", "status" : "xyz" }
{ "_id" : ObjectId("59c8ba673b92ae498a5716b0"), "name" : "dec", "age" : "19", "status" : "xyz" }
{ "_id" : ObjectId("59c8ba673b92ae498a5716b1"), "name" : "asd", "age" : "30", "status" : "xyz" }
>
```
save替换了 _id 为 56064f89ade2f21f36b03136 的文档数据：
```
>db.col.save({
    "_id" : ObjectId("56064f89ade2f21f36b03136"),
    "title" : "MongoDB",
    "description" : "MongoDB 是一个 Nosql 数据库",
    "by" : "hello",
    "url" : "http://www.hello.com",
    "tags" : [
            "mongodb",
            "NoSQL"
    ],
    "likes" : 110
})
```
替换成功后，我们可以通过 find() 命令来查看替换后的数据
```
>db.col.find().pretty()
{
        "_id" : ObjectId("56064f89ade2f21f36b03136"),
        "title" : "MongoDB",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "hello",
        "url" : "http://www.hello.com",
        "tags" : [
                "mongodb",
                "NoSQL"
        ],
        "likes" : 110
}
> 
```
---

## 删除文档
```
> db.col.find()
{ "_id" : ObjectId("56066169ade2f21f36b03137"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb", "database", "NoSQL" ], "likes" : 100 }
{ "_id" : ObjectId("5606616dade2f21f36b03138"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb", "database", "NoSQL" ], "likes" : 100 }
>db.col.remove({'title':'MongoDB 教程'})
WriteResult({ "nRemoved" : 2 })           # 删除了两条数据
>db.col.find()
……                                        # 没有数据
```
如果你只想删除第一条找到的记录可以设置 justOne 为 1
如果你想删除所有数据，可以使用以下方式：
```
>db.col.remove({})
>db.col.find()
```

删除集合下全部文档：
```
db.inventory.deleteMany({})
```
删除 status 等于 A 的全部文档：
```
db.inventory.deleteMany({ status : "A" })
```
删除 status 等于 D 的一个文档：
```
db.inventory.deleteOne( { status: "D" } )
```
---

## 查询文档
```
> db.col.find().pretty()
{
        "_id" : ObjectId("56063f17ade2f21f36b03133"),
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "教程",
        "url" : "http://www.hello.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
```
pretty() 方法以格式化的方式来显示所有文档
findOne() 
---

## AND
```
> db.col.find({"by":"教程", "title":"MongoDB 教程"}).pretty()
{
        "_id" : ObjectId("56063f17ade2f21f36b03133"),
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "教程",
        "url" : "http://www.hello.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
```
---

## OR
```
>db.col.find({$or:[{"by":"教程"},{"title": "MongoDB 教程"}]}).pretty()
{
        "_id" : ObjectId("56063f17ade2f21f36b03133"),
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "教程",
        "url" : "http://www.hello.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
```
---
```
>db.col.find({"likes": {$gt:50}, $or: [{"by": "教程"},{"title": "MongoDB 教程"}]}).pretty()
{
        "_id" : ObjectId("56063f17ade2f21f36b03133"),
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "教程",
        "url" : "http://www.hello.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
```
---

## 条件操作符
先使用以下命令清空集合 "col" 的数据：
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

#### 大于操作符 - $gt
如果你想获取 "col" 集合中 "likes" 大于 100 的数据，你可以使用以下命令：
```
db.col.find({"likes" : {$gt : 100}})
```
输出结果：
```
> db.col.find({"likes" : {$gt : 100}})
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "java" ], "likes" : 150 }
```

#### 小于操作符 - $lte
如果你想获取"col"集合中 "likes" 小于等于 150 的数据，你可以使用以下命令：
```
db.col.find({likes : {$lte : 150}})
```
输出结果：
```
> db.col.find({likes : {$lte : 150}})
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb" ], "likes" : 100 }
```

#### 使用 (<) 和 (>) 查询 - $lt 和 $gt
如果你想获取"col"集合中 "likes" 大于100，小于 200 的数据，你可以使用以下命令：
```
db.col.find({likes : {$lt :200, $gt : 100}})
```
输出结果：
```
> db.col.find({likes : {$lt :200, $gt : 100}})
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "java" ], "likes" : 150 }
```
---

## $type
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
 如果想获取 "col" 集合中 title 为 String 的数据，你可以使用以下命令：
db.col.find({"title" : {$type : 2}})
```
输出结果为：
```
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb" ], "likes" : 100 }
```
---

## Limit()
集合 col 中的数据如下：
```
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb" ], "likes" : 100 }
```
以上实例为显示查询文档中的两条记录：
```
> db.col.find({},{"title":1,_id:0}).limit(2)
{ "title" : "PHP 教程" }
{ "title" : "Java 教程" }
```
>注：如果你们没有指定limit()方法中的参数则显示集合中的所有数据。
---

## Skip() 
跳过指定数量的数据
以上实例只会显示第二条文档数据
```
>db.col.find({},{"title":1,_id:0}).limit(1).skip(1)
{ "title" : "Java 教程" }
>
```
> 注:skip()方法默认参数为 0 。
---

## 排序sort()
 1 为升序排列，而-1是用于降序排列
col 集合中的数据如下：
```
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb" ], "likes" : 100 }
```
以下实例演示了 col 集合中的数据按字段 likes 的降序排列：
```
>db.col.find({},{"title":1,_id:0}).sort({"likes":-1})
{ "title" : "PHP 教程" }
{ "title" : "Java 教程" }
{ "title" : "MongoDB 教程" }
```
---

## 创建索引ensureIndex() 
Key 值为你要创建的索引字段，1为指定按升序创建索引，如果你想按降序来创建索引指定为-1即可

## 聚合aggregate() 
$project：修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档。
$match：用于过滤数据，只输出符合条件的文档。$match使用MongoDB的标准查询操作。
$limit：用来限制MongoDB聚合管道返回的文档数。
$skip：在聚合管道中跳过指定数量的文档，并返回余下的文档。
$unwind：将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值。
$group：将集合中的文档分组，可用于统计结果。
$sort：将输入文档排序后输出。
$geoNear：输出接近某一地理位置的有序文档。

>参考自:http://www.runoob.com/
