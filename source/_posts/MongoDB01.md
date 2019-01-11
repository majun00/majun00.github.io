---
title: MongoDB学习(1)--安装及配置
tags: 
- MongoDB
categories:
- MongoDB
---

## MongoDB
MongoDB 是一个基于分布式文件存储的数据库。由 C++ 语言编写。旨在为 WEB 应用提供可扩展的高性能数据存储解决方案。MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。

# Windows 平台安装 MongoDB
## MongoDB 下载
MongoDB 提供了可用于 32 位和 64 位系统的预编译二进制包，你可以从MongoDB官网下载安装，MongoDB 预编译二进制包下载地址：[https://www.mongodb.com/download-center#community](https://www.mongodb.com/download-center#community)

> 注意：在 MongoDB 2.2 版本后已经不再支持 Windows XP 系统。最新版本也已经没有了 32 位系统的安装文件。

*   **MongoDB for Windows 64-bit** 适合 64 位的 Windows Server 2008 R2, Windows 7 , 及最新版本的 Window 系统。
*   **MongoDB for Windows 32-bit** 适合 32 位的 Window 系统及最新的 Windows Vista。 32 位系统上 MongoDB 的数据库最大为 2GB。
*   **MongoDB for Windows 64-bit Legacy** 适合 64 位的 Windows Vista, Windows Server 2003, 及 Windows Server 2008 。

根据你的系统下载 32 位或 64 位的 .msi 文件，下载后双击该文件，按操作提示安装即可。
安装过程中，你可以通过点击 "Custom(自定义)" 按钮来设置你的安装目录。

**创建数据目录**
MongoDB将数据目录存储在 db 目录下。但是这个数据目录不会主动创建，我们在安装完成后需要创建它。请注意，数据目录应该放在根目录下（(如： C:\ 或者 D:\ 等 )。
在本教程中，我们已经在 C 盘安装了 mongodb，现在让我们创建一个 data 的目录然后在 data 目录里创建 db 目录。
```
c:\>cd c:\

c:\>mkdir data

c:\>cd data

c:\data>mkdir db

c:\data>cd db

c:\data\db>
```
你也可以通过 window 的资源管理器中创建这些目录，而不一定通过命令行。

## 命令行下运行 MongoDB 服务器
从 MongoDB 目录的 bin 目录中执行 mongod.exe 文件。
```
C:\mongodb\bin\mongod --dbpath c:\data\db
```

## 连接MongoDB
在命令窗口中运行 mongo.exe 命令即可连接上 MongoDB
```
C:\mongodb\bin\mongo.exe
```

## 配置 MongoDB 服务
**管理员模式打开命令行窗口**
创建目录，执行下面的语句来创建数据库和日志文件的目录
```
mkdir c:\data\db
mkdir c:\data\log
```

**创建配置文件**
创建一个配置文件。该文件必须设置 systemLog.path 参数，包括一些附加的配置选项更好。例如，创建一个配置文件位于 C:\mongodb\mongod.cfg，其中指定 systemLog.path 和 storage.dbPath。具体配置内容如下：
```
systemLog:
    destination: file
    path: c:\data\log\mongod.log
storage:
    dbPath: c:\data\db
```

### 安装 MongoDB服务
通过执行mongod.exe，使用--install选项来安装服务，使用--config选项来指定之前创建的配置文件。
```
"C:\mongodb\bin\mongod.exe" --config "C:\mongodb\mongod.cfg" --install
```
要使用备用 dbpath，可以在配置文件（例如：C:\mongodb\mongod.cfg）或命令行中通过 --dbpath 选项指定。如果需要，您可以安装 mongod.exe 或 mongos.exe 的多个实例的服务。只需要通过使用 --serviceName 和 --serviceDisplayName 指定不同的实例名。只有当存在足够的系统资源和系统的设计需要这么做。

启动MongoDB服务
```
net start MongoDB
```

关闭MongoDB服务
```
net stop MongoDB
```

移除MongoDB服务
```
"C:\mongodb\bin\mongod.exe" --remove
```

> **命令行下运行 MongoDB 服务器** 和 **配置 MongoDB 服务** 任选一个方式启动就可以。

## MongoDB 后台管理 Shell
如果你需要进入MongoDB后台管理，你需要先打开mongodb装目录的下的bin目录，然后执行mongo.exe文件，MongoDB Shell是MongoDB自带的交互式Javascript shell,用来对MongoDB进行操作和管理的交互式环境。当你进入mongoDB后台后，它默认会链接到 test 文档（数据库）：
```
> mongo
MongoDB shell version: 3.0.6
connecting to: test
……
```
(由于它是一个JavaScript shell，您可以运行一些简单的算术运算)

**db** 命令用于查看当前操作的文档（数据库）：
```
> db
test
>
```

插入一些简单的记录并查找它：
```
> db.runoob.insert({x:10})
WriteResult({ "nInserted" : 1 })
> db.runoob.find()
{ "_id" : ObjectId("5604ff74a274a611b0c990aa"), "x" : 10 }
>
```
第一个命令将数字 10 插入到 runoob 集合的 x 字段中。

# MongoDB - 连接
### 启动 MongoDB 服务
只需要在 MongoDB 安装目录的 bin 目录下执行 **mongod** 即可。执行启动操作后，mongodb 在输出一些必要信息后不会输出任何信息，之后就等待连接的建立，当连接被建立后，就会开始打印日志信息。你可以使用 MongoDB shell 来连接 MongoDB 服务器。你也可以使用 PHP 来连接 MongoDB。本教程我们会使用 MongoDB shell 来连接 Mongodb 服务。

标准 URI 连接语法：
`mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]`
*   **mongodb://** 这是固定的格式，必须要指定。
*   **username:password@** 可选项，如果设置，在连接数据库服务器之后，驱动都会尝试登陆这个数据库
*   **host1** 必须的指定至少一个host, host1 是这个URI唯一要填写的。它指定了要连接服务器的地址。如果要连接复制集，请指定多个主机地址。
*   **portX** 可选的指定端口，如果不填，默认为27017
*   **/database **如果指定username:password@，连接并验证登陆指定数据库。若不指定，默认打开 test 数据库。
*   **?options** 是连接选项。如果不使用/database，则前面需要加上/。所有连接选项都是键值对name=value，键值对之间通过&或;（分号）隔开

标准的连接格式包含了多个选项(options)，如下所示：
`replicaSet=name` 验证replica set的名称。 Impliesconnect=replicaSet.
`slaveOk=true|false` 
true:在connect=direct模式下，驱动会连接第一台机器，即使这台服务器不是主。在connect=replicaSet模式下，驱动会发送所有的写请求到主并且把读取操作分布在其他从服务器。
false: 在 connect=direct模式下，驱动会自动找寻主服务器. 在connect=replicaSet 模式下，驱动仅仅连接主服务器，并且所有的读写命令都连接到主服务器。
`safe=true|false` 
true: 在执行更新操作之后，驱动都会发送getLastError命令来确保更新成功。(还要参考 wtimeoutMS).
false: 在每次更新之后，驱动不会发送getLastError来确保更新成功。
`w=n` 驱动添加 { w : n } 到getLastError命令. 应用于safe=true。
`wtimeoutMS=ms` 驱动添加 { wtimeout : ms } 到 getlasterror 命令. 应用于 safe=true.
`fsync=true|false` 
true: 驱动添加 { fsync : true } 到 getlasterror 命令.应用于 safe=true.
false: 驱动不会添加到getLastError命令中。
`journal=true|false` 如果设置为 true, 同步到 journal (在提交到数据库前写入到实体中). 应用于 safe=true
`connectTimeoutMS=ms` 可以打开连接的时间。
`socketTimeoutMS=ms` 发送和接受sockets的时间。

### 实例
使用默认端口来连接 MongoDB 的服务。
```
mongodb://localhost
```

通过 shell 连接 MongoDB 服务：
```
$ ./mongo
MongoDB shell version: 3.0.6
connecting to: test
... 
```

这时候你返回查看运行 **./mongod** 命令的窗口，可以看到是从哪里连接到MongoDB的服务器，您可以看到如下信息：
```
……省略信息……
2015-09-25T17:22:27.336+0800 I CONTROL  [initandlisten] allocator: tcmalloc
2015-09-25T17:22:27.336+0800 I CONTROL  [initandlisten] options: { storage: { dbPath: "/data/db" } }
2015-09-25T17:22:27.350+0800 I NETWORK  [initandlisten] waiting for connections on port 27017
2015-09-25T17:22:36.012+0800 I NETWORK  [initandlisten] connection accepted from 127.0.0.1:37310 #1 (1 connection now open)  # 该行表明一个来自本机的连接

……省略信息……
```

## MongoDB 连接命令格式
使用用户名和密码连接到 MongoDB 服务器，你必须使用 '**username:password@hostname/dbname**' 格式，'username'为用户名，'password' 为密码。
使用用户名和密码连接登陆到默认数据库：
```
$ ./mongo
MongoDB shell version: 3.0.6
connecting to: test
```

使用用户 admin 使用密码 123456 连接到本地的 MongoDB 服务上。输出结果如下所示：
```
> mongodb://admin:123456@localhost/
... 
```

使用用户名和密码连接登陆到指定数据库，格式如下：
```
mongodb://admin:123456@localhost/test
```

### 更多连接实例
连接本地数据库服务器，端口是默认的。
```
mongodb://localhost
```

使用用户名fred，密码foobar登录localhost的admin数据库。
```
mongodb://fred:foobar@localhost
```

使用用户名fred，密码foobar登录localhost的baz数据库。
```
mongodb://fred:foobar@localhost/baz
```

连接 replica pair, 服务器1为example1.com服务器2为example2。
```
mongodb://example1.com:27017,example2.com:27017
```

连接 replica set 三台服务器 (端口 27017, 27018, 和27019):
```
mongodb://localhost,localhost:27018,localhost:27019
```

连接 replica set 三台服务器, 写入操作应用在主服务器 并且分布查询到从服务器。
```
mongodb://host1,host2,host3/?slaveOk=true
```

直接连接第一个服务器，无论是replica set一部分或者主服务器或者从服务器。
```
mongodb://host1,host2,host3/?connect=direct;slaveOk=true
```
当你的连接服务器有优先级，还需要列出所有服务器，你可以使用上述连接方式。

安全模式连接到localhost:
```
mongodb://localhost/?safe=true
```

以安全模式连接到replica set，并且等待至少两个复制服务器成功写入，超时时间设置为2秒。
```
mongodb://host1,host2,host3/?safe=true;w=2;wtimeoutMS=2000
```

>参考自:http://www.runoob.com/
