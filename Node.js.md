### Node.js

#### 1. 什么是node.js

- 既不是语言,也不是框架,是一个平台

- 没有BOM,DOM

- node为JavaScript 提供了一些服务器级别的API

  - 文件操作
  - http服务

- 在node 中没有全局作用域的概念

- 只能用require 的方法来加载多个JavaScript 脚本文件

- 需要把被外部使用的成员手动挂载到`exports`接口对象中

- 谁`require` 这个 模块,谁就可以得到模块内部的`exports`接口对象

- Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。

  Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。

#### 2. 核心模块

- 核心模块是由 Node 提供的一个个具名的模块,它们都有自己的特殊名称标识

  - fs  文件操作模块

  -  http  网络服务构建模块

  -  os  操作系统信息模块

  -  path  路径处理模块

    ...

#### 3. http

- require
- 端口号
  - ip 地址定位计算机
  - 端口号定位具体的应用程序
- Content-Type
  - 服务器最好把每次响应的数据是什么内容类型都告诉客户端，而且要正确的告诉
  - 不同的资源对应的 Content-Type 是不一样，具体参照：http://tool.oschina.net/commons
  - 对于文本类型的数据，最好都加上编码，目的是为了防止中文解析乱码问题
- 通过网络发送文件
  - 发送的并不是文件，本质上来讲发送是文件的内容
  - 当浏览器收到服务器响应内容之后，就会根据你的 Content-Type 进行对应的解析处理



#### 4. 创建node.js 应用

```javascript
var http = require('http');

http.createServer(function (request, response) {

	// 发送 HTTP 头部 
	// HTTP 状态值: 200 : OK
	// 内容类型: text/plain
	response.writeHead(200, {'Content-Type': 'text/plain'});

	// 发送响应数据 "Hello World"
	response.end('Hello World\n');
}).listen(3000);

// 终端打印如下信息
console.log('Server running at http://127.0.0.1:3000/');
```

命令行输入:

```javascript
node server.js
Server running at http://127.0.0.1:3000/
```



#### 5. 安装npm

##### NPM 使用介绍

- NPM建立了一个NodeJS生态圈，NodeJS开发者和用户可以在里边互通有无

NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：

- 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
- 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

- 测试安装是否成功

  npm	-v

  出现版本号标识安装成功

- 旧版本升级:

  npm install npm -g

##### 全局安装与本地安装

npm 的包安装分为本地安装（local）、全局安装（global）两种，从敲的命令行来看，差别只是有没有-g而已，比如

```
npm install express          # 本地安装
npm install express -g   # 全局安装
```

如果出现以下错误：

```
npm err! Error: connect ECONNREFUSED 127.0.0.1:8087 
```

解决办法为：

```
$ npm config set proxy null
```

##### 本地安装

- 1. 将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。
- 2. 可以通过 require() 来引入本地安装的包。

##### 全局安装

- 1. 将安装包放在 /usr/local 下或者你 node 的安装目录。
- 2. 可以直接在命令行里使用。

如果你希望具备两者功能，则需要在两个地方安装它或使用 npm link。

#### 6. 下载第三方包

我们可以使用以下命令来下载第三方包。

```
$ npm install argv
...
argv@0.0.2 node_modules\argv

```

下载好之后，argv包就放在了工程目录下的node_modules目录中，因此在代码中只需要通过require('argv')的方式就好，无需指定第三方包路径。

以上命令默认下载最新版第三方包，如果想要下载指定版本的话，可以在包名后边加上@<version>，例如通过以下命令可下载0.0.1版的argv。

```
$ npm install argv@0.0.1
...
argv@0.0.1 node_modules\argv

```

NPM对package.json的字段做了扩展，允许在其中申明第三方包依赖。因此，上边例子中的package.json可以改写如下：

```
{
    "name": "node-echo",
    "main": "./lib/echo.js",
    "dependencies": {
        "argv": "0.0.2"
    }
}

```

这样处理后，在工程目录下就可以使用npm install命令批量安装第三方包了。

更重要的是，当以后node-echo也上传到了NPM服务器，别人下载这个包时，NPM会根据包中申明的第三方包依赖自动下载进一步依赖的第三方包。

例如，使用npm install node-echo命令时，NPM会自动创建以下目录结构。

```
- project/
    - node_modules/
        - node-echo/
            - node_modules/
                + argv/
            ...
    ...

```

如此一来，用户只需关心自己直接使用的第三方包，不需要自己去解决所有包的依赖关系。

##### 安装命令行程序

从NPM服务上下载安装一个命令行程序的方法与第三方包类似。

例如上例中的node-echo提供了命令行使用方式，只要node-echo自己配置好了相关的package.json字段，对于用户而言，只需要使用以下命令安装程序。

```
$ npm install node-echo -g

```

参数中的-g表示全局安装，因此node-echo会默认安装到以下位置，并且NPM会自动创建好Linux系统下需要的软链文件或Windows系统下需要的.cmd文件。

```
- /usr/local/               # Linux系统下
    - lib/node_modules/
        + node-echo/
        ...
    - bin/
        node-echo
        ...
    ...

- %APPDATA%\npm\            # Windows系统下
    - node_modules\
        + node-echo\
        ...
    node-echo.cmd
    ...

```

##### 发布代码

第一次使用NPM发布代码前需要注册一个账号。终端下运行npm adduser，之后按照提示做即可。

账号注册完成后，接着我们需要编辑package.json文件，加入NPM必需的字段。接着上边node-echo的例子，package.json里必要的字段如下。

```
{
    "name": "node-echo",           # 包名，在NPM服务器上须要保持唯一
    "version": "1.0.0",            # 当前版本号
    "dependencies": {              # 第三方包依赖，需要指定包名和版本号
        "argv": "0.0.2"
      },
    "main": "./lib/echo.js",       # 入口模块位置
    "bin" : {
        "node-echo": "./bin/node-echo"      # 命令行程序名和主模块位置
    }
}

```

之后，我们就可以在package.json所在目录下运行npm publish发布代码了。

##### 版本号

使用NPM下载和发布代码时都会接触到版本号。NPM使用语义版本号来管理代码，这里简单介绍一下。

语义版本号分为X.Y.Z三位，分别代表主版本号、次版本号和补丁版本号。当代码变更时，版本号按以下原则更新。

- 如果只是修复bug，需要更新Z位。
- 如果是新增了功能，但是向下兼容，需要更新Y位。
- 如果有大变动，向下不兼容，需要更新X位。

版本号有了这个保证后，在申明第三方包依赖时，除了可依赖于一个固定版本号外，还可依赖于某个范围的版本号。例如"argv": "0.0.x"表示依赖于0.0.x系列的最新版argv。

NPM支持的所有版本号范围指定方式可以查看[官方文档](https://npmjs.org/doc/files/package.json.html#dependencies)。

##### NPM常用命令

除了本章介绍的部分外，NPM还提供了很多功能，package.json里也有很多其它有用的字段。

除了可以在[npmjs.org/doc/](https://npmjs.org/doc/)查看官方文档外，这里再介绍一些NPM常用命令。

NPM提供了很多命令，例如install和publish，使用npm help可查看所有命令。

- NPM提供了很多命令，例如`install`和`publish`，使用`npm help`可查看所有命令。
- 使用`npm help <command>`可查看某条命令的详细帮助，例如`npm help install`。
- 在`package.json`所在目录下使用`npm install . -g`可先在本地安装当前命令行程序，可用于发布前的本地测试。
- 使用`npm update <package>`可以把当前目录下`node_modules`子目录里边的对应模块更新至最新版本。
- 使用`npm update <package> -g`可以把全局安装的对应命令行程序更新至最新版。
- 使用`npm cache clear`可以清空NPM本地缓存，用于对付使用相同版本号发布新版本代码的人。
- 使用`npm unpublish <package>@<version>`可以撤销发布自己发布过的某个版本代码。

#### 7. fs.writeFile

```javascript
//第一个参数：文件路径
//第二个参数：文件内容
//第三个参数：回调函数
fs.writeFile('./data/你好.md', '大家好，给大家介绍一下，我是Node.js', function (error) {
  // console.log('文件写入成功')
  // console.log(error)
  if (error) {
//  成功：
//      文件写入成功
//      error 是 null
//  失败：
//      文件写入失败
//      error 就是错误对象
    console.log('写入失败')
  } else {
    console.log('写入成功了')
  }
})
```

#### 8.Node.js REPL(交互式解释器)

Node.js REPL(Read Eval Print Loop:交互式解释器) 表示一个电脑的环境，类似 Window 系统的终端或 Unix/Linux shell，我们可以在终端中输入命令，并接收系统的响应。

Node 自带了交互式解释器，可以执行以下任务：

- **读取** - 读取用户输入，解析输入了Javascript 数据结构并存储在内存中。
- **执行** - 执行输入的数据结构
- **打印** - 输出结果
- **循环** - 循环操作以上步骤直到用户两次按下 **ctrl-c** 按钮退出。

Node 的交互式解释器可以很好的调试 Javascript 代码。

开始学习 REPL

我们可以输入以下命令来启动 Node 的终端：

```
$ node
> 

```

这时我们就可以在 > 后输入简单的表达式，并按下回车键来计算结果。

##### 简单的表达式运算

接下来让我们在 Node.js REPL 的命令行窗口中执行简单的数学运算：

```
$ node
> 1 +4
5
> 5 / 2
2.5
> 3 * 6
18
> 4 - 1
3
> 1 + ( 2 * 3 ) - 4
3
>

```

##### 使用变量

你可以将数据存储在变量中，并在你需要的使用它。

变量声明需要使用 **var** 关键字，如果没有使用 var 关键字变量会直接打印出来。

使用 **var** 关键字的变量可以使用 console.log() 来输出变量。

```
$ node
> x = 10
10
> var y = 10
undefined
> x + y
20
> console.log("Hello World")
Hello World
undefined
> console.log("www.w3cschool.cn")
www.w3cschool.cn
undefined

```

##### 多行表达式

Node REPL 支持输入多行表达式，这就有点类似 JavaScript。接下来让我们来执行一个 do-while 循环：

```
$ node
> var x = 0
undefined
> do {
... x++;
... console.log("x: " + x);
... } while ( x < 5 );
x: 1
x: 2
x: 3
x: 4
x: 5
undefined
>

```

**...** 三个点的符号是系统自动生成的，你回车换行后即可。Node 会自动检测是否为连续的表达式。

##### 下划线(_)变量

你可以使用下划线(_)获取表达式的运算结果：

```
$ node
> var x = 10
undefined
> var y = 20
undefined
> x + y
30
> var sum = _
undefined
> console.log(sum)
30
undefined
>

```

##### REPL 命令

- **ctrl + c** - 退出当前终端。
- **ctrl + c 按下两次** - 退出 Node REPL。
- **ctrl + d** - 退出 Node REPL.
- **向上/向下 键** - 查看输入的历史命令
- **tab 键** - 列出当前命令
- **.help** - 列出使用命令
- **.break** - 退出多行表达式
- **.clear** - 退出多行表达式
- **.save filename** - 保存当前的 Node REPL 会话到指定文件
- **.load filename** - 载入当前 Node REPL 会话的文件内容。

##### 停止 REPL

前面我们已经提到按下两次 **ctrl + c** 建就能退出 REPL:

```
$ node
>
(^C again to quit)
>
```

#### 9. EcmaScript

##### 核心模块

Node 为 JavaScript 提供了很多服务器级别的 API ，这些 API 绝大多数都被包装到了一个具名的核心模块中了。

例如文件操作的 `fs` 核心模块，http 服务构建的 `http` 模块，`path` 路径操作模块、`os` 操作系统信息模块。。。。

只要这个模块是一个核心模块，马上想到如果想要使用它，就必须先使用 `require` 方法加载才能使用：

```javascript
var fs = require('fs')
var http = require('http')
```

#### 10. Node.js 回调函数

Node.js 异步编程的直接体现就是回调。

# 未完待续...

