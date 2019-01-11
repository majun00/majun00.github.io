---
title: 比较 mock、json-server、graphQL
tags: 
- Mockjs
- json-server
- GraphQL
categories:
- Mock
---

## mock.js

1. 安装

```
# cnpm i mockjs
```

2. 基本使用

```
const Mock=require('mockjs)
Mock.mock('http://a.com/num', 'post', {
    "number|1-100": 100
})

Mock.mock('http://a.com/type',function(options) {
    return options.type
}))
```

现在就可以拦截请求返回 mock 数据了，post 请求'http://a.com/num'返回1-100随机数，请求'http://a.com/type'返回当前请求类型

3. 缺点：

- 不及 json-server 使用方便
- 不能跨域使用
- 无法定义复杂结构

> 具体请看[官方文档](http://mockjs.com/)

## json-server

1. 安装

```
# cnpm i json-server -g
```

2. 启动
   在项目文件根目录新建一个 json 文件，例如

```
{
    "data": [{
            "name": "张三",
            "age": 12,
            "id": 0
        },
        {
            "name": "李是",
            "age": 22,
            "id": 1
        },
        {
            "name": "王五",
            "age": 14,
            "id": 2
        }
    ]
}
```

```
# json-server db.json -p 3001
```

3. 访问 localhost:3001 就可访问并操作数据了

```
GET /data 获取所有数据
GET /data/1 获取id为1的数据
DELETE /data/1 删除id为1的数据
POST /data 新增数据，请求body中必须包含data的属性数据
PUT /data/1 修改数据，请求body中必须包含data的属性数据
PATCH /data/1 修改数据，请求body中必须包含data的属性数据
GET /data?age_gte=10 获取age大于等于10的数据
GET /data?_sort=age&_order=asc 根据age升序排序
```

更多操作请看[官方文档](https://www.npmjs.com/package/json-server)

## graphQL

(这里我们结合 express 来运行 graphQL)

```
cnpm install express express-graphql graphql
```

```
var express = require('express');
var graphqlHTTP = require('express-graphql');
var { buildSchema } = require('graphql');
var app = express();

let listData = [{
        "name": "张三",
        "age": 12,
        "id": 0
    },
    {
        "name": "李是",
        "age": 22,
        "id": 1
    }, {
        "name": "王五",
        "age": 14,
        "id": 2
    }
]

var schema = buildSchema(`
  type Item {
    name: String,
    age: Int,
    id: Int,
  }

  type Query{
      list: [Item]
  }
`);

var root = { list: listData };

app.use('/graphql', graphqlHTTP({
    schema: schema,
    rootValue: root,
    graphiql: true,
}));

app.listen(4000, () => console.log('Now browse to localhost:4000/graphql'));
```

现在我们在打开的 graphQL 中输入

```
query{
   list{
		id,
    name,
    age
  }
}
```

就会返回数据了, 这里的数据是自己写的假数据，和 json-server 搭配食用最佳哟

更多操作请看[官方文档](http://graphql.cn)

> 听说 APIJSON 也不错[官方文档](https://github.com/TommyLemon/APIJSON)
