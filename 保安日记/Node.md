![image](assets/v2-d5a78cd0626ff0acfe3f0d3a65828108_r-20220727205227-2gkknpd.jpg)

# Node

[Node.js](https://nodejs.dev/en/) 是一个基于 `Chrome V8引擎` 的 `JavaScript` 运行时环境，`Node.js` 可以运行 `JS`文件

# Node模块

## Node模块

### global模块

类似于 `window` 对象，`global模块`是`node` 中的全局模块，除了`global模块`（全局模块）中的内容可以直接使用，其他模块都是需要加载

```js
//window 浏览器中的全局对象
//global nodejs中的全局对象,node里面使用 global里面的变量,不需要引入

//当前执行的文件所在的 文件夹 的绝对路径
console.log(__dirname)// C:\Users\hulinghao\Desktop\xxx\Node.js - day01\04-源代码
//当前执行的 文件 的绝对路径
console.log(__filename)// C:\Users\hulinghao\Desktop\xxx\Node.js - day01\04-源代码\global模块测试.js
```

### fs模块

* 概念：fs 模块不是全局的，不能直接使用。因此需要导入才能使用。

* 读取文件：`fs.readFileSync('文件路径', '编码格式')`

  ```js
  // 1. 导入fs内置模块。类似于 script src="xxx.js"
  const fs = require('fs')  // console.log('fs:', fs)

  // 2. 调用提供的方法
  // 不设置编码格式,返回的是Buffer对象(js中表示进制数据的格式)
  const res = fs.readFileSync('./info/a.txt')
  console.log('res:', res)

  // 设置了编码格式之后,会返回对应编码的内容
  const res = fs.readFileSync('./info/a.txt', 'utf-8')
  console.log('res:', res)
  ```
* 关于 Buffer 对象

  ```js
  Buffer对象是Nodejs用于处理二进制数据的。
  其实任意的数据在计算机底层都是二进制数据，因为计算机只认识二进制。
  所以读取任意的文件，返回的结果都是二进制数据，即Buffer对象。
  Buffer对象可以调用toString()方法转换成字符串。
  ```
* 写文件：`fs.writeFileSync('路径', 内容)`。覆盖写入，返回值是`undefined`，文件不存在会创建文件，文件夹不存在会报错。

  ```js
  // 1. 导入fs内置模块
  const fs = require('fs')

  const data = `
  咏鸭-某黑鸭
  嘎嘎嘎
  曲项向天歌
  黄毛浮黑水
  黄掌拨红油
  `
  try {
    const res = fs.writeFileSync('./msg/b.txt', data)
    console.log('res:', res)
  } catch (error) {
    console.log('写入失败')
  }
  ```

### path模块

Node.js 提供的处理路径的模块 `path`，提供了很多处理路径的方法和属性，也是一个内置模块

```js
// 1. 导入path
const path = require('path')

// 2. __dirname(文件夹) __filename(文件)
const res3 = path.join(__dirname, './score/总成绩.txt')
console.log('res3:', res3)
```

### http模块

`http`也是内置模块, `Node.js` 官方提供的、用来创建web 服务器的模块。它提供了一系列的方法和属性,让开发者(咱们)可以用代码来创建服务器,接收**请求**及响应**内容**

* 创建最基本web服务器

  ```js
  // 1. 导入 http 模块
  const http = require('http')

  // 2. 创建 web 服务器实例
  const server = http.createServer()

  // 3. 启动服务器
  // 3000 端口号
  server.listen(4399, () => {
    console.log('my server start work')
  })

  // 4. 为服务器实例绑定 request 事件，监听客户端的请求,当客户端发送请求到服务器的时候，会触发这个事件
  server.on('request', (request, response) => {  // request ,response 形参。名字可以改,如非必要不建议改
    // console.log('request:', request)
    console.log('request.method:', request.method)
    console.log('request.url:', request.url)
    // console.log('response:', response)
    // 这里要处理客户端的请求
    console.log('hello html')
    // response.end('（￣︶￣）↗　')
    // 设置响应头
    response.setHeader('info', 'good good study daydayup')
    // 设置状态码
    response.statusCode = 404
    // 响应内容给客户端
    response.end('nice to meet you')
  })
  ```
* `request(请求)对象`常见属性

  ```js
  headers:所有的请求头信息
  method:请求的方式
  url:请求的地址
  ```
* `response(响应)对象`常见属性和方法

  ```js
  res.write(data);  给浏览器发送响应体，可以调用多次，从而提供连续的请求体
  res.end();   通知浏览器，所有响应头和响应主体都已被发送，即服务器将其视为已完成。
  res.end(data); 结束请求，并且响应一段内容，相当于res.write(data) + res.end()
  res.statusCode: 响应的的状态码 200 404 500
  res.statusMessage: 响应的状态信息， OK Not Found ,会根据statusCode自动设置。
  res.setHeader(name, value); 设置响应头信息， 比如content-type
  res.writeHead(statusCode, statusMessage, options); 设置响应头，同时可以设置状态码和状态信息。
  ```
* 中文乱码问题：当调用 `res.end()` 方法，向客户端发送中文内容时，会出现乱码问题，需要手动设置内容编码格式

  ```js
  //setHeader('Content-Type','text/plain; charset=utf-8');设置内容的格式,让浏览器知道如何解析
  // 普通文本
  res.setHeader('Content-Type', 'text/plain; charset=utf-8');
  // html
  res.setHeader('Content-Type', 'text/html; charset=utf-8');
  ```
* 根据`url`响应不同的`html内容`：服务器返回的内容由服务器说了算

  ```js
  // 1. 导入 http 模块
  const http = require('http')

  // 2. 创建 web 服务器实例
  const server = http.createServer()

  // 3. 启动服务器
  // 3000 端口号
  server.listen(3000, () => {
    console.log('my server start work')
  })

  // 4. 为服务器实例绑定 request 事件，监听客户端的请求
  server.on('request', (request, response) => {
    // 设置响应头 响应的内容 html标签编码格式为 utf-8
    response.setHeader('Content-Type', 'text/html; charset=utf-8')
    if (request.url === '/' || request.url === '/index.html') {
      response.end(' <h1> 我是首页</h1>')
    } else if (request.url === '/about.html') {
      response.end('<h1> 我是about页面</h1>')
    } else {
      response.statusCode = 404
      response.end('<h1> 404 not found</h1>')
    }
  })
  ```

<br />

### 全局模块

* 全局模块指令：全局安装的模块，在系统盘（C 盘）,通过命令 `npm root -g` 可以查看全局安装路径

  ```js
  //安装
  npm i 模块名 -g
  npm i -g 模块名
  //卸载
  npm un 模块名 -g
  # 或者
  npm uninstall 模块名 -g
  ```

* `nrm` ：是作用是切换镜像源

  ```js
  npm i -g nrm  //全局安装nrm
  nrm --help   # 查看帮助
  nrm ls    # 查看全部可用的镜像源
  nrm test # 测试各个源的速度
  nrm use taobao  # 切换到淘宝镜像
  nrm use npm  # 切换到npm主站
  ```
* ​`serve`​：功能和自己编写的 web 服务器类似，通过 `serve`​ 托管并测试访问

## 模块化规范

### AMD

```js
- AMD规范采用异步方式加载模块，模块的加载不影响它后面语句的运行。
- 聊到AMD主要指的是通过require.js实现的模块化
- 项目中看到类似代码说明用的是AMD规范

// 执行基本操作
require(["jquery","underscore"],function($,_){
  // some code here
});
```

### CMD

```js
- CMD是另一种js模块化方案，它与AMD很类似。
- 不同点在于：AMD 推崇依赖前置、提前执行，CMD推崇依赖就近、延迟执行。
- 项目中看到类似代码说明用的是CMD规范

// 执行基本操作
define(function(require, exports, module) {
    var a = require('./a'); //在需要时申明
    a.doSomething();
    if (false) {
        var b = require('./b');
        b.doSomething();
    }
});
```

### CommonJS

```js
Node.js是CommonJS规范的主要实践
```

### EMS

```js
- 也叫做ES6 Module
- ES6 在语言标准的层面上，实现了模块功能，逐步会成为浏览器和服务器通用的模块解决方案
```
