---
title: 学习yarn
tags: 
- Yarn
categories:
- Yarn
---

# 常用命令
#### 通过 npm 安装
```
npm install --global yarn
```
执行 `set PATH=%PATH%;C:\.yarn\bin` 来重新设置环境。

#### 初始化一个新的项目
```
yarn init
```

#### 添加一个依赖包
```
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]
```

#### 更新一个依赖包
```
yarn upgrade [package]
yarn upgrade [package]@[version]
yarn upgrade [package]@[tag]
```

#### 删除一个依赖包
```
yarn remove [package]
```

#### 安装所有的依赖包
```
yarn
```
or
```
yarn install
```

# 使用教程
#### **三步走**
1.  项目下初始化 - 切到你的项目下，执行`yarn init`，会在根目录下生成一个`package.json`
2.  添加依赖安装
3.  开工

### **命令解释**
*   `yarn init` 

#### **添加依赖**
*   yarn add [package] — 添加包，会自动安装最新版本，注意会覆盖指定版本号！！！
*   `yarn add [package]@[version]` — 带版本号安装
*   `yarn add [package]@[tag]` — tag，是指代git上的推送的tag【no release!!】

#### **更新依赖**
*   `yarn upgrade [package]` — 更新某个包
*   `yarn upgrade [package]@[version]` — 指定更新到某个版本
*   `yarn upgrade [package]@[tag]` — 指定更新版本到某个标签
以上不能使用， 唯一能用的是在项目下执行，`yarn upgarde`, 会遍历所有依赖，然后全部更新

#### **移除依赖**
*   `yarn remove [package]` — 移除某个包 

#### **在其他项目启动项目**
类似npm，执行`npm install`;
yarn管理器支持两种安装所有依赖的命令：`yarn` 或者 `yarn install`
yarn管理器有一个很重要的文件需要注意，就是**`yarn.lock`**，这个是用来依赖的正确性，快速可靠安装的；是执行cli的时候自动生成的，在项目的根目录下，需要保留！！！！不要编辑它，这是自动生成的 在其他电脑初始化，必须记得把`package.json`和`yarn.lock`复制过去，简直就是秒下载【缓存机制】

### yarn.lock 文件
npm 和 Yarn 都使用 `package.json` 来跟踪项目的依赖，版本号并非一直准确，因为你可以定义版本号范围，这样你可以选择一个主版本和次要版本的包，但让 npm 安装最新的补丁也许可以修改一些 bug。

理想状态下使用[语义化版本](http://semver.org/)发布补丁不会包含大的变化，但不幸的是这必非真理。npm 的这种策略可能导致两台拥有相同 `package.json` 文件的机子安装了不同版本的包，这可能导致一些错误。

为了避免包版本的错误匹配，一个确定的安装版本被固定在一个锁文件中。每次模块被添加时，Yarn 就会创建（或更新）`yarn.lock` 文件，这样你就可以保证其它机子也安装相同版本的包，同时包含了 `package.json` 中定义的一系列允许的版本。

在 npm 中同样可以使用 `npm shrinkwrap` 命令来生成一个锁文件，这样在使用 `npm install` 时会在读取 `package.json` 前先读取这个文件，就像 Yarn 会先读取`yarn.lock` 一样。这里的区别是 Yarn 总会自动更新 `yarn.lock`，而 npm 需要你重新操作。

1.  [yarn.lock 文档](https://yarnpkg.com/en/docs/configuration#toc-use-yarn-lock-to-pin-your-dependencies)
2.  [npm shrinkwrap 文档](https://docs.npmjs.com/cli/shrinkwrap)

### 并行安装
每当 npm 或 Yarn 需要安装一个包时，它会进行一系列的任务。在 npm 中这些任务是按包的顺序一个个执行，这意味着必须等待上一个包被完整安装才会进入下一个；Yarn 则并行的执行这些任务，提高了性能。

为了比较，我在没有使用 shrinkwrap/yarn.lock 的方式以及清理了缓存下使用 npm 与 Yarn 安装 [express](https://www.npmjs.com/package/express)，总共安装了 42 个依赖。

*   npm: 9 s
*   Yarn: 1.37 s

我无法相信自己的眼睛，所以重复以上步骤，但得到相同结果。接着我安装 [gulp](https://www.npmjs.com/package/gulp) 进行测试，总共安装了 195 个依赖。

*   npm: 11 s
*   Yarn: 7.81 s

似乎根据所需要安装的包的数量而有所不同，但 Yarn 依旧比较快。

### 清晰的输出
npm 默认情况下非常冗余，例如使用 `npm install` 时它会递归列出所有安装的信息；而 Yarn 则一点也不冗余，当可以使用其它命令时，它适当的使用 emojis 表情来减少信息（Windows 除外）。

## Yarn vs npm: CLI 的差异
除了一些功能差异，Yarn 命令也存在一些区别。例如移除或修改了一些 npm 命令以及添加了几个有趣的命令。

### yarn global
不像 npm 添加 `-g` 或 `--global` 可以进行全局安装，Yarn 使用的是 `global` 前缀。不过与 npm 类似，项目依赖不推荐全局安装。

`global` 前缀只能用于 `yarn add`, `yarn bin`, `yarn ls` 和 `yarn remove`，除`yarn add` 外，这些命令都和 npm 等效。


### yarn install
`npm install` 命令会根据 `package.json` 安装依赖以及允许你添加新的模块；`yarn install` 仅会按 `yarn.lock` 或 `package.json` 里面的依赖顺序来安装模块。

1.  [yarn install 文档](https://yarnpkg.com/en/docs/cli/install)
2.  [npm install 文档](https://docs.npmjs.com/cli/install)

### yarn add [–dev]
与 `npm install` 类似，`yarn add` 允许你添加与安装模块，就像命令的名称一样，添加依赖意味着也会算定将依赖写入 `package.json`，类似 npm 的 `--save` 参数；Yarn 的 `--dev` 参数则是添加开发依赖，类似 npm 的 `--save-dev` 参数。

1.  [yarn add 文档](https://yarnpkg.com/en/docs/cli/add)
2.  [npm install 文档](https://docs.npmjs.com/cli/install)

### yarn licenses [ls|generate-disclaimer]
npm 没有类似命令来方便编写自己的包。`yarn licenses ls` 列出所有已安装包的许可协议。`yarn licenses generate-disclaimer` 生成包含已安装包许可协议的免责声明。某些协议要求使用者必须在项目中包含该协议，这时候该命令将变得非常好用。

1.  [yarn licenses 文档](https://yarnpkg.com/en/docs/cli/licenses)

### yarn why
该命令会查找依赖关系并找出为什么会将某些包安装在你的项目中。也许你明确为什么添加，也许它只是你安装包中的一个依赖，`yarn why` 可以帮你弄找出。

1.  [yarn why 文档](https://yarnpkg.com/en/docs/cli/why)

### yarn upgrade
该命令会根据符合 `package.json` 设定的规则而不是 `yarn.lock` 定义的确切版本来将包更新到最新版本。如果想用 npm 来实现相同目的，可以这样执行：
```
rm -rf node_modules
npm install
```
不要将该命令与 `npm update` 混淆，它指的是更新到自己的最新版。
1.  [yarn upgrade 文档](https://yarnpkg.com/en/docs/cli/upgrade)

### yarn generate-lock-entry
`yarn generate-lock-entry` 会基于 `package.json` 设置的依赖生成 `yarn.lock` 文件，该命令与 `npm shrinkwrap` 类似，但应该小心使用，因为通过 `yarn add` 和 `yarn upgrade` 命令添加或更新依赖时会自动更新生成该锁文件。

1.  [yarn generate-lock-entry 文档](https://yarnpkg.com/en/docs/cli/generate-lock-entry)
2.  [npm shrinkwrap 文档](https://docs.npmjs.com/cli/shrinkwrap)

下面介绍一些常用的命令：
*   `yarn`和 `yarn install` ，这两个命令的效果是一样的，等同于`npm install`，使用这个命令会在该目录生成一个yarn.lock的文件。

*   `yarn add koa`，安装`koa`模块并更新package.json和yarn.lock文件，等同于`npm install koa --save`。也可以使用`yarn global add koa`，等同于`npm install koa -g`，将模块直接安装到全局环境变量里，方便使用。

*   `yarn list`，根据当前项目的package.json查看模块的依赖及版本。

*   `yarn info koa`，查看`koa`模块的详细信息，类似于`npm view koa`。

*   `yarn init`，初始化项目package.json文件，等同于`npm init`。

*   `yarn run`，运行package.json中的`script`。

注意 : 之前说了npm存在一些历史遗留问题 比如说你的项目模块依赖是图中描述的，`@1.2.1`代表这个模块的版本。在你安装A的时候需要安装依赖C和D，很多依赖不会指定版本号，默认会安装最新的版本，这样就会出现问题：比如今天安装模块的时候C和D是某一个版本，而当以后C、D更新的时候，再次安装模块就会安装C和D的最新版本，如果新的版本无法兼容你的项目，你的程序可能就会出BUG，甚至无法运行。这就是npm的弊端，而yarn为了解决这个问题推出了yarn.lock的机制，这是作者项目中的yarn.lock文件。大家会看到，这个文件已经把依赖模块的版本号全部锁定，当你执行`yarn install`的时候，yarn会读取这个文件获得依赖的版本号，然后依照这个版本号去安装对应的依赖模块，这样依赖就会被锁定，以后再也不用担心版本号的问题了。其他人或者其他环境下使用的时候，把这个yarn.lock拷贝到相应的环境项目下再安装即可，**注意：这个文件不要手动修改它，当你使用一些操作如`yarn add`时，yarn会自动更新yarn.lock。**

