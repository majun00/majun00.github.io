---
title: Linux 命令-curl 常用命令
tags: 
- Linux
categories:
- Linux
---

# Linux 命令-curl 常用命令

## 下载单个文件

`cur http://www.demo.com` 默认将输出打印到标准输出中(STDOUT)中

## 通过-o/-O 选项保存下载的文件到指定的文件中

1. `-o`：将文件保存为命令行中指定的文件名的文件中
   `curl -o test.html http://www.demo.com/index.html` 将文件保存到本地并命名为 test.html
2. `-O`：使用 URL 中默认的文件名保存文件到本地
   `curl -O http://www.demo.com/index.html` 将文件下载到本地并命名为 index.html
3. `curl -O URL1 -O URL2` 同时获取多个文件
   若同时从同一站点下载多个文件时，curl 会尝试重用链接

### 补充

1. `curl http://www.demo.com/index.html > test.html` 可以使用 > 符号将输出重定向到本地文件中
2. 有时候下载图片可以能是前面的部分名称是一样的，就最后的尾椎名不一样 `curl -O http://www.linux.com/dodo[1-5].JPG` 这样就会把 dodo1，dodo2，dodo3，dodo4，dodo5 全部保存下来
3. 有时候下载的东西会比较大，这个时候我们可以分段下载。使用内置 option：-r

```
# curl -r 0-100 -o dodo1_part1.JPG http://www.linux.com/dodo1.JPG
# curl -r 100-200 -o dodo1_part2.JPG http://www.linux.com/dodo1.JPG
# curl -r 200- -o dodo1_part3.JPG http://www.linux.com/dodo1.JPG
# cat dodo1_part* > dodo1.JPG
```

这样就可以查看 dodo1.JPG 的内容了

## 通过-L 选项进行重定向

`curl -L http://www.baidu.com` 有时候我们想要 curl 做的，就是像浏览器一样跟随链接的跳转，获取最终的网页内容。我们可以在命令中添加 -L 选项来跟随链接重定向

## 通过使用-C 选项可对大文件使用断点续传功能

`curl -C - -O http://www.demo.com/index.html` 通过添加-C 选项继续对该文件进行下载，已经下载过的文件不会被重新下载, 突然掉线了，也可以使用以下的方式续传

## 通过--limit-rate 选项对 CURL 的最大网络使用进行限制

`curl --limit-rate 1000B -O http://www.demo.com/index.html` 下载速度最大不会超过 1000B/second

## 通过使用-z 选项来实现下载指定时间内修改过的文件

- 当下载一个文件时，可对该文件的最后修改日期进行判断，如果该文件在指定日期内修改过，就进行下载，否则不下载。
- `curl -z 21-Dec-11 http://www.demo.com/index.html` 若 index.html 文件在 2011/12/21 之后有过更新才会进行下载

## 通过-u 选项提供用户名和密码进行授权

1. `curl -u username:password URL`
2. `curl -u username URL` 通常的做法是在命令行只输入用户名，之后会提示输入密码，这样可以保证在查看历史记录时不会将密码泄露

## 从 FTP 服务器下载文件

- CURL 同样支持 FTP 下载，若在 url 中指定的是某个文件路径而非具体的某个要下载的文件名，CURL 则会列出该目录下的所有文件名而并非下载该目录下的所有文件
- `curl -u ftpuser:ftppass -O ftp://ftp_server/public_html/` 列出 public_html 下的所有文件夹和文件
- `curl -u ftpuser:ftppass -O ftp://ftp_server/public_html/xss.php` 下载 xss.php 文件

### 补充

1. `curl -O -u 用户名:密码 ftp://www.linux.com/dodo1.JPG`
2. `curl -O ftp://用户名:密码@www.linux.com/dodo1.JPG`
3. 显示下载进度条 `curl -# -O http://www.linux.com/dodo1.JPG`
4. 不会显示下载进度信息 `curl -s -O http://www.linux.com/dodo1.JPG`

## 通过 -T 选项可将指定的本地文件上传到 FTP 服务器上

1. `curl -u ftpuser:ftppass -T myfile.txt ftp://ftp.testserver.com` 将 myfile.txt 文件上传到服务器
2. `curl -u ftpuser:ftppass -T "{file1,file2}" ftp://ftp.testserver.com` 同时上传多个文件
3. `curl -u ftpuser:ftppass -T - ftp://ftp.testserver.com/myfile_1.txt` 从标准输入获取内容保存到服务器指定的文件中

## 通过使用 -v 和 -trace 获取更多的链接信息

## -x 选项可以为 CURL 添加代理功能

`curl -x proxysever.test.com:3128 http://google.co.in` 指定代理主机和端口
指定 proxy 服务器以及其端口

### 补充

很多时候上网需要用到代理服务器(比如是使用代理服务器上网或者因为使用 curl 别人网站而被别人屏蔽 IP 地址的时候)，幸运的是 curl 通过使用内置 option：-x 来支持设置代理
`curl -x 192.168.100.100:1080 http://www.linux.com`

## 保存与使用网站 cookie 信息

1. `curl -D sugarcookies http://localhost/sugarcrm/index.php` 将网站的 cookies 信息保存到 sugarcookies 文件中
2. `curl -b sugarcookies http://localhost/sugarcrm/index.php` 使用上次保存的 cookie 信息

### 补充

1. 使用 -c 保存 Cookie `curl -c "cookie-example" http://www.example.com`
2. 使用 -b 读取 Cookie，-b 后面既可以是 Cookie 字符串，也可以是保存了 Cookie 的文件名
   - `curl -b "JSESSIONID=D0112A5063D938586B659EF8F939BE24" http://www.example.com`
   - `curl -b "cookie-example" http://www.example.com`
3. 保存 http 的 response 里面的 header 信息。内置 option: -D

## 传递请求数据

默认 curl 使用 GET 方式请求数据，这种方式下直接通过 URL 传递数据,可以通过 --data/-d 方式指定使用 POST 方式传递数据

1. `curl -u username https://api.github.com/user?access_token=XXXXXXXXXX` GET 请求
2. `curl -u username --data "param1=value1&param2=value" https://api.github.com` POST 请求
3. `curl --data @filename https://github.api.com/authorizations` 也可以指定一个文件，将该文件中的内容当作数据传递给服务器端
4. 默认情况下，通过 POST 方式传递过去的数据中若有特殊字符，首先需要将特殊字符转义在传递给服务器端，如 value 值中包含有空格，则需要先将空格转换成%20，如：
   `curl -d "value%201" http://hostname.com`
5. 在新版本的 CURL 中，提供了新的选项 --data-urlencode，通过该选项提供的参数会自动转义特殊字符。
   `curl --data-urlencode "value 1" http://hostname.com`
6. 除了使用 GET 和 POST 协议外，还可以通过 -X 选项指定其它协议，如：
   `curl -I -X DELETE https://api.github.cim`
7. 上传文件
   `curl --form "fileupload=@filename.txt" http://hostname/resource`

### 补充

1. 使用 -G 选项可指定 GET 方式
2. 带 Cookie 登录
   `curl -c "cookie-login" -d "userName=tom&passwd=123456" http://www.example.com/login`
   再次访问该网站时，使用以下命令：
   `curl -b "cookie-login" http://www.example.com/login`
   这样，就能保持访问的是登录后的页面了。

## 可以使用 -I 选项显示 HTTP 头，而不显示文件内容，也可以使用 -i 选项同时显示 HTTP 头和文件内容

1. `curl -I http://www.codebelief.com`
2. `curl -i http://www.codebelief.com`

## 使用 -A 自定义 User-Agent

可以使用 -A 来自定义用户代理，例如下面的命令将伪装成安卓火狐浏览器对网页进行请求：
`curl -A "Mozilla/5.0 (Android; Mobile; rv:35.0) Gecko/35.0 Firefox/35.0" http://www.baidu.com`

## 使用 -H 自定义 header

当我们需要传递特定的 header 的时候，可以仿照以下命令来写：
`curl -H "Referer: www.example.com" -H "User-Agent: Custom-User-Agent" http://www.baidu.com`
可以看到，当我们使用 -H 来自定义 User-Agent 时，需要使用 "User-Agent: xxx" 的格式。
我们能够直接在 header 中传递 Cookie，格式与上面的例子一样：
`curl -H "Cookie: JSESSIONID=D0112A5063D938586B659EF8F939BE24" http://www.example.com`

## 伪造 referer（盗链）

很多服务器会检查 http 访问的 referer 从而来控制访问。比如：你是先访问首页，然后再访问首页中的邮箱页面，这里访问邮箱的 referer 地址就是访问首页成功后的页面地址，如果服务器发现对邮箱页面访问的 referer 地址不是首页的地址，就断定那是个盗连了
curl 中内置 option：-e 可以让我们设定 referer
`curl -e "www.linux.com" http://mail.linux.com` 这样就会让服务器其以为你是从 www.linux.com 点击某个链接过来的
