---
title: MongoDB学习--常用操作整理
tags: 
- MongoDB
categories:
- MongoDB
---

## 实例
以下实例我们创建了数据库 hello:
```
> use hello
switched to db hello
> db
hello
> 
```
如果你想查看所有数据库，可以使用 show dbs 命令：
```
> show dbs
local  0.078GB
test   0.078GB
> 
```
可以看到，我们刚创建的数据库 hello 并不在数据库的列表中， 要显示它，我们需要向 hello 数据库插入一些数据。
```
> db.hello.insert({"name":"教程"})
WriteResult({ "nInserted" : 1 })
> show dbs
local   0.078GB
hello  0.078GB
test    0.078GB
> 
```
MongoDB 中默认的数据库为 test，如果你没有创建新的数据库，集合将存放在 test 数据库中。

## 实例
以下实例我们删除了数据库 hello。
首先，查看所有数据库：
```
> show dbs
local   0.078GB
hello  0.078GB
test    0.078GB
```
接下来我们切换到数据库 hello：
```
> use hello
switched to db hello
> 
```
执行删除命令：
```
> db.dropDatabase()
{ "dropped" : "hello", "ok" : 1 }
```
最后，我们再通过 show dbs 命令数据库是否删除成功：
```
> show dbs
local  0.078GB
test   0.078GB
> 
```

## 实例
以下实例删除了 hello 数据库中的集合 site：
```
> use hello
switched to db hello
> show tables
site
> db.site.drop()
true
> show tables
> 
```

## 实例
以下文档可以存储在 MongoDB 的 hello 数据库 的 col 集合中：
```
>db.col.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '教程',
    url: 'http://www.hello.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
```
以上实例中 col 是我们的集合名，如果该集合不在该数据库中， MongoDB 会自动创建该集合并插入文档。
查看已插入文档：
```
> db.col.find()
{ "_id" : ObjectId("56064886ade2f21f36b03134"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb", "database", "NoSQL" ], "likes" : 100 }
> 
```
我们也可以将数据定义为一个变量，如下所示：
```
> document=({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '教程',
    url: 'http://www.hello.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
});
```
执行后显示结果如下：
```
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
```
执行插入操作：
```
> db.col.insert(document)
WriteResult({ "nInserted" : 1 })
> 
```
插入文档你也可以使用 db.col.save(document) 命令。如果不指定 _id 字段 save() 方法类似于 insert() 方法。如果指定 _id 字段，则会更新该 _id 的数据。

3.2 版本后还有以下几种语法可用于插入文档:
`db.collection.insertOne()` : 向指定集合中插入一条文档数据
`db.collection.insertMany()` : 向指定集合中插入多条文档数据

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

## 实例
我们在集合 col 中插入如下数据：
```
>db.col.insert({
    title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '教程',
    url: 'http://www.hello.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
```
接着我们通过 update() 方法来更新标题(title):
```
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
>
```
可以看到标题(title)由原来的 "MongoDB 教程" 更新为了 "MongoDB"。
以上语句只会修改第一条发现的文档，如果你要修改多条相同的文档，则需要设置 multi 参数为 true。
```
>db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}},{multi:true})
```

## 实例
以下实例中我们替换了 _id 为 56064f89ade2f21f36b03136 的文档数据：
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

## 更多实例
只更新第一条记录：
```
db.col.update( { "count" : { $gt : 1 } } , { $set : { "test2" : "OK"} } );
```
全部更新：
```
db.col.update( { "count" : { $gt : 3 } } , { $set : { "test2" : "OK"} },false,true );
```
只添加第一条：
```
db.col.update( { "count" : { $gt : 4 } } , { $set : { "test5" : "OK"} },true,false );
```
全部添加加进去:
```
db.col.update( { "count" : { $gt : 5 } } , { $set : { "test5" : "OK"} },true,true );
```
全部更新：
```
db.col.update( { "count" : { $gt : 15 } } , { $inc : { "count" : 1} },false,true );
```
只更新第一条记录：
```
db.col.update( { "count" : { $gt : 10 } } , { $inc : { "count" : 1} },false,false );
```

在3.2版本开始，MongoDB提供以下更新集合文档的方法：
`db.collection.updateOne()` 向指定集合更新单个文档
`db.collection.updateMany()` 向指定集合更新多个文档
首先我们在test集合里插入测试数据
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

## 实例
以下文档我们执行两次插入操作：
```
>db.col.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '教程',
    url: 'http://www.hello.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
```
使用 find() 函数查询数据：
```
> db.col.find()
{ "_id" : ObjectId("56066169ade2f21f36b03137"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb", "database", "NoSQL" ], "likes" : 100 }
{ "_id" : ObjectId("5606616dade2f21f36b03138"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb", "database", "NoSQL" ], "likes" : 100 }
```
接下来我们移除 title 为 'MongoDB 教程' 的文档：
```
>db.col.remove({'title':'MongoDB 教程'})
WriteResult({ "nRemoved" : 2 })           # 删除了两条数据
>db.col.find()
……                                        # 没有数据
```
如果你只想删除第一条找到的记录可以设置 justOne 为 1，如下所示：
```
>db.COLLECTION_NAME.remove(DELETION_CRITERIA,1)
```
如果你想删除所有数据，可以使用以下方式（类似常规 SQL 的 truncate 命令）：
```
>db.col.remove({})
>db.col.find()
>
```

remove() 方法已经过时了，现在官方推荐使用 deleteOne() 和 deleteMany() 方法。
如删除集合下全部文档：
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

## 实例
以下实例我们查询了集合 col 中的数据：
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
除了 find() 方法之外，还有一个 findOne() 方法，它只返回一个文档。

## 实例
以下实例通过 by 和 title 键来查询 教程 中 MongoDB 教程 的数据
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
以上实例中类似于 WHERE 语句：WHERE by='教程' AND title='MongoDB 教程'

## 实例
以下实例中，我们演示了查询键 by 值为 教程 或键 title 值为 MongoDB 教程 的文档。
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
>
```
以下实例演示了 AND 和 OR 联合使用，类似常规 SQL 语句为： 'where likes>50 AND (by = '教程' OR title = 'MongoDB 教程')'
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

db.col.find({"likes" : {$gt : 100}})
db.col.find({likes : {$gte : 100}})
db.col.find({likes : {$lt : 150}})
db.col.find({likes : {$lte : 150}})
db.col.find({likes : {$lt :200, $gt : 100}})
```
如果想获取 "col" 集合中 title 为 String 的数据，你可以使用以下命令：
```
db.col.find({"title" : {$type : 2}})
```

## 实例
集合 col 中的数据如下：
```
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb" ], "likes" : 100 }
以上实例为显示查询文档中的两条记录：
> db.col.find({},{"title":1,_id:0}).limit(2)
{ "title" : "PHP 教程" }
{ "title" : "Java 教程" }
>
```
注：如果你们没有指定limit()方法中的参数则显示集合中的所有数据。

## 实例
以下实例只会显示第二条文档数据
```
>db.col.find({},{"title":1,_id:0}).limit(1).skip(1)
{ "title" : "Java 教程" }
>
```
注:skip()方法默认参数为 0 。

```
db.col.find({},{"title":1,_id:0}).limit(2)
```
补充说明：
第一个 {} 放 where 条件，为空表示返回集合中所有文档。
第二个 {} 指定那些列显示和不显示 （0表示不显示 1表示显示)。

## 实例
col 集合中的数据如下：
```
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "url" : "http://www.hello.com", "tags" : [ "mongodb" ], "likes" : 100 }
以下实例演示了 col 集合中的数据按字段 likes 的降序排列：
>db.col.find({},{"title":1,_id:0}).sort({"likes":-1})
{ "title" : "PHP 教程" }
{ "title" : "Java 教程" }
{ "title" : "MongoDB 教程" }
>
```
其中 1 为升序排列，而-1是用于降序排列。

skip(), limilt(), sort()三个放在一起执行的时候，执行的顺序是先 sort(), 然后是 skip()，最后是显示的 limit()。

>参考自:http://www.runoob.com/
