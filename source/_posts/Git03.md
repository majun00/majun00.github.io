---
title: Git学习(3)--基础知识
tags: 
- Git
categories:
- Git
---

## Git 工作区、暂存区和版本库
- 工作区：就是你在电脑里能看到的目录。
- 暂存区：英文叫stage, 或index。一般存放在 ".git目录下" 下的index文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
- 版本库：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

> 1. 当对工作区修改（或新增）的文件执行 "git add" 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。
> 2. 当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。
> 3. 当执行 "git reset HEAD" 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。
> 4. 当执行 "git rm --cached <file>" 命令时，会直接从暂存区删除文件，工作区则不做出改变。
> 5. 当执行 "git checkout ." 或者 "git checkout -- <file>" 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。
> 6. 当执行 "git checkout HEAD ." 或者 "git checkout HEAD <file>" 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

## Git 创建仓库
#### git init
Git 使用 git init 命令来初始化一个 Git 仓库，Git 的很多命令都需要在 Git 的仓库中运行，所以 git init 是使用 Git 的第一个命令。在执行完成 git init 命令后，Git 仓库会生成一个 .git 目录，该目录包含了资源的所有元数据，其他的项目目录保持不变（不像 SVN 会在每个子目录生成 .svn 目录，Git 只在仓库的根目录生成 .git 目录）。
使用当前目录作为Git仓库，我们只需使它初始化。
```
git init
```
该命令执行完后会在当前目录生成一个 .git 目录。
使用我们指定目录作为Git仓库。
```
git init newrepo
```
初始化后，会在 newrepo 目录下会出现一个名为 .git 的目录，所有 Git 需要的数据和资源都存放在这个目录中。如果当前目录下有几个文件想要纳入版本控制，需要先用 git add 命令告诉 Git 开始对这些文件进行跟踪，然后提交：
```
$ git add *.c
$ git add README
$ git commit -m '初始化项目版本'
```
以上命令将目录下以 .c 结尾及 README 文件提交到仓库中。

#### git clone
我们使用 git clone 从现有 Git 仓库中拷贝项目（类似 svn checkout）。
克隆仓库的命令格式为：
```
$ git clone <repo>
```
如果我们需要克隆到指定的目录，可以使用以下命令格式：
```
$ git clone <repo> <directory>
```
> 参数说明：repo:Git 仓库。directory:本地目录。

比如，要克隆 Ruby 语言的 Git 代码仓库 Grit，可以用下面的命令：
```
$ git clone git://github.com/schacon/grit.git
```
执行该命令后，会在当前目录下创建一个名为grit的目录，其中包含一个 .git 的目录，用于保存下载下来的所有版本记录。如果要自己定义要新建的项目目录名称，可以在上面的命令末尾指定新的名字：
```
$ git clone git://github.com/schacon/grit.git mygrit
```

## Git 基本操作
### 获取与创建项目命令
####git init
用 git init 在目录中创建新的 Git 仓库。 你可以在任何时候、任何目录中这么做，完全是本地化的。在目录中执行 git init，就可以创建一个 Git 仓库了。比如我们创建 hello 项目：
```
$ mkdir hello
$ cd hello/
$ git init
Initialized empty Git repository in /Users/tianqixin/www/hello/.git/
# 在 /www/hello/.git/ 目录初始化空 Git 仓库完毕。
```
现在你可以看到在你的项目中生成了 .git 这个子目录。 这就是你的 Git 仓库了，所有有关你的此项目的快照数据都存放在这里。
```
ls -a
.    ..    .git
```

#### git clone
使用 git clone 拷贝一个 Git 仓库到本地，让自己能够查看该项目，或者进行修改。如果你需要与他人合作一个项目，或者想要复制一个项目，看看代码，你就可以克隆那个项目。 执行命令：
```
 git clone [url]
```
[url] 为你想要复制的项目，就可以了。
例如我们克隆 Github 上的项目：
```
$ git clone git@github.com:schacon/simplegit.git
Cloning into 'simplegit'...
remote: Counting objects: 13, done.
remote: Total 13 (delta 0), reused 0 (delta 0), pack-reused 13
Receiving objects: 100% (13/13), done.
Resolving deltas: 100% (2/2), done.
Checking connectivity... done.
```
克隆完成后，在当前目录下会生成一个 simplegit 目录：
```
$ cd simplegit/ $ ls README Rakefile lib
```
上述操作将复制该项目的全部记录。
```
$ ls -a
.        ..       .git     README   Rakefile lib
$ cd .git
$ ls
HEAD        description info        packed-refs
branches    hooks       logs        refs
config      index       objects
```
默认情况下，Git 会按照你提供的 URL 所指示的项目的名称创建你的本地项目目录。 通常就是该 URL 最后一个 / 之后的项目名称。如果你想要一个不一样的名字， 你可以在该命令后加上你想要的名称。

### 基本快照
Git 的工作就是创建和保存你的项目的快照及与之后的快照进行对比。

####git add
git add 命令可将该文件添加到缓存，如我们添加以下两个文件：
```
$ touch README
$ touch hello.php
$ ls
README        hello.php
$ git status -s
?? README
?? hello.php
$ 
```
git status 命令用于查看项目的当前状态。
接下来我们执行 git add 命令来添加文件：
```
$ git add README hello.php 
```
现在我们再执行 git status，就可以看到这两个文件已经加上去了。
```
$ git status -s
A  README
A  hello.php
$ 
```
新项目中，添加所有文件很普遍，我们可以使用 git add . 命令来添加当前项目的所有文件。
现在我们修改 README 文件：
```
$ vim README
```
在 README 添加以下内容：# hello Git 测试，然后保存退出。
再执行一下 git status：
```
$ git status -s
AM README
A  hello.php
```
"AM" 状态的意思是，这个文件在我们将它添加到缓存之后又有改动。改动后我们在执行 git add 命令将其添加到缓存中：
```
$ git add .
$ git status -s
A  README
A  hello.php
```
当你要将你的修改包含在即将提交的快照里的时候，需要执行 git add。

#### git status
git status 以查看在你上次提交之后是否有修改。我演示该命令的时候加了 -s 参数，以获得简短的结果输出。如果没加该参数会详细输出内容：
```
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

    new file:   README
    new file:   hello.php
```

#### git diff
执行 git diff 来查看执行 git status 的结果的详细信息。git diff 命令显示已写入缓存与已修改但尚未写入缓存的改动的区别。git diff 有两个主要的应用场景。
- 尚未缓存的改动：git diff
- 查看已缓存的改动： git diff --cached
- 查看已缓存的与未缓存的所有改动：git diff HEAD
- 显示摘要而非整个 diff：git diff --stat
在 hello.php 文件中输入以下内容：
```
<?php
echo 'hello';
?>
```
```
$ git status -s
A  README
AM hello.php
$ git diff
diff --git a/hello.php b/hello.php
index e69de29..69b5711 100644
--- a/hello.php
+++ b/hello.php
@@ -0,0 +1,3 @@
+<?php
+echo 'hello';
+?>
```
git status 显示你上次提交更新后的更改或者写入缓存的改动， 而 git diff 一行一行地显示这些改动具体是啥。接下来我们来查看下 git diff --cached 的执行效果：
```
$ git add hello.php 
$ git status -s
A  README
A  hello.php
$ git diff --cached
diff --git a/README b/README
new file mode 100644
index 0000000..8f87495
--- /dev/null
+++ b/README
@@ -0,0 +1 @@
+# hello Git 测试
diff --git a/hello.php b/hello.php
new file mode 100644
index 0000000..69b5711
--- /dev/null
+++ b/hello.php
@@ -0,0 +1,3 @@
+<?php
+echo 'hello';
+?>
```

#### git commit
使用 git add 命令将想要快照的内容写入缓存区， 而执行 git commit 将缓存区内容添加到仓库中。Git 为你的每一个提交都记录你的名字与电子邮箱地址，所以第一步需要配置用户名和邮箱地址。
```
$ git config --global user.name 'hello'
$ git config --global user.email test@hello.com
```
接下来我们写入缓存，并提交对 hello.php 的所有改动。在首个例子中，我们使用 -m 选项以在命令行中提供提交注释。
```
$ git add hello.php
$ git status -s
A  README
A  hello.php
$ $ git commit -m '第一次版本提交'
[master (root-commit) d32cf1f] 第一次版本提交
 2 files changed, 4 insertions(+)
 create mode 100644 README
 create mode 100644 hello.php
```
现在我们已经记录了快照。如果我们再执行 git status:
```
$ git status
# On branch master
nothing to commit (working directory clean)
```
以上输出说明我们在最近一次提交之后，没有做任何改动，是一个"working directory clean：干净的工作目录"。如果你没有设置 -m 选项，Git 会尝试为你打开一个编辑器以填写提交信息。 如果 Git 在你对它的配置中找不到相关信息，默认会打开 vim。屏幕会像这样：
```
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
# modified:   hello.php
#
~
~
".git/COMMIT_EDITMSG" 9L, 257C
```
如果你觉得 git add 提交缓存的流程太过繁琐，Git 也允许你用 -a 选项跳过这一步。命令格式如下：
```
git commit -a
```
我们先修改 hello.php 文件为以下内容：
```
<?php
echo 'hello';
echo 'hello';
?>
```
再执行以下命令：
```
git commit -am '修改 hello.php 文件'
[master 71ee2cb] 修改 hello.php 文件
 1 file changed, 1 insertion(+)
```

#### git reset HEAD
git reset HEAD 命令用于取消已缓存的内容。我们先改动文件 README 文件，内容如下：
```
# hi
# hi 
```
hello.php 文件修改为：
```
<?php
echo 'hello';
echo 'hello';
echo 'hello';
?>
```
现在两个文件修改后，都提交到了缓存区，我们现在要取消其中一个的缓存，操作如下：
```
$ git status -s
 M README
 M hello.php
$ git add .
$ git status -s
M  README
M  hello.pp
$ git reset HEAD -- hello.php 
Unstaged changes after reset:
M    hello.php
$ git status -s
M  README
 M hello.php
```
现在你执行 git commit，只会将 README 文件的改动提交，而 hello.php 是没有的。
```
$ git commit -m '修改'
[master f50cfda] 修改
 1 file changed, 1 insertion(+)
$ git status -s
 M hello.php
```
可以看到 hello.php 文件的修改并为提交。这时我们可以使用以下命令将 hello.php 的修改提交：
```
$ git commit -am '修改 hello.php 文件'
[master 760f74d] 修改 hello.php 文件
 1 file changed, 1 insertion(+)
$ git status
On branch master
nothing to commit, working directory clean
```
简而言之，执行 git reset HEAD 以取消之前 git add 添加，但不希望包含在下一提交快照中的缓存。

#### git rm
如果只是简单地从工作目录中手工删除文件，运行 git status 时就会在 Changes not staged for commit 的提示。要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除，然后提交。可以用以下命令完成此项工作
```
git rm <file>
```
如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f
```
git rm -f <file>
```
如果把文件从暂存区域移除，但仍然希望保留在当前工作目录中，换句话说，仅是从跟踪清单中删除，使用 --cached 选项即可
```
git rm --cached <file>
```
如我们删除 hello.php文件：
```
$ git rm hello.php 
rm 'hello.php'
$ ls
README
```
不从工作区中删除文件：
```
$ git rm --cached README 
rm 'README'
$ ls
README
```
可以递归删除，即如果后面跟的是一个目录做为参数，则会递归删除整个目录中的所有子目录和文件：
```
git rm –r * 
```
进入某个目录中，执行此语句，会删除该目录下的所有文件和子目录。

#### git mv
git mv 命令用于移动或重命名一个文件、目录、软连接。
我们先把刚移除的 README 添加回来：
```
$ git add README 
然后对其重名:
$ git mv README  README.md
$ ls
README.md
```


> 参考自http://www.runoob.com/git/git-tutorial.html
