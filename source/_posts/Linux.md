---
title: Linux 常用命令
tags: 
- Linux
categories:
- Linux
---

# Linux 常用命令

## ls [-alrtAFR][name...]

`-a` 显示所有文件及目录 (ls 内定将文件名或目录名称开头为"."的视为隐藏档，不会列出)
`-l` 除文件名称外，亦将文件型态、权限、拥有者、文件大小等资讯详细列出
`-r` 将文件以相反次序显示(原定依英文字母次序)
`-t` 将文件依建立时间之先后次序列出
`-A` 同 `-a` ，但不列出 "." (目前目录) 及 ".." (父目录)
`-F` 在列出的文件名称后加一符号；例如可执行档则加 "\*", 目录则加 "/"
`-R` 若目录下有文件，则以下之文件亦皆依序列出

## touch [-acfm][-d<日期时间>][-r<参考文件或目录>][-t<日期时间>][--help][--version][文件或目录…]

用于修改文件或者目录的时间属性，包括存取时间和更改时间。若文件不存在，系统会建立一个新的文件。

## mkdir [-p] <dirName>

`-p` 确保目录名称存在，不存在的就建一个。
`mkdir -p BBB/Test` 在工作目录下的 BBB 目录中，建立一个名为 Test 的子目录。 若 BBB 目录原本不存在，则建立一个。（注：本例若不加 -p，且原本 BBB 目录不存在，则产生错误。）

## rm [options] <name>...

`-i` 删除前逐一询问确认。
`-f` 即使原档案属性设为唯读，亦直接删除，无需逐一确认。
`-r` 将目录及以下之档案亦逐一删除。

## mv [options] <source> <dest>

`-i`: 若指定目录已有同名文件，则先询问是否覆盖旧文件;
`-f`: 在 mv 操作要覆盖某已有的目标文件时不给任何指示;
`mv 文件名 文件名` 将源文件名改为目标文件名
`mv 文件名 目录名` 将文件移动到目标目录
`mv 目录名 目录名` 目标目录已存在，将源目录移动到目标目录；目标目录不存在则改名
`mv 目录名 文件名` 出错

## cp [options] <source> <dest>

`-a`：此选项通常在复制目录时使用，它保留链接、文件属性，并复制目录下的所有内容。其作用等于 dpR 参数组合。
`-d`：复制时保留链接。这里所说的链接相当于 Windows 系统中的快捷方式。
`-f`：覆盖已经存在的目标文件而不给出提示。
`-i`：与-f 选项相反，在覆盖目标文件之前给出提示，要求用户确认是否覆盖，回答"y"时目标文件将被覆盖。
`-p`：除复制文件的内容外，还把修改时间和访问权限也复制到新文件中。
`-r`：若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。
`-l`：不复制文件，只是生成链接文件。

## cat [-AbeEnstTuv][--help] [--version] <fileName>

`-n` 或 `--number`：由 1 开始对所有输出的行数编号。
`-b` 或 `--number-nonblank`：和 `-n` 相似，只不过对于空白行不编号。
`-s` 或 `--squeeze-blank`：当遇到有连续两行以上的空白行，就代换为一行的空白行。

## pwd 显示工作目录

## wget <URL> 从网站下载文件
`-q` 无提示下载
`-b` 后台下载
`-O` 指定不同的文件名
`–m` 下载整个网站
`--no-check-certificate` 绕过SSL/TLS证书的验证
`--user=<user_id> --password=<user_password>` 从密码保护的网站下载文件

## scp [可选参数] <file_source> <file_target>

`-r`： 递归复制整个目录。
`-P <port>`：注意是大写的 P, port 是指定数据传输用到的端口号

1、从本地复制到远程
`scp local_file remote_username@remote_ip:remote_folder`
或者
`scp local_file remote_username@remote_ip:remote_file`
或者
`scp local_file remote_ip:remote_folder`
或者
`scp local_file remote_ip:remote_file`
第 1,2 个指定了用户名，命令执行后需要再输入密码，第 1 个仅指定了远程的目录，文件名字不变，第 2 个指定了文件名；
第 3,4 个没有指定用户名，命令执行后需要输入用户名和密码，第 3 个仅指定了远程的目录，文件名字不变，第 4 个指定了文件名；

应用实例：
`scp /home/space/music/1.mp3 root@www.runoob.com:/home/root/others/music`
`scp /home/space/music/1.mp3 root@www.runoob.com:/home/root/others/music/001.mp3`
`scp /home/space/music/1.mp3 www.runoob.com:/home/root/others/music`
`scp /home/space/music/1.mp3 www.runoob.com:/home/root/others/music/001.mp3`

复制目录命令格式：

`scp -r local_folder remote_username@remote_ip:remote_folder`
或者
`scp -r local_folder remote_ip:remote_folder`
第 1 个指定了用户名，命令执行后需要再输入密码；
第 2 个没有指定用户名，命令执行后需要输入用户名和密码；
应用实例：

`scp -r /home/space/music/ root@www.runoob.com:/home/root/others/`
`scp -r /home/space/music/ www.runoob.com:/home/root/others/`
上面命令将本地 music 目录复制到远程 others 目录下。

2、从远程复制到本地
从远程复制到本地，只要将从本地复制到远程的命令的后 2 个参数调换顺序即可，如下实例

应用实例：
`scp root@www.runoob.com:/home/root/others/music /home/space/music/1.mp3`
`scp -r www.runoob.com:/home/root/others/ /home/space/music/`
说明 1.如果远程服务器防火墙有为 scp 命令设置了指定的端口，我们需要使用 -P 参数来设置命令的端口号，命令格式如下：
`scp -P 4588 remote@www.runoob.com:/usr/local/sin.sh /home/administrator` #scp 命令使用端口号 4588 使用 scp 命令要确保使用的用户具有可读取远程服务器相应文件的权限，否则 scp 命令是无法起作用的。

## rcp

1. `rcp [-pr][源文件或目录][目标文件或目录]`
2. `rcp [-pr][源文件或目录...][目标文件]`
   `-p` 　保留源文件或目录的属性，包括拥有者，所属群组，权限与时间。
   `-r`　递归处理，将指定目录下的文件与子目录一并处理。

使用 rcp 指令复制远程文件到本地进行保存。
设本地主机当前账户为 rootlocal，远程主机账户为 root，要将远程主机（218.6.132.5）主目录下的文件"testfile"复制到本地目录"test"中，则输入如下命令：
`rcp root@218.6.132.5:./testfile testfile` #复制远程文件到本地
`rcp root@218.6.132.5:home/rootlocal/testfile testfile` #要求当前登录账户 cmd 登录到远程主机
`rcp 218.6.132.5:./testfile testfile`

## ftp [-dignv][主机名称或ip地址]

## tftp [主机名称或 IP 地址]

## tar

`-c` 或`--create` 建立新的备份文件。
`-t` 或`--list` 列出备份文件的内容。
`-x` 或`--extract` 或`--get` 从备份文件中还原文件。
`-z` 或`--gzip` 或`--ungzip` 通过 gzip 指令处理备份文件。
`-f<备份文件>`或`--file=<备份文件>` 指定备份文件。
`-v` 或`--verbose` 显示指令执行过程。

```
压缩文件 非打包
# touch a.c
# tar -czvf test.tar.gz a.c //压缩 a.c文件为test.tar.gz
a.c

列出压缩文件内容
# tar -tzvf test.tar.gz
-rw-r--r-- root/root 0 2010-05-24 16:51:59 a.c

解压文件
# tar -xzvf test.tar.gz a.c
```

## gzip

`-c` 或`--stdout` 或`--to-stdout` 　把压缩后的文件输出到标准输出设备，不去更动原始文件。
`-d` 或`--decompress` 或`----uncompress` 　解开压缩文件。
`-f` 或`--force` 　强行压缩文件。不理会文件名称或硬连接是否存在以及该文件是否为符号连接。
`-l` 或`--list` 　列出压缩文件的相关信息。
`-r` 或`--recursive` 　递归处理，将指定目录下的所有文件及子目录一并处理。
`-v` 或`--verbose` 　显示指令执行过程。

```
压缩文件
# ls //显示当前目录文件
a.c b.h d.cpp
# gzip * //压缩目录下的所有文件
# ls //显示当前目录文件
a.c.gz b.h.gz d.cpp.gz


列出详细的信息
# gzip -dv * //解压文件，并列出详细信息
a.c.gz: 0.0% -- replaced with a.c
b.h.gz: 0.0% -- replaced with b.h
d.cpp.gz: 0.0% -- replaced with d.cpp


显示压缩文件的信息
# gzip -l *
compressed uncompressed ratio uncompressed_name
24 0 0.0% a.c
24 0 0.0% b.h
26 0 0.0% d.cpp
```

## zip

将 /home/html/ 这个目录下所有文件和文件夹打包为当前目录下的 html.zip：
`zip -q -r html.zip /home/html`

如果在我们在 /home/html 目录下，可以执行以下命令：
`zip -q -r html.zip *`

从压缩文件 cp.zip 中删除文件 a.c
`zip -dv cp.zip a.c`

## unzip

查看压缩文件中包含的文件：
`# unzip -l abc.zip`

-v 参数用于查看压缩文件目录信息，但是不解压该文件。
`# unzip -v abc.zip`



# 其他

## vim常用基本操作
- 按下i，进入编辑模式
- 按下esc退出编辑模式
- 按下:q 退出
- 按下:wq 保持并离开
[更多操作请点这里](http://www.runoob.com/linux/linux-vim.html)

## yum 管理软件包
```
yum install <package_name> 安装指定包
yum remove <package_name> 移除指定包
yum update <package_name> 更新指定包
yum search <keyword> 搜索指定包
yum list 列出全部可安装包
yum update 更新所有包
yum clean packages 清除缓存目录下的包
```

## 遇到端口被占用的处理
```
ps -ef | grep 应用  可以取到进程号
netstat -apn|grep 端口号  可以取到进程号
kill -9 进程号
```

> 关于linux的crul命令我单独写了一篇博客[linux的crul命令](https://www.jianshu.com/p/51a6233fe91c)



