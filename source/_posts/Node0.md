---
title: Node知识点归纳整理
tags: 
- NodeJS
categories:
- NodeJS
---

> 小伙伴们最好同时学习一些http、服务器、linux的知识哟

### 补充
1. 环境变量 
`cmd:sysdm.cpl`
检测PATH环境变量是否配置了Node.js，点击开始=》运行=》输入"cmd" => 输入命令"path"

2. 常用指令
`ls` 查看
`md 目录名` 创建目录
`rd(rmdir) 目录名` 删除目录
`rd /s/q 目录名` 安静删除目录及子目录
`echo on > 文件名` 创建文件
`echo 内容 > 文件名` 设置文件内容
`echo 内容 >> 文件名` 添加文件内容
`rm 文件名` 删除文件
`cat 文件名` 查看文件内容
`cat > 文件名` 进入文件内容编辑
`cat >> 文件名` 进入文件内容添加编辑

3. node多版本管理 nvm
`nvm version`
`nvm list` 查看nvm里node版本的列表
`nvm user 版本号` 切换nvm里node的版本

4. REPL 命令行方式
read-eval-print-loop 读取代码-执行-打印结果-循环这个过程
在REPL环境中，`_`表示最后一次执行结果; `.exit`可以退出REPL环境
`node` 进入REPL环境
`node js文件` 通过js文件执行

5. nodejs跨域 
express配置
```
app.all('/test', function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header('Access-Control-Allow-Methods', 'PUT, GET, POST, DELETE, OPTIONS');
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    res.header('Access-Control-Allow-Headers', 'Content-Type');
    next();
});
```
 nginx配置
然而添加了这些之后，仍然不好使。查了查，可能是要在nginx上也作设置，在nginx相应路径添加如下：
```
location ^~ /test {
    add_header 'Access-Control-Allow-Origin' '*';
    add_header 'Access-Control-Allow-Credentials' 'true';
    add_header 'Access-Control-Allow-Methods' 'OPTION, POST, GET';
    add_header 'Access-Control-Allow-Headers' 'X-Requested-With, Content-Type';
}
```
之后重新加载nginx配置即可，大功告成。


### 全局对象global
```
// __filename包含文件名称的全路径
console.log(__filename);
// __dirname文件的路径（不包含文件名称）
console.log(__dirname);
// 定时函数，用法与浏览器中的定时函数类似
var timer = setTimeout(function(){
    console.log(123);
},1000);
// 在Node.js中没有window对象，但是有一个类似的对象global，访问全局成员的时候可以省略global
global.console.log(123456);
// process.argv,argv是一个数组，默认情况下，前两项数据分别是：Node.js环境的路径
// 当前执行的js文件的全路径,从第三个参数开始表示命令行参数
console.log(process.argv);
// process.arch打印当前系统的架构（64位或者32位）
console.log(process.arch);
```

### buffer
> Buffer对象是Node处理二进制数据的一个接口。它是Node原生提供的全局对象，可以直接使用，不需要require(‘buffer’)

Buffer的基本操作 : (Buffer本质上就是字节数组)
    1、构造方法（类）(不常用)
    2、静态方法
    3、实例方法

- 实例化
    + Buffer.from(array)
    + Buffer.from(string)
    + Buffer.alloc(size)
- 功能方法
    + Buffer.isEncoding() 判断是否支持该编码
    + Buffer.isBuffer() 判断是否为Buffer
    + Buffer.byteLength() 返回指定编码的字节长度，默认utf8
    + Buffer.concat() 将一组Buffer对象合并为一个Buffer对象
- 实例方法
    + write() 向buffer对象中写入内容
    + slice() 截取新的buffer对象
    + toString() 把buf对象转成字符串
    + toJson() 把buf对象转成json形式的字符串 (toJSON方法不需要显式调用，当JSON.stringify方法调用的时候会自动调用toJSON方法)

## 核心模块API
### 路径操作
- 路径基本操作API
```javascript
// path.basename()获取路径的最后一部分
// console.log(path.basename('/foo/bar/baz/asdf/quux.html'));
// console.log(path.basename('/foo/bar/baz/asdf/quux.html', '.html'));

// path.dirnam()获取路径
// console.log(__dirname);
// console.log(path.dirname('/abc/qqq/www/abc'));

// path.extname()获取扩展名称
// console.log(path.extname('index.html'));

// path.format(),mpath.parse()路径的格式化处理
// path.format() obj->string
// path.parse()  string->obj
// let obj = path.parse(__filename);
// console.log(obj.base);
/*
{ root: 'E:\\', 文件的跟路径
  dir: 'E:\\node\\day02\\02-code',文件的全路径
  base: '02.js',文件的名称
  ext: '.js',扩展名
  name: '02' 文件名称
}
*/
// let objpath = {
//     root : 'd:\\',
//     dir : 'd:\\qqq\\www',
//     base : 'abc.txt',
//     ext : '.txt',
//     name : 'abc'
// };
// let strpath = path.format(objpath);
// console.log(strpath);

// path.isAbsolute()判断是否为绝对路径
// console.log(path.isAbsolute('/foo/bar'));
// console.log(path.isAbsolute('C:/foo/..'));

// path.join()拼接路径（..表示上层路径；.表示当前路径）,在连接路径的时候会格式化路径
// console.log(path.join('/foo', 'bar', 'baz/asdf', 'quux', '../../'));

// path.normalize()规范化路径
// console.log(path.normalize('/foo/bar//baz/asdf/quux/..'));
// console.log(path.normalize('C:\\temp\\\\foo\\bar\\..\\'));

// path.relative()计算相对路径
// console.log(path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb'));
// console.log(path.relative('C:\\orandea\\test\\aaa', 'C:\\orandea\\impl\\bbb'));

// path.resolve解析路径()
// console.log(path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif'));

// 两个特殊属性path.delimiter,path.sep
// console.log(path.delimiter);//表示路径分隔符（windows是\ Linux是/）
// console.log(path.sep);//环境变量分隔符(windows中使用; linux中使用:)
```

###异步I/O input/output
1、文件操作
2、网络操作

在浏览器中也存在异步操作：
1、定时任务
2、事件处理
3、Ajax回调处理

js的运行是单线程的引入事件队列机制,Node.js中的事件模型与浏览器中的事件模型类似--单线程+事件队列

Node.js中异步执行的任务：
1、文件I/O
2、网络I/O

基于回调函数的编码风格

### 文件操作
- 文件信息获取
- 读文件操作
- 写文件操作
- 目录操作

```javascript
/*
    文件操作-查看文件状态
*/
const fs = require('fs');
fs.stat('./abc',(err,stat) => {
    // 一般回调函数的第一个参数是错误对象，如果err为null,表示没有错误，否则表示报错了
    // if(err) return;
    // if(stat.isFile()){
    //     console.log('文件');
    // }else if(stat.isDirectory()){
    //     console.log('目录');
    // }
    // console.log(stat);
    /*
    atime 文件访问时间
    ctime 文件的状态信息发生变化的时间（比如文件的权限）
    mtime 文件数据发生变化的时间
    birthtime 文件创建的时间
    */
});
// 同步操作
// let ret = fs.statSync('./data.txt');
// console.log(ret);
```

```javascript
/*
    读文件操作
*/
const fs = require('fs');
const path = require('path');
let strpath = path.join(__dirname,'data.txt');

fs.readFile(strpath,(err,data)=>{
    // if(err) return;
    console.log(data.toString());
});

// 如果有第二个参数并且是编码，那么回调函数获取到的数据就是字符串
// 如果没有第二个参数，那么得到的就是Buffer实例对象
// fs.readFile(strpath,'utf8',(err,data)=>{
//     // if(err) return;
//     console.log(data);
// });

// 同步操作
// let ret = fs.readFileSync(strpath,'utf8');
// console.log(ret);
```

```javascript
/*
    写文件操作
*/
const fs = require('fs');
const path = require('path');
let strpath = path.join(__dirname,'data.txt');

fs.writeFile(strpath,'hello nihao','utf8',(err)=>{
    // if(!err){
    //     console.log('文件写入成功');
    // }
});

// let buf = Buffer.from('hi');
// fs.writeFile(strpath,buf,'utf8',(err)=>{
//     if(!err){
//         console.log('文件写入成功');
//     }
// });

// 同步操作
// fs.writeFileSync(strpath,'tom and jerry');
```

```javascript
/*
    大文件操作（流式操作）
    fs.createReadStream(path[, options])
    fs.createWriteStream(path[, options])
*/

const path = require('path');
const fs = require('fs');

let spath = path.join(__dirname,'../03-source','file.zip');
let dpath = path.join('C:\\Users\\www\\Desktop','file.zip');

let readStream = fs.createReadStream(spath);
let writeStream = fs.createWriteStream(dpath);

// 基于事件的处理方式
// let num = 1;
readStream.on('data',(chunk)=>{
    // num++;
    writeStream.write(chunk);
});
readStream.on('end',()=>{
    console.log('文件处理完成'+num);
});

// pipe的作用直接把输入流和输出流
readStream.pipe(writeStream);

fs.createReadStream(spath).pipe(fs.createWriteStream(dpath));
```

```javascript
/*
    目录操作
    1、创建目录
    fs.mkdir(path[, mode], callback)
    fs.mkdirSync(path[, mode])
    2、读取目录
    fs.readdir(path[, options], callback)
    fs.readdirSync(path[, options])
    3、删除目录
    fs.rmdir(path, callback)
    fs.rmdirSync(path)
*/
const path = require('path');
const fs = require('fs');
// 创建目录
fs.mkdir(path.join(__dirname,'abc'),(err)=>{
    console.log(err);
});
// fs.mkdirSync(path.join(__dirname,'hello'));

// --------------------------------
// 读取目录
fs.readdir(__dirname,(err,files)=>{
    files.forEach((item,index)=>{
        fs.stat(path.join(__dirname,item),(err,stat)=>{
            // if(stat.isFile()){
            //     console.log(item,'文件');
            // }else if(stat.isDirectory()){
            //     console.log(item,'目录');
            // }
        });
    });
});
// let files = fs.readdirSync(__dirname);
// files.forEach((item,index)=>{
//     fs.stat(path.join(__dirname,item),(err,stat)=>{
//         if(stat.isFile()){
//             console.log(item,'文件');
//         }else if(stat.isDirectory()){
//             console.log(item,'目录');
//         }
//     });
// });

// ------------------------------
// 删除目录
fs.rmdir(path.join(__dirname,'abc'),(err)=>{
    console.log(err);
});
// fs.rmdirSync(path.join(__dirname,'qqq'));
```

## Node.js实现静态网站功能
> 使用http模块初步实现服务器功能
> 实现静态服务器功能

```javascript
/*
    初步实现服务器功能
*/
const http = require('http');
// 创建服务器实例对象
let server = http.createServer();
// 绑定请求事件
server.on('request',(req,res)=>{
    res.end('hello');
});
// 监听端口
server.listen(3000);

// -----------------------------
http.createServer((req,res)=>{
    res.end('ok');
}).listen(3000,'192.168.0.106',()=>{
    console.log('running...');
});
```

```javascript
/*
    处理请求路径的分发
    1、req对象是Class: http.IncomingMessage的实例对象
    2、res对象是Class: http.ServerResponse的实例对象
*/
const http = require('http');
http.createServer((req,res)=>{
    // req.url可以获取URL中的路径（端口之后部分）
    // res.end(req.url);
    if(req.url.startsWith('/index')){
        // write向客户端响应内容,可以写多次
        res.write('hello');
        res.write('hi');
        // end方法用来完成响应，只能执行一次
        res.end();
    }else if(req.url.startsWith('/about')){
        res.end('about');
    }else{
        res.end('no content');
    }
}).listen(3000,'192.168.0.106',()=>{
    console.log('running...');
});
```

```javascript
/*
    响应完整的页面信息
*/
const http = require('http');
const path = require('path');
const fs = require('fs');

// 根据路径读取文件的内容，并且响应到浏览器端
let readFile = (url,res) => {
    fs.readFile(path.join(__dirname,'www',url),'utf8',(err,fileContent)=>{
        if(err){
            res.end('server error');
        }else{
            res.end(fileContent);
        }
    });
}

http.createServer((req,res)=>{
    // 处理路径的分发
    if(req.url.startsWith('/index')){
        readFile('index.html',res);
    }else if(req.url.startsWith('/about')){
        readFile('about.html',res);
    }else if(req.url.startsWith('/list')){
        readFile('list.html',res);
    }else{
        // 设置相应类型和编码
        res.writeHead(200,{
            'Content-Type':'text/plain; charset=utf8'
        });
        res.end('页面被狗狗叼走了');
    }
}).listen(3000,'192.168.0.106',()=>{
    console.log('running...');
});
```

```javascript
/*
    响应完整的页面信息优化
*/
const http = require('http');
const path = require('path');
const fs = require('fs');
const mime = require('./mime.json');

http.createServer((req,res)=>{
    fs.readFile(path.join(__dirname,'www',req.url),(err,fileContent)=>{
        if(err){
            // 没有找到对应文件
            res.writeHead(404,{
                'Content-Type':'text/plain; charset=utf8'
            });
            res.end('页面被狗狗叼走了');
        }else{
            let dtype = 'text/html';
            // 获取请求文件的后缀
            let ext = path.extname(req.url);
            // 如果请求的文件后缀合理，就获取到标准的响应格式
            if(mime[ext]){
                dtype = mime[ext];
            }
            // 如果响应的内容是文本，就设置成utf8编码
            if(dtype.startsWith('text')){
                dtype += '; charset=utf8'
            }
            // 设置响应头信息
            res.writeHead(200,{
                'Content-Type':dtype
            });
            res.end(fileContent);
        }
    });
}).listen(3000,()=>{
    console.log('running...');
});
```

```javascript
//封装成模块
const path = require('path');
const fs = require('fs');
const mime = require('./mime.json');

exports.staticServer = (req,res,root) => {
    fs.readFile(path.join(root,req.url),(err,fileContent)=>{
        if(err){
            // 没有找到对应文件
            res.writeHead(404,{
                'Content-Type':'text/plain; charset=utf8'
            });
            res.end('页面被狗狗叼走了');
        }else{
            let dtype = 'text/html';
            // 获取请求文件的后缀
            let ext = path.extname(req.url);
            // 如果请求的文件后缀合理，就获取到标准的响应格式
            if(mime[ext]){
                dtype = mime[ext];
            }
            // 如果响应的内容是文本，就设置成utf8编码
            if(dtype.startsWith('text')){
                dtype += '; charset=utf8'
            }
            // 设置响应头信息
            res.writeHead(200,{
                'Content-Type':dtype
            });
            res.end(fileContent);
        }
    });
}
//调用
const http = require('http');
const ss = require('./06.js');
const path = require('path');

http.createServer((req,res)=>{
    // ss.staticServer(req,res,path.join(__dirname,'www'));
    ss.staticServer(req,res,path.join('C:\\Users\\www\\Desktop','test'));
}).listen(3000,()=>{
    console.log('running....');
})
```

## 参数传递与获取
- get参数获取
```javascript
/*
    get参数处理-url核心模块
*/
// const url = require('url');
// parse方法的作用就是把URL字符串转化为对象
// let str = 'http://www.baidu.com/abc/qqq?flag=123&keyword=java';
let ret = url.parse(str,true);
// console.log(ret.query.keyword);

// format的作用就是把对象转化为标准的URL字符串
// let obj = {
//   protocol: 'http:',
//   slashes: true,
//   auth: null,
//   host: 'www.baidu.com',
//   port: null,
//   hostname: 'www.baidu.com',
//   hash: null,
//   search: '?flag=123&keyword=java',
//   query: 'flag=123&keyword=java',
//   pathname: '/abc/qqq',
//   path: '/abc/qqq?flag=123&keyword=java',
//   href: 'http://www.baidu.com/abc/qqq?flag=123&keyword=java' 
// };
let ret1 = url.format(obj);
// console.log(ret1);
```

```javascript
/*
    get参数解析
*/
const http = require('http');
const path = require('path');
const url = require('url');

http.createServer((req,res)=>{
    let obj = url.parse(req.url,true);
    res.end(obj.query.username + '=========' + obj.query.password);
}).listen(3000,()=>{
    console.log('running....');
})
```

- post参数获取
```javascript
/*
    post参数处理
*/
const querystring = require('querystring');
const http = require('http');

// parse方法的作用就是把字符串转成对象
// let param = 'username=lisi&password=123';
// let param = 'foo=bar&abc=xyz&abc=123';
let obj = querystring.parse(param);
// console.log(obj);

// stringify的作用就是把对象转成字符串
// let obj1 = {
//     flag : '123',
//     abc : ['hello','hi']
// }
let str1 = querystring.stringify(obj1);
// console.log(str1);

http.createServer((req,res)=>{
    if(req.url.startsWith('/login')){
        let pdata = '';
        req.on('data',(chunk)=>{
            // 每次获取一部分数据
            pdata += chunk;
        });
        req.on('end',()=>{
            // 这里才能得到完整的数据
            console.log(pdata);
            let obj = querystring.parse(pdata);
            res.end(obj.username+'-----'+obj.password);
        });
    }
}).listen(3000,()=>{
    console.log('running...');
})
```

```javascript
/*
    登录验证功能
*/
const http = require('http');
const url = require('url');
const querystring = require('querystring');
const ss = require('./06.js');

http.createServer((req,res)=>{
    // 启动静态资源服务
    if(req.url.startsWith('/www')){
        ss.staticServer(req,res,__dirname);
    }
    console.log(req.url);
    // 动态资源
    if(req.url.startsWith('/login')){
        // get请求
        if(req.method == 'GET'){
            let param = url.parse(req.url,true).query;
            if(param.username == 'admin' && param.password == '123'){
                res.end('get success');
            }else{
                res.end('get failure');
            }
        }
        // post请求
        if(req.method == 'POST'){
            let pdata = '';
            req.on('data',(chunk)=>{
                pdata += chunk;
            });
            req.on('end',()=>{
                let obj = querystring.parse(pdata);
                if(obj.username == 'admin' && obj.password == '123'){
                    res.end('post success');
                }else{
                    res.end('post failure');
                }
            });
        }
    }

}).listen(3000,()=>{
    console.log('running....');
});
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <form action="http://localhost:3000/login" method="post">
        用户名：<input type="text" name="username"><br>
        密码：<input type="password" name="password"><br>
        <input type="submit" value="提交">
    </form>
</body>
</html>
```

## 动态网站开发
> 创建服务器实现动态网站效果

```javascript
/*
    动态网站开发
    成绩查询功能
*/
const http = require('http');
const path = require('path');
const fs = require('fs');
const querystring = require('querystring');
const scoreData = require('./scores.json');

http.createServer((req,res)=>{
    // 路由（请求路径+请求方式）
    if(req.url.startsWith('/query') && req.method == 'GET'){
        // 查询成绩的入口地址 /query
        fs.readFile(path.join(__dirname,'view','index.tpl'),'utf8',(err,content)=>{
            if(err){
                res.writeHead(500,{
                    'Content-Type':'text/plain; charset=utf8'
                });
                res.end('服务器错误，请与管理员联系');
            }
            res.end(content);
        });
    }else if(req.url.startsWith('/score') && req.method == 'POST'){
        // 获取成绩的结果 /score
        let pdata = '';
        req.on('data',(chunk)=>{
            pdata += chunk;
        });
        req.on('end',()=>{
            let obj = querystring.parse(pdata);
            let result = scoreData[obj.code];
            fs.readFile(path.join(__dirname,'view','result.tpl'),'utf8',(err,content)=>{
                if(err){
                    res.writeHead(500,{
                        'Content-Type':'text/plain; charset=utf8'
                    });
                    res.end('服务器错误，请与管理员联系');
                }
                // 返回内容之前要进行数据渲染
                content = content.replace('$$chinese$$',result.chinese);
                content = content.replace('$$math$$',result.math);
                content = content.replace('$$english$$',result.english);
                content = content.replace('$$summary$$',result.summary);
                res.end(content);
            });
        });
    }
}).listen(3000,()=>{
    console.log('running....');
});
```
```html
<!-- view/index -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>查询成绩</title>
</head>
<body>
    <form action="http://localhost:3000/score" method="post">
        请输入考号：<input type="text" name="code">
        <input type="submit" value="查询">
    </form>
</body>
</html>
```
```html
<!-- view/result -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>成绩结果</title>
</head>
<body>
    <div>
        <ul>
            <li>语文：$$chinese$$</li>
            <li>数学：$$math$$</li>
            <li>外语：$$english$$</li>
            <li>综合：$$summary$$</li>
        </ul>
    </div>
</body>
</html>
```
```json
{
    "no123" : {
        "chinese" : "100",
        "math" : "140",
        "english" : "149",
        "summary" : "289"
    },
    "no124" : {
        "chinese" : "120",
        "math" : "120",
        "english" : "119",
        "summary" : "239"
    },
    "no125" : {
        "chinese" : "130",
        "math" : "110",
        "english" : "139",
        "summary" : "269"
    }
}
```

## 模板引擎
> 理解模板引擎本质
> 引擎基本使用

```javascript
/*
    模板引擎
*/
let template = require('art-template');
// let html = template(__dirname + '/mytpl.art', {
//     user: {
//         name: 'lisi'
//     }
// });
// console.log(html);
// ----------------------------------
// let tpl = '<ul>{{each list as value}}<li>{{value}}</li>{{/each}}</ul>';
// let render = template.compile(tpl);
// let ret = render({
//     list : ['apple','orange','banana']
// });
// console.log(ret);
// -----------------------------------
// let tpl = '<ul>{{each list as value}}<li>{{value}}</li>{{/each}}</ul>';
// let tpl = '<ul>{{each list}}<li>{{$index}}-------------{{$value}}</li>{{/each}}</ul>';
// let ret = template.render(tpl,{
//     list : ['apple','orange','banana','pineapple']
// });
// console.log(ret);

let html = template(__dirname + '/score.art', {
    chinese : '120',
    math : '130',
    english : '146',
    summary : '268'
});
console.log(html);
```
```html
<!-- mytpl.art -->
{{if user}}
  <h2>{{user.name}}</h2>
{{/if}}
```
```html
<!-- score.art -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>成绩结果</title>
</head>
<body>
    <div>
        <ul>
            <li>语文：{{chinese}}</li>
            <li>数学：{{math}}</li>
            <li>外语：{{english}}</li>
            <li>综合：{{summary}}</li>
        </ul>
    </div>
</body>
</html>
```

```javascript
/*
    优化
*/
const http = require('http');
const path = require('path');
const fs = require('fs');
const querystring = require('querystring');
const scoreData = require('./scores.json');
const template = require('art-template');

http.createServer((req,res)=>{
    // 路由（请求路径+请求方式）
    // 查询成绩的入口地址 /query
    if(req.url.startsWith('/query') && req.method == 'GET'){
        let content = template(path.join(__dirname,'view','index.art'),{});
        res.end(content);
    }else if(req.url.startsWith('/score') && req.method == 'POST'){
        // 获取成绩的结果 /score
        let pdata = '';
        req.on('data',(chunk)=>{
            pdata += chunk;
        });
        req.on('end',()=>{
            let obj = querystring.parse(pdata);
            let result = scoreData[obj.code];
            let content = template(path.join(__dirname,'view','result.art'),result);
            res.end(content);
        });
    }else if(req.url.startsWith('/all') && req.method == 'GET'){
        let arr = [];
        for(let key in scoreData){
            arr.push(scoreData[key]);
        }
        // 全部成绩
        let content = template(path.join(__dirname,'view','list.art'),{
            list : arr
        });
        res.end(content);
    }
}).listen(3000,()=>{
    console.log('running....');
});
```
```html
<!-- index.art -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>查询成绩</title>
</head>
<body>
    <form action="http://localhost:3000/score" method="post">
        请输入考号：<input type="text" name="code">
        <input type="submit" value="查询">
    </form>
    <div><a href="http://localhost:3000/all">全部成绩</a></div>
</body>
</html>
```
```html
<!-- result.art -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>成绩结果</title>
</head>
<body>
    <div>
        <ul>
            <li>语文：{{chinese}}</li>
            <li>数学：{{math}}</li>
            <li>外语：{{english}}</li>
            <li>综合：{{summary}}</li>
        </ul>
    </div>
</body>
</html>
```
```html
<!-- list.art -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>所有信息</title>
</head>
<body>
    <div>
        {{each list}}
        <ul>
            <li>语文：{{$value.chinese}}</li>
            <li>数学：{{$value.math}}</li>
            <li>外语：{{$value.english}}</li>
            <li>综合：{{$value.summary}}</li>
        </ul>
        {{/each}}
    </div>
</body>
</html>
```
```json
{
    "no123" : {
        "chinese" : "100",
        "math" : "140",
        "english" : "149",
        "summary" : "289"
    },
    "no124" : {
        "chinese" : "120",
        "math" : "120",
        "english" : "119",
        "summary" : "239"
    },
    "no125" : {
        "chinese" : "130",
        "math" : "110",
        "english" : "139",
        "summary" : "269"
    }
}
```

##小结
Node.js的Web开发相关的内容：
1、Node.js不需要依赖第三方应用软件（Apache），可以基于api自己实现
2、实现静态资源服务器
3、路由处理
4、动态网站
5、模板引擎
6、get和post参数传递和处理

## Express基本使用
- 静态服务器
- 路由
- 中间件
- 模板引擎整合
- 常用API基本使用

```javascript
/*
    初步实现服务器功能
*/
const express = require('express');
const app = express();
// const app = require('express')();

app.get('/',(req,res)=>{
    res.send('ok');
}).listen(3000,()=>{
    console.log('running...');
});

// ----------------------------------
let server = app.get('/',(req,res)=>{
    res.send('abc');
});
server.listen(3000,()=>{
    console.log('running...');
});
```

```javascript
/*
    托管静态文件
    可以指定虚拟目录
    可以指定多个目录作为静态资源目录
*/
const express = require('express');
const app = express();
// 实现静态资源服务
// use方法的第一个参数可以指定一个虚拟路径
// let server = app.use('/abc',express.static('public'));
// app.use('/nihao',express.static('hello'));
// server.listen(3000,()=>{
//     console.log('running...');
// });

// -----------------------------------
app.use('/abc',express.static('public'));
// app.use('/nihao',express.static('hello'));
app.listen(3000,()=>{
    console.log('running...');
});
//public、hello是文件夹，里面存放着文件资源
```

```javascript
/*
    路由（根据请求路径和请求方式进行路径分发处理）
    http的常用请求方式：
    post   添加
    get    查询
    put    更新
    delete 删除
    restful api (一种URL的格式)
*/
const express = require('express');
const app = express();

// 基本的路由处理
app.get('/',(req,res)=>{
    res.send('get data');
});
// app.post('/',(req,res)=>{
//     res.send('post data');
// });
// app.put('/',(req,res)=>{
//     res.send('put data');
// });
// app.delete('/',(req,res)=>{
//     res.send('delete data');
// });

// 直接使用use分发可以处理所有的路由请求
// app.use((req,res)=>{
//     res.send('ok');
// });

// all方法绑定的路由与请求方式无关
app.all('/abc',(req,res)=>{
    res.end('test router');
});

// route方法可以指定特定的请求方式
app.route('/hello')
   .get((req,res)=>{
       res.send('get data');
   }).post((req,res)=>{
       res.send('post data');
   });

// 封装路由模块
// const express = require('express');
// const router = express.Router();
// 
// router.get('/hi',(req,res)=>{
//     res.send('hi router');
// });
// 
// router.get('/hello',(req,res)=>{
//     res.send('hello router');
// });
// 
// router.post('/abc',(req,res)=>{
//     res.send('abc router');
// });
// 
// module.exports = router;
// 调用封装的路由模块
// const router = require('./myrouter.js');
// app.use('/admin',router);

app.listen(3000,()=>{
    console.log('running...');
});
```

## Express基本使用
- 中间件:应用级中间件 路由级中间件 错误处理中间件 内置中间件 第三方中间件
- 参数处理
- 模板引擎整合

```javascript
/*
    中间件：就是处理过程中的一个环节（本质上就是一个函数）
*/
const express = require('express');
const app = express();
let total = 0;

app.use((req,res,next)=>{
    console.log('有人访问');
    // next方法的作用就是把请求传递到下一个中间件
    next()
});
app.use('/user',(req,res,next)=>{
    // 记录访问时间
    console.log(Date.now());
    // next方法的作用就是把请求传递到下一个中间件
    next()
});
app.use('/user',(req,res,next)=>{
    // 记录访问日志
    console.log('访问了/user');
    next()
});
app.use('/user',(req,res)=>{
    total++;
    console.log(total);
    res.send('result');
});
app.listen(3000,()=>{
    console.log('running...');
});
```

```javascript
/*
    中间件的挂载方式和执行流程
    use方法
    路由方式:get post put delete
*/
const express = require('express');
const app = express();

app.get('/abc',(req,res,next)=>{
    console.log(1);
    // 跳转到下一个路由
    next('route');
},(req,res)=>{
    console.log(2);
    res.send('abc');
});

app.get('/abc',(req,res)=>{
    console.log(3);
    res.send('hello');
});

// --------------------------------
// var cb0 = function (req, res, next) {
//   console.log('CB0');
//   next();
// }
// var cb1 = function (req, res, next) {
//   console.log('CB1');
//   next();
// }
// var cb2 = function (req, res) {
//   res.send('Hello from C!');
// }
// app.get('/example', [cb0, cb1, cb2]);

app.listen(3000,()=>{
    console.log('running...');
});
```

```javascript
/*
    应用中间件
*/
const express = require('express');
const app = express();
const bodyParser = require('body-parser')
// 挂载内置中间件
app.use(express.static('public'));

// 挂载参数处理中间件（post）
app.use(bodyParser.urlencoded({ extended: false }));

// 处理get提交参数
app.get('/login',(req,res)=>{
    let data = req.query;
    console.log(data);
    res.send('get data');
});

// 处理post提交参数
app.post('/login',(req,res)=>{
    let data = req.body;
    // console.log(data);
    if(data.username == 'admin' && data.password == '123'){
        res.send('success');
    }else{
        res.send('failure');
    }
});

app.listen(3000,()=>{
    console.log('running...');
});
```

```javascript
/*
    参数处理
*/
const express = require('express');
const app = express();
const bodyParser = require('body-parser')
// 挂载内置中间件
app.use(express.static('public'));

// 挂载参数处理中间件（post）
app.use(bodyParser.urlencoded({ extended: false }));
// 处理json格式的参数
app.use(bodyParser.json());

// 处理get提交参数
app.get('/login',(req,res)=>{
    let data = req.query;
    console.log(data);
    res.send('get data');
});

// 处理post提交参数
app.post('/login',(req,res)=>{
    let data = req.body;
    // console.log(data);
    if(data.username == 'admin' && data.password == '123'){
        res.send('success');
    }else{
        res.send('failure');
    }
});

app.put('/login',(req,res)=>{
    res.end('put data');
});

app.delete('/login',(req,res)=>{
    res.end('delete data');
});

app.listen(3000,()=>{
    console.log('running...');
});
```

```javascript
/*
    模板引擎整合：art-template
*/
const express = require('express');
const path = require('path');
const template = require('art-template');
const app = express();

// 设置模板的路径
app.set('views',path.join(__dirname,'views'));
// 设置模板引擎
app.set('view engine','art');

// 使express兼容art-template模板引擎
app.engine('art', require('express-art-template'));

app.get('/list',(req,res)=>{
    let data = {
        title : '水果',
        list : ['apple','orange','banana']
    }
    // 参数一：模板名称；参数二：渲染模板的数据
    res.render('list',data);
});

app.listen(3000,()=>{
    console.log('running...');
});
```

## Express图书管理案例
```javascript
// index.js
const express = require('express');
const path = require('path');
const router = require('./router.js');
const template = require('art-template');
const bodyParser = require('body-parser');
const app = express();

// 启动静态资源服务
app.use('/www',express.static('public'));

// 设置模板引擎
// 设置模板的路径
app.set('views',path.join(__dirname,'views'));
// 设置模板引擎
app.set('view engine','art');
// 使express兼容art-template模板引擎
app.engine('art', require('express-art-template'));

// 处理请求参数
// 挂载参数处理中间件（post）
app.use(bodyParser.urlencoded({ extended: false }));
// 处理json格式的参数
app.use(bodyParser.json());

// 启动服务器功能
// 配置路由
app.use(router);
// 监听端口
app.listen(3000,()=>{
    console.log('running...');
});
```

```javascript
/*
    路由模块 router.js
*/
const express = require('express');
const router = express.Router();
const service = require('./service.js');

// 路由处理

// 渲染主页
router.get('/',service.showIndex);
// 添加图书(跳转到添加图书的页面)
router.get('/toAddBook',service.toAddBook);
// 添加图书(提交表单)
router.post('/addBook',service.addBook);
// 跳转到编辑图书信息页面
router.get('/toEditBook',service.toEditBook);
// 编辑图书提交表单
router.post('/editBook',service.editBook);
// 删除图书信息
router.get('/deleteBook',service.deleteBook);

module.exports = router;
```

```javascript
/*
    业务模块 service.js
*/
const data = require('./data.json');
const path = require('path');
const fs = require('fs');
// 自动生成图书编号（自增）
let maxBookCode = ()=>{
    let arr = [];
    data.forEach((item)=>{
        arr.push(item.id);
    });
    return Math.max.apply(null,arr);
}
// 把内存数据写入文件
let writeDataToFile = (res) => {
    fs.writeFile(path.join(__dirname,'data.json'),JSON.stringify(data,null,4),(err)=>{
        if(err){
            res.send('server error');
        }
        // 文件写入成功之后重新跳转到主页面
        res.redirect('/');
    });
}
// 渲染主页面
exports.showIndex = (req,res) => {
    res.render('index',{list : data});
}
// 跳转到添加图书的页面
exports.toAddBook = (req,res) => {
    res.render('addBook',{});
}
// 添加图书保存数据
exports.addBook = (req,res) => {
    // 获取表单数据
    let info = req.body;
    let book = {};
    for(let key in info){
        book[key] = info[key];
    }
    book.id = maxBookCode() + 1;
    data.push(book);
    // 把内存中的数据写入文件
    writeDataToFile(res);
}
// 跳转编辑图书页面
exports.toEditBook = (req,res) => {
    let id = req.query.id;
    let book = {};
    data.forEach((item)=>{
        if(id == item.id){
            book = item;
            return;
        }
    });
    res.render('editBook',book);
}
// 编辑图书更新数据
exports.editBook = (req,res) => {
    let info = req.body;
    data.forEach((item)=>{
        if(info.id == item.id){
            for(let key in info){
                item[key] = info[key];
            }
            return;
        }
    });
    // 把内存中的数据写入文件
    writeDataToFile(res);
}
// 删除图书信息
exports.deleteBook = (req,res) => {
    let id = req.query.id;
    data.forEach((item,index)=>{
        if(id == item.id){
            // 删除数组的一项数据
            data.splice(index,1);
        }
        return;
    });
    // 把内存中的数据写入文件
    writeDataToFile(res);
}
```

```html
<!-- index.art -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>图书管理系统</title>
    <link rel="stylesheet" type="text/css" href="/www/style.css">
</head>
<body>
    <div class="title">图书管理系统<a href="/toAddBook">添加图书</a></div>
    <div class="content">
        <table cellpadding="0" cellspacing="0">
            <thead>
                <tr>
                    <th>编号</th>
                    <th>名称</th>
                    <th>作者</th>
                    <th>分类</th>
                    <th>描述</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody>
                {{each list}}
                <tr>
                    <td>{{$value.id}}</td>
                    <td>{{$value.name}}</td>
                    <td>{{$value.author}}</td>
                    <td>{{$value.category}}</td>
                    <td>{{$value.desc}}</td>
                    <td><a href="/toEditBook?id={{$value.id}}">修改</a>|<a href="/deleteBook?id={{$value.id}}">删除</a></td>
                </tr>
                {{/each}}
                <!-- <tr>
                    <td>1</td>
                    <td>西游记</td>
                    <td>吴承恩</td>
                    <td>文学</td>
                    <td>佛教与道教的斗争</td>
                    <td><a href="#">修改</a>|<a href="#">删除</a></td>
                </tr>
                <tr>
                    <td>1</td>
                    <td>西游记</td>
                    <td>吴承恩</td>
                    <td>文学</td>
                    <td>佛教与道教的斗争</td>
                    <td><a href="#">修改</a>|<a href="#">删除</a></td>
                </tr>
                <tr>
                    <td>1</td>
                    <td>西游记</td>
                    <td>吴承恩</td>
                    <td>文学</td>
                    <td>佛教与道教的斗争</td>
                    <td><a href="#">修改</a>|<a href="#">删除</a></td>
                </tr> -->
            </tbody>
        </table>
    </div>
</body>
</html>
```

```html
<!-- addBook.art -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>添加图书</title>
</head>
<body>
    <div>添加图书</div>
    <form action="/addBook" method="post">
        名称：<input type="text" name="name"><br>
        作者：<input type="text" name="author"><br>
        分类：<input type="text" name="category"><br>
        描述：<input type="text" name="desc"><br>
        <input type="submit" value="提交">
    </form>
</body>
</html>
```

```html
<!-- editBook.art -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>修改图书</title>
</head>
<body>
    <div>修改图书</div>
    <form action="/editBook" method="post">
        <input type="hidden" name="id" value="{{id}}">
        名称：<input type="text" name="name" value="{{name}}"><br>
        作者：<input type="text" name="author" value="{{author}}"><br>
        分类：<input type="text" name="category" value="{{category}}"><br>
        描述：<input type="text" name="desc" value="{{desc}}"><br>
        <input type="submit" value="提交">
    </form>
</body>
</html>
```

```json
[
    {
        "id": "1",
        "name": "三国演义",
        "author": "罗贯中",
        "category": "文学",
        "desc": "一个杀伐纷争的年代"
    },
    {
        "id": "2",
        "name": "水浒传",
        "author": "施耐庵",
        "category": "文学",
        "desc": "108条好汉的故事"
    },
    {
        "id": "3",
        "name": "西游记",
        "author": "吴承恩",
        "category": "文学",
        "desc": "佛教与道教的斗争"
    },
    {
        "id": "4",
        "name": "红楼梦",
        "author": "曹雪芹",
        "category": "文学",
        "desc": "一个封建王朝的缩影"
    },
    {
        "name": "天龙八部",
        "author": "金庸",
        "category": "文学",
        "desc": "武侠小说",
        "id": 5
    }
]
```

## Node.js操作数据库
- Mysql环境准备
```javascript
/*
    把data.json文件中的数据拼接成insert语句
*/
const path = require('path');
const fs = require('fs');

fs.readFile(path.join(__dirname,'../','data.json'),'utf8',(err,content)=>{
    if(err) return;
    let list = JSON.parse(content);
    let arr = [];
    list.forEach((item)=>{
        let sql = `insert into book (name,author,category,description) values ('${item.name}','${item.author}','${item.category}','${item.desc}');`;
        arr.push(sql);
    });
    fs.writeFile(path.join(__dirname,'data.sql'),arr.join(''),'utf8',(err)=>{
        console.log('init data finished!');
    });
});
```

```javascript
/*
    操作数据库基本步骤
*/
// 加载数据库驱动
const mysql = require('mysql');
// 创建数据库连接
const connection = mysql.createConnection({
    host: 'localhost', // 数据库所在的服务器的域名或者IP地址
    user: 'root', // 登录数据库的账号
    password: '', // 登录数据库的密码
    database: 'book' // 数据库名称
});
// 执行连接操作
connection.connect();
// 操作数据库
connection.query('select count(*) as total from book', function(error, results, fields) {
    if (error) throw error;
    console.log('表book中共有', results[0].total + '条数据');
});
// 关闭数据库
connection.end();
```

## mysql第三方包基本使用
- 操作数据库基本步骤
- 实现增删改成基本操作
```javascript
/*
    插入数据
*/
// 加载数据库驱动
const mysql = require('mysql');
// 创建数据库连接
const connection = mysql.createConnection({
    host: 'localhost', // 数据库所在的服务器的域名或者IP地址
    user: 'root', // 登录数据库的账号
    password: '', // 登录数据库的密码
    database: 'book' // 数据库名称
});
// 执行连接操作
connection.connect();

let sql = 'insert into book set ?'
let data = {
    name : '明朝那些事',
    author : '当年明月',
    category : '文学',
    description : '明朝的历史'
}
// 操作数据库
connection.query(sql,data, function(error, results, fields) {
    if (error) throw error;
    // console.log(results);
    if(results.affectedRows == 1){
        console.log('数据插入成功');
    }
});
// 关闭数据库
connection.end();
```

```javascript
/*
    更新数据
*/
// 加载数据库驱动
const mysql = require('mysql');
// 创建数据库连接
const connection = mysql.createConnection({
    host: 'localhost', // 数据库所在的服务器的域名或者IP地址
    user: 'root', // 登录数据库的账号
    password: '', // 登录数据库的密码
    database: 'book' // 数据库名称
});
// 执行连接操作
connection.connect();

let sql = 'update book set name=?,author=?,category=?,description=? where id=?';
let data = ['浪潮之巅','吴军','计算机','IT巨头的兴衰史',8];

// 操作数据库
connection.query(sql,data, function(error, results, fields) {
    if (error) throw error;
    // console.log(results);
    if(results.affectedRows == 1){
        console.log('更新成功');
    }
});
// 关闭数据库
connection.end();
```

```javascript
/*
    删除数据
*/
// 加载数据库驱动
const mysql = require('mysql');
// 创建数据库连接
const connection = mysql.createConnection({
    host: 'localhost', // 数据库所在的服务器的域名或者IP地址
    user: 'root', // 登录数据库的账号
    password: '', // 登录数据库的密码
    database: 'book' // 数据库名称
});
// 执行连接操作
connection.connect();

let sql = 'delete from book where id = ?';
let data = [9];

// 操作数据库
connection.query(sql,data, function(error, results, fields) {
    if (error) throw error;
    // console.log(results);
    if(results.affectedRows == 1){
        console.log('删除成功');
    }
});
// 关闭数据库
connection.end();
```

```javascript
/*
    查询数据
*/
// 加载数据库驱动
const mysql = require('mysql');
// 创建数据库连接
const connection = mysql.createConnection({
    host: 'localhost', // 数据库所在的服务器的域名或者IP地址
    user: 'root', // 登录数据库的账号
    password: '', // 登录数据库的密码
    database: 'book' // 数据库名称
});
// 执行连接操作
connection.connect();

let sql = 'select * from book where id = ?';
let data = [6];

// 操作数据库
connection.query(sql,data, function(error, results, fields) {
    if (error) throw error;
    console.log(results[0].name);
    // console.log(results);
});
// 关闭数据库
connection.end();
```

```javascript
/*
    封装操作数据库的通用api
*/
const mysql = require('mysql');

exports.base = (sql,data,callback) => {
    // 创建数据库连接
    const connection = mysql.createConnection({
        host: 'localhost', // 数据库所在的服务器的域名或者IP地址
        user: 'root', // 登录数据库的账号
        password: '', // 登录数据库的密码
        database: 'book' // 数据库名称
    });
    // 执行连接操作
    connection.connect();

    // 操作数据库(数据库操作也是异步的)
    connection.query(sql,data, function(error, results, fields) {
        if (error) throw error;
        callback(results);
    });
    // 关闭数据库
    connection.end();
}
```

```javascript
/*
    测试通用api
*/
const db = require('./db.js');

// 插入操作
let sql = 'insert into book set ?';
let data = {
    name : '笑傲江湖',
    author : '金庸',
    category : '文学',
    description : '武侠小说'
}
db.base(sql,data,(result)=>{
    console.log(result);
});
// 更新操作
let sql = 'update book set name=?,author=?,category=?,description=? where ?';
let data = ['天龙八部','金庸','文学','武侠小说',11];
db.base(sql,data,(result)=>{
    console.log(result);
});
删除操作
let sql = 'delete from book where id = ?';
let data = [11];
db.base(sql,data,(result)=>{
    console.log(result);
});
// 查询操作
let sql = 'select * from book where id = ?';
let data = [8];
db.base(sql,data,(result)=>{
    console.log(result[0].name);
});
```

## 基于数据库实现登录功能
```javascript
/*
    登录验证（前端+后端+数据库）
*/
const express = require('express');
const bodyParser = require('body-parser');
const db = require('./db.js');
const app = express();

app.use(bodyParser.urlencoded({ extended: false }));
app.use(express.static('public'));

app.post('/check',(req,res)=>{
    let param = req.body;

    let sql = 'select count(*) as total from user where username=? and password=?';
    let data = [param.username,param.password];

    db.base(sql,data,(result)=>{
        if(result[0].total == 1){
            res.send('login success!');
        }else{
            res.send('login failure!');
        }
    });
});

app.listen(3000,()=>{
    console.log('running...');
});
```

```javascript
/*
    封装操作数据库的通用api
*/
const mysql = require('mysql');

exports.base = (sql,data,callback) => {
    // 创建数据库连接
    const connection = mysql.createConnection({
        host: 'localhost', // 数据库所在的服务器的域名或者IP地址
        user: 'root', // 登录数据库的账号
        password: '', // 登录数据库的密码
        database: 'book' // 数据库名称
    });
    // 执行连接操作
    connection.connect();

    // 操作数据库(数据库操作也是异步的)
    connection.query(sql,data, function(error, results, fields) {
        if (error) throw error;
        callback(results);
    });
    // 关闭数据库
    connection.end();
}
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
    <form action="http://localhost:3000/check" method="post">
        用户名：<input type="text" name="username"><br>
        密码：<input type="password" name="password"><br>
        <input type="submit" value="登录">
    </form>
</body>
</html>
```

## 图书管理系统整合数据库
```javascript
// index.js
const express = require('express');
const path = require('path');
const router = require('./router.js');
const template = require('art-template');
const bodyParser = require('body-parser');
const app = express();

// 启动静态资源服务
app.use('/www',express.static('public'));

// 设置模板引擎
// 设置模板的路径
app.set('views',path.join(__dirname,'views'));
// 设置模板引擎
app.set('view engine','art');
// 使express兼容art-template模板引擎
app.engine('art', require('express-art-template'));

// 处理请求参数
// 挂载参数处理中间件（post）
app.use(bodyParser.urlencoded({ extended: false }));
// 处理json格式的参数
app.use(bodyParser.json());

// 启动服务器功能
// 配置路由
app.use(router);
// 监听端口
app.listen(3000,()=>{
    console.log('running...');
});
```

```javascript
/*
    路由模块 router.js
*/
const express = require('express');
const router = express.Router();
const service = require('./service.js');

// 路由处理

// 渲染主页
router.get('/',service.showIndex);
// 添加图书(跳转到添加图书的页面)
router.get('/toAddBook',service.toAddBook);
// 添加图书(提交表单)
router.post('/addBook',service.addBook);
// 跳转到编辑图书信息页面
router.get('/toEditBook',service.toEditBook);
// 编辑图书提交表单
router.post('/editBook',service.editBook);
// 删除图书信息
router.get('/deleteBook',service.deleteBook);

module.exports = router;
```

```javascript
/*
    封装操作数据库的通用api db.js
*/
const mysql = require('mysql');

exports.base = (sql,data,callback) => {
    // 创建数据库连接
    const connection = mysql.createConnection({
        host: 'localhost', // 数据库所在的服务器的域名或者IP地址
        user: 'root', // 登录数据库的账号
        password: '', // 登录数据库的密码
        database: 'book' // 数据库名称
    });
    // 执行连接操作
    connection.connect();

    // 操作数据库(数据库操作也是异步的)
    connection.query(sql,data, function(error, results, fields) {
        if (error) throw error;
        callback(results);
    });
    // 关闭数据库
    connection.end();
}
```

```javascript
/*
    业务模块 service.js
*/
const data = require('./data.json');
const path = require('path');
const fs = require('fs');
const db = require('./db.js');

// 自动生成图书编号（自增）
let maxBookCode = ()=>{
    let arr = [];
    data.forEach((item)=>{
        arr.push(item.id);
    });
    return Math.max.apply(null,arr);
}
// 把内存数据写入文件
let writeDataToFile = (res) => {
    fs.writeFile(path.join(__dirname,'data.json'),JSON.stringify(data,null,4),(err)=>{
        if(err){
            res.send('server error');
        }
        // 文件写入成功之后重新跳转到主页面
        res.redirect('/');
    });
}

// 渲染主页面
exports.showIndex = (req,res) => {
    let sql = 'select * from book';
    db.base(sql,null,(result)=>{
        res.render('index',{list : result});
    });
}
// 跳转到添加图书的页面
exports.toAddBook = (req,res) => {
    res.render('addBook',{});
}
// 添加图书保存数据
exports.addBook = (req,res) => {
    // 获取表单数据
    let info = req.body;
    let book = {};
    for(let key in info){
        book[key] = info[key];
    }
    let sql = 'insert into book set ?';
    db.base(sql,book,(result)=>{
        if(result.affectedRows == 1){
            res.redirect('/');
        }
    });
}
// 跳转编辑图书页面
exports.toEditBook = (req,res) => {
    let id = req.query.id;
    let sql = 'select * from book where id=?';
    let data = [id];
    db.base(sql,data,(result)=>{
        res.render('editBook',result[0]);
    });
}
// 编辑图书更新数据
exports.editBook = (req,res) => {
    let info = req.body;
    let sql = 'update book set name=?,author=?,category=?,description=? where id=?';
    let data = [info.name,info.author,info.category,info.description,info.id];
    db.base(sql,data,(result)=>{
        if(result.affectedRows == 1){
            res.redirect('/');
        }
    });
}
// 删除图书信息
exports.deleteBook = (req,res) => {
    let id = req.query.id;
    let sql = 'delete from book where id=?';
    let data = [id];
    db.base(sql,data,(result)=>{
        if(result.affectedRows == 1){
            res.redirect('/');
        }
    });
}
```

```html
<!-- index.art -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>图书管理系统</title>
    <link rel="stylesheet" type="text/css" href="/www/style.css">
</head>
<body>
    <div class="title">图书管理系统<a href="/toAddBook">添加图书</a></div>
    <div class="content">
        <table cellpadding="0" cellspacing="0">
            <thead>
                <tr>
                    <th>编号</th>
                    <th>名称</th>
                    <th>作者</th>
                    <th>分类</th>
                    <th>描述</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody>
                {{each list}}
                <tr>
                    <td>{{$value.id}}</td>
                    <td>{{$value.name}}</td>
                    <td>{{$value.author}}</td>
                    <td>{{$value.category}}</td>
                    <td>{{$value.description}}</td>
                    <td><a href="/toEditBook?id={{$value.id}}">修改</a>|<a href="/deleteBook?id={{$value.id}}">删除</a></td>
                </tr>
                {{/each}}
                <!-- <tr>
                    <td>1</td>
                    <td>西游记</td>
                    <td>吴承恩</td>
                    <td>文学</td>
                    <td>佛教与道教的斗争</td>
                    <td><a href="#">修改</a>|<a href="#">删除</a></td>
                </tr>
                <tr>
                    <td>1</td>
                    <td>西游记</td>
                    <td>吴承恩</td>
                    <td>文学</td>
                    <td>佛教与道教的斗争</td>
                    <td><a href="#">修改</a>|<a href="#">删除</a></td>
                </tr>
                <tr>
                    <td>1</td>
                    <td>西游记</td>
                    <td>吴承恩</td>
                    <td>文学</td>
                    <td>佛教与道教的斗争</td>
                    <td><a href="#">修改</a>|<a href="#">删除</a></td>
                </tr> -->
            </tbody>
        </table>
    </div>
</body>
</html>
```

```html
<!-- addBook.art -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>添加图书</title>
</head>
<body>
    <div>添加图书</div>
    <form action="/addBook" method="post">
        名称：<input type="text" name="name"><br>
        作者：<input type="text" name="author"><br>
        分类：<input type="text" name="category"><br>
        描述：<input type="text" name="description"><br>
        <input type="submit" value="提交">
    </form>
</body>
</html>
```

```html
<!-- editBook.art -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>修改图书</title>
</head>
<body>
    <div>修改图书</div>
    <form action="/editBook" method="post">
        <input type="hidden" name="id" value="{{id}}">
        名称：<input type="text" name="name" value="{{name}}"><br>
        作者：<input type="text" name="author" value="{{author}}"><br>
        分类：<input type="text" name="category" value="{{category}}"><br>
        描述：<input type="text" name="description" value="{{description}}"><br>
        <input type="submit" value="提交">
    </form>
</body>
</html>
```

## 后台接口开发
- json接口
- jsonp接口
- restful接口
```javascript
/*
    后台接口开发
*/
const express = require('express');
const db = require('./db.js');
const app = express();

// 指定api路径 allBooks (json接口)
// app.get('/allBooks',(req,res)=>{
//     let sql = 'select * from book';
//     db.base(sql,null,(result)=>{
//         res.json(result);
//     });
// });

// 修改json回调函数传递参数的key
app.set('jsonp callback name', 'cb');
// 指定api路径 allBooks （jsonp接口）
app.get('/allBooks',(req,res)=>{
    let sql = 'select * from book';
    db.base(sql,null,(result)=>{
        res.jsonp(result);
    });
});

app.listen(3000,()=>{
    console.log('running...');
});
```
```javascript
// /*
//     封装操作数据库的通用api db.js
// */
// const mysql = require('mysql');
// 
// exports.base = (sql,data,callback) => {
//     // 创建数据库连接
//     const connection = mysql.createConnection({
//         host: 'localhost', // 数据库所在的服务器的域名或者IP地址
//         user: 'root', // 登录数据库的账号
//         password: '', // 登录数据库的密码
//         database: 'book' // 数据库名称
//     });
//     // 执行连接操作
//     connection.connect();
// 
//     // 操作数据库(数据库操作也是异步的)
//     connection.query(sql,data, function(error, results, fields) {
//         if (error) throw error;
//         callback(results);
//     });
//     // 关闭数据库
//     connection.end();
// }
```

```javascript
/*
    restful api  是从URL的格式来表述的
    get     http://localhost:3000/books
    get     http://localhost:3000/books/book
    post    http://localhost:3000/books/book
    get     http://localhost:3000/books/book/1
    put     http://localhost:3000/books/book
    delete  http://localhost:3000/books/book/2

    传统的URL风格
    http://localhost:3000/
    http://localhost:3000/toAddBook
    http://localhost:3000/addBook
    http://localhost:3000/toEditBook?id=1
    http://localhost:3000/editBook
    http://localhost:3000/deleteBook?id=2
*/
const express = require('express');
const db = require('./db.js');
const app = express();

app.get('/books',(req,res)=>{
    let sql = 'select * from book';
    db.base(sql,null,(result)=>{
        res.json(result);
    });
});

// http://localhost:3000/books/book/1
app.get('/books/book/:id',(req,res)=>{
    let id = req.params.id;
    let sql = 'select * from book where id=?';
    let data = [id];
    db.base(sql,data,(result)=>{
        res.json(result[0]);
    });
});

app.listen(3000,()=>{
    console.log('running...');
});
```

## 基于Ajax与后台接口的前端渲染案例
- 前端渲染与后端渲染分析
```javascript
// ---index.js
/*
    实现图书管理系统后台接口
*/
const express = require('express');
const bodyParser = require('body-parser');
const router = require('./router.js');
const app = express();

app.use('/www',express.static('public'));
app.use(bodyParser.urlencoded({ extended: false }));
app.use(router);

app.listen(3000,()=>{
    console.log('running...');
});

// ---router.js
/*
    get     http://localhost:3000/books
    get     http://localhost:3000/books/book
    post    http://localhost:3000/books/book
    get     http://localhost:3000/books/book/1
    put     http://localhost:3000/books/book
    delete  http://localhost:3000/books/book/2
*/
const express = require('express');
const service = require('./service.js');
const router = express.Router();
// 提供所有的图书信息
router.get('/books',service.allBooks);
// 添加图书信息时提交数据
router.post('/books/book',service.addBook);
// 编辑图书时根据id查询相应信息
router.get('/books/book/:id',service.getBookById);
// 提交编辑的数据
router.put('/books/book',service.editBook);
// 删除图书信息
router.delete('/books/book/:id',service.deleteBook);
// 查询天气
router.get('/weather/:id',service.queryWeather);

module.exports = router;

//---db.js
/*
    封装操作数据库的通用api
*/
const mysql = require('mysql');

exports.base = (sql,data,callback) => {
    // 创建数据库连接
    const connection = mysql.createConnection({
        host: 'localhost', // 数据库所在的服务器的域名或者IP地址
        user: 'root', // 登录数据库的账号
        password: '', // 登录数据库的密码
        database: 'book' // 数据库名称
    });
    // 执行连接操作
    connection.connect();

    // 操作数据库(数据库操作也是异步的)
    connection.query(sql,data, function(error, results, fields) {
        if (error) throw error;
        callback(results);
    });
    // 关闭数据库
    connection.end();
}

// ---service.js
const db = require('./db.js');
const weather = require('./api.js');

exports.allBooks = (req,res) => {
    let sql = 'select * from book';
    db.base(sql,null,(result)=>{
        res.json(result);
    });
};

exports.addBook = (req,res) => {
    let info = req.body;
    let sql = 'insert into book set ?';
    db.base(sql,info,(result)=>{
        if(result.affectedRows == 1){
            res.json({flag : 1});
        }else{
            res.json({flag : 2});
        }  
    });
};

exports.getBookById = (req,res) => {
    let id = req.params.id;
    let sql = 'select * from book where id=?';
    let data = [id];
    db.base(sql,data,(result)=>{
        res.json(result[0]);
    });
};

exports.editBook = (req,res) => {
    let info = req.body;
    let sql = 'update book set name=?,author=?,category=?,description=? where id=?';
    let data = [info.name,info.author,info.category,info.description,info.id];
    db.base(sql,data,(result)=>{
        if(result.affectedRows == 1){
            res.json({flag : 1});
        }else{
            res.json({flag : 2});
        }  
    });
};

exports.deleteBook = (req,res) => {
    let id = req.params.id;
    let sql = 'delete from book where id=?';
    let data = [id];
    db.base(sql,data,(result)=>{
        if(result.affectedRows == 1){
            res.json({flag : 1});
        }else{
            res.json({flag : 2});
        } 
    });
};

exports.queryWeather = (req,res) => {
    let cityCode = req.params.id;
    weather.queryWeather(cityCode,(data)=>{
        res.json({info : data.weatherinfo});
    });
}

// ---
/*
    调用第三方接口-获取天气信息-封装通用的天气查询模块
*/
const http = require('http');
const querystring = require('querystring');

exports.queryWeather = (cityCode,callback) => {
    let options = {
        protocol : 'http:',
        hostname : 'www.weather.com.cn',
        port : 80,
        path : '/data/sk/'+cityCode+'.html',
        method : 'get'
    }

    let req = http.request(options,(res)=>{
        let info = '';

        res.on('data',(chunk)=>{
            info += chunk;
        });
        res.on('end',()=>{
            info = JSON.parse(info);
            callback(info);
        });
    });

    req.end();
}

// ---index.js
$(function(){
    // 初始化数据列表
    function initList(){
        $.ajax({
            type : 'get',
            url : '/books',
            dataType : 'json',
            success : function(data){
                // 渲染数据列表
                var html = template('indexTpl',{list : data});
                $('#dataList').html(html);
                // 必须在渲染完成内容之后才可以操作DOM标签

                $('#dataList').find('tr').each(function(index,element){
                    var td = $(element).find('td:eq(5)');
                    var id = $(element).find('td:eq(0)').text();
                    // 绑定编辑图书的单击事件
                    td.find('a:eq(0)').click(function(){
                        editBook(id);
                    });
                    // 绑定删除图书的单击事件
                    td.find('a:eq(1)').click(function(){
                        deleteBook(id);
                    });

                    // 绑定添加图书信息的单击事件
                    addBook();
                    // 重置表单
                    var form = $('#addBookForm');
                    form.get(0).reset();
                    form.find('input[type=hidden]').val('');
                });
            }
        });
    }
    initList();
    // 删除图书信息
    function deleteBook(id){
        $.ajax({
            type : 'delete',
            url : '/books/book/' + id,
            dataType : 'json',
            success : function(data){
                // 删除图书信息之后重新渲染数据列表
                if(data.flag == '1'){
                    initList();
                }
            }
        });
    }
    // 编辑图书信息
    function editBook(id){
        var form = $('#addBookForm');
        // 先根据数据id查询最新的数据
        $.ajax({
            type : 'get',
            url : '/books/book/' + id,
            dataType : 'json',
            success : function(data){
                // 初始化弹窗
                var mark = new MarkBox(600,400,'编辑图书',form.get(0));
                mark.init();
                // 填充表单数据
                form.find('input[name=id]').val(data.id);
                form.find('input[name=name]').val(data.name);
                form.find('input[name=author]').val(data.author);
                form.find('input[name=category]').val(data.category);
                form.find('input[name=description]').val(data.description);
                // 对表单提交按钮重新绑定单击事件
                form.find('input[type=button]').unbind('click').click(function(){
                    // 编辑完成数据之后重新提交表单
                    $.ajax({
                        type : 'put',
                        url : '/books/book',
                        data : form.serialize(),
                        dataType : 'json',
                        success : function(data){
                            if(data.flag == '1'){
                                // 隐藏弹窗
                                mark.close();
                                // 重新渲染数据列表
                                initList();
                            }
                        }
                    });
                });
            }
        });
    }
    // 添加图书信息
    function addBook(){
        $('#addBookId').click(function(){
            var form = $('#addBookForm');
            // 实例化弹窗对象
            var mark = new MarkBox(600,400,'添加图书',form.get(0));
            mark.init();
            form.find('input[type=button]').unbind('click').click(function(){
                $.ajax({
                    type : 'post',
                    url : '/books/book',
                    data : form.serialize(),
                    dataType : 'json',
                    success : function(data){
                        if(data.flag == '1'){
                            // 关闭弹窗
                            mark.close();
                            // 添加图书成功之后重新渲染数据列表
                            initList();
                        }
                    }
                });
            });
        });
    }
    // 查询天气
    $('#weather').click(function(){
        var cityCode = $('select option:selected').val();
        $.ajax({
            type : 'get',
            url : '/weather/' + cityCode,
            dataType : 'json',
            success : function(data){
                var html = template('weatherTpl',data.info);
                var mark = new MarkBox(600,400,'天气信息',$(html).get(0));
                mark.init();
            }
        });
    });
});
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>图书管理系统</title>
    <link rel="stylesheet" type="text/css" href="/www/css/style.css">
    <script type="text/javascript" src="/www/js/jquery.js"></script>
    <script type="text/javascript" src="/www/js/template-web.js"></script>
    <script type="text/javascript" src="/www/js/dialog.js"></script>
    <script type="text/javascript" src="/www/js/index.js"></script>
    <script type="text/template" id="indexTpl">
        {{each list}}
        <tr>
            <td>{{$value.id}}</td>
            <td>{{$value.name}}</td>
            <td>{{$value.author}}</td>
            <td>{{$value.category}}</td>
            <td>{{$value.description}}</td>
            <td><a href="javascript:;">修改</a>|<a href="javascript:;">删除</a></td>
        </tr>
        {{/each}}
    </script>
    <script type="text/template" id="weatherTpl">
        <ul>
            <li>城市：{{city}}</li>
            <li>风向：{{WD}}</li>
            <li>级别：{{WS}}</li>
            <li>温度：{{temp}}</li>
            <li>时间：{{time}}</li>
        </ul>
    </script>
</head>
<body>
    <div class="title">图书管理系统<a id="addBookId" href="javascript:;">添加图书</a>
    <select>
        <option value="101010100">北京</option>
        <option value="101020100">上海</option>
        <option value="101280101">广州</option>
        <option value="101280601">深圳</option>
    </select>
    <input type="button" value="查询" id="weather">
    </div>
    <div class="content">
        <table cellpadding="0" cellspacing="0">
            <thead>
                <tr>
                    <th>编号</th>
                    <th>名称</th>
                    <th>作者</th>
                    <th>分类</th>
                    <th>描述</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody id="dataList">
                
            </tbody>
        </table>
    </div>

    <form class="form" id="addBookForm">
        <input type="hidden" name="id">
        名称：<input type="text" name="name"><br>
        作者：<input type="text" name="author"><br>
        分类：<input type="text" name="category"><br>
        描述：<input type="text" name="description"><br>
        <input type="button" value="提交">
    </form>
</body>
</html>
```

## 从服务器主动发送请求
- 基于核心模块主动发送请求
```javascript
/*
    从服务器主动发送请求
    http.request(options[, callback])
*/

const http = require('http');
const path = require('path');
const fs = require('fs');

let options = {
    hostname : 'www.baidu.com',
    port : 80
}

let req = http.request(options,(res)=>{
    let info = '';

    res.on('data',(chunk)=>{
        info += chunk;
    });
    res.on('end',()=>{
        fs.writeFile(path.join(__dirname,'baidu.html'),info,(err)=>{
            console.log('已经获取到百度主页的内容');
        });
    });
});

req.end();

// ---01.js
/*
    从服务器主动发送请求调用接口-查询数据
*/

const http = require('http');

let options = {
    protocol : 'http:',
    hostname : 'localhost',
    port : 3000,
    path : '/books'
}

let req = http.request(options,(res)=>{
    let info = '';

    res.on('data',(chunk)=>{
        info += chunk;
    });
    res.on('end',()=>{
        console.log(info);
    });
});

req.end();

// ---02.js
/*
    从服务器主动发送请求调用接口-添加数据
*/

const http = require('http');
const querystring = require('querystring');

let options = {
    protocol : 'http:',
    hostname : 'localhost',
    port : 3000,
    path : '/books/book',
    method : 'post',
    headers : {
        'Content-Type': 'application/x-www-form-urlencoded',
    }
}

let req = http.request(options,(res)=>{
    let info = '';

    res.on('data',(chunk)=>{
        info += chunk;
    });
    res.on('end',()=>{
        console.log(info);
    });
});

let data = querystring.stringify({
    name : 'adafa',
    author : 'asdfa',
    category : 'afdasf',
    description : 'adsfa'
});
req.write(data);
req.end();

// ---03.js
/*
    从服务器主动发送请求调用接口-根据id去查询数据
*/

const http = require('http');
const querystring = require('querystring');

let options = {
    protocol : 'http:',
    hostname : 'localhost',
    port : 3000,
    path : '/books/book/2',
    method : 'get'
}

let req = http.request(options,(res)=>{
    let info = '';

    res.on('data',(chunk)=>{
        info += chunk;
    });
    res.on('end',()=>{
        console.log(info);
    });
});

req.end();

// ---04.js
/*
    从服务器主动发送请求调用接口-编辑数据
*/

const http = require('http');
const querystring = require('querystring');

let options = {
    protocol : 'http:',
    hostname : 'localhost',
    port : 3000,
    path : '/books/book',
    method : 'put',
    headers : {
        'Content-Type': 'application/x-www-form-urlencoded',
    }
}

let req = http.request(options,(res)=>{
    let info = '';

    res.on('data',(chunk)=>{
        info += chunk;
    });
    res.on('end',()=>{
        console.log(info);
    });
});

let data = querystring.stringify({
    id : 34,
    name : 'ddddddddd',
    author : 'sssssssss',
    category : 'eeeeeeeee',
    description : 'gggggggg'
});
req.write(data);
req.end();

// ---05.js
/*
    从服务器主动发送请求调用接口-删除数据
*/

const http = require('http');
const querystring = require('querystring');

let options = {
    protocol : 'http:',
    hostname : 'localhost',
    port : 3000,
    path : '/books/book/34',
    method : 'delete'
}

let req = http.request(options,(res)=>{
    let info = '';

    res.on('data',(chunk)=>{
        info += chunk;
    });
    res.on('end',()=>{
        console.log(info);
    });
});

req.end();

```

## 调用第三方接口
- 针对第三方接口进行二次封装
- 扩展天气查询功能

```javascript
/*
    调用第三方接口-获取天气信息
*/
const http = require('http');
const querystring = require('querystring');

let cityCode = '101010300';
let options = {
    protocol : 'http:',
    hostname : 'www.weather.com.cn',
    port : 80,
    path : '/data/sk/'+cityCode+'.html',
    method : 'get'
}

let req = http.request(options,(res)=>{
    let info = '';
    res.on('data',(chunk)=>{
        info += chunk;
    });
    res.on('end',()=>{
        console.log(info);
    });
});

req.end();

// --- ---
/*
    调用第三方接口-获取天气信息-封装通用的天气查询模块
*/

const http = require('http');
const querystring = require('querystring');

exports.queryWeather = (cityCode,callback) => {
    let options = {
        protocol : 'http:',
        hostname : 'www.weather.com.cn',
        port : 80,
        path : '/data/sk/'+cityCode+'.html',
        method : 'get'
    }

    let req = http.request(options,(res)=>{
        let info = '';

        res.on('data',(chunk)=>{
            info += chunk;
        });
        res.on('end',()=>{
            info = JSON.parse(info);
            callback(info);
        });
    });
    req.end();
}
// ---
/*
    测试天气查询模块
*/
const weather = require('./weather.js');/**/

weather.queryWeather('101020100',(data)=>{
    console.log(data.weatherinfo.WD);
});
```

```javascript
/*
    通过第三方包superagent发送服务器请求
*/
const request = require('superagent');
// request.get('http://localhost:3000/books')
//        .end((err,res) => {
//            // console.log(res.body);//有数据格式
//            console.log(res.text);//字符串
//        });

// request.post('http://localhost:3000/books/book')
//        .set('Content-Type', 'application/x-www-form-urlencoded')
//        .send({name:'abadf',author:'adfads',category:'adsfads',description:'asdfads'})
//        .end((err,res) => {
//            console.log(res.body);//有数据格式
//        });

// request.get('http://localhost:3000/books/book/2')
//        .end((err,res) => {
//            console.log(res.body);//有数据格式
//        });

// request.put('http://localhost:3000/books/book')
//        .set('Content-Type', 'application/x-www-form-urlencoded')
//        .send({id:'39',name:'dddd',author:'fff',category:'ccc',description:'ggg'})
//        .end((err,res) => {
//            console.log(res.body);//有数据格式
//        });

// request.delete('http://localhost:3000/books/book/39')
//        .end((err,res) => {
//            console.log(res.body);//有数据格式
//        });

request.get('http://www.weather.com.cn/data/sk/101010100.html')
       .end((err,res) => {
           // console.log(res.body);//有数据格式
           console.log(res.text);
       });

```
