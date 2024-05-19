![image](assets/v2-d5a78cd0626ff0acfe3f0d3a65828108_r-20220727205227-2gkknpd.jpg)

[Node.js ](http://nodejs.cn/api/)是一个基于 `Chrome V8引擎`​​ 的 `JavaScript`​​ 运行时环境，`Node.js`​​ 可以运行 `JS`​​文件。

​`Node.js`​ 使用了一个**事件驱动**、**非阻塞式 I/O**的模型，使其轻量又高效。

​`Node.js `​不是一门语言，也不是 `JavaScript `​的框架，也不是像`Nginx`​一样的`Web`​服务器 。

​`Node.js`​ 是 `JavaScript `​在服务器端的运行环境（平台）。

​`Node.js `​的组成分为：

* ​**ECMAScript**​。`ECMAScript`​ 的所有语法在 Node 环境中都可以使用。
* Node 环境提供的一些附加 API(包括文件、网络等相关的 API)。

​![bab95704bf2f5dd8e05236c74a98d487fafa6d41](assets/bab95704bf2f5dd8e05236c74a98d487fafa6d41-20231021171604-6o4fin8.png)​

# 基本概念

## 服务端渲染

* 客户端渲染（`CSR / Client side render`​）：

  * 前端通过一大堆接口请求数据，然后通过 `JS`​ 动态处理和生成页面结构和展示。
  * 优点是**前后端分离**、减小服务器压力、局部刷新。
  * 缺点是不利于 SEO（如果你的页面然后通过 `Ajax `​异步获取内容，抓取工具并不会等待异步完成后再行抓取页面内容）、首屏渲染慢。
* **服务端渲染（SSR / Server Side Render）：**

  * 服务器返回的不是接口数据，而是一整个页面（或整个楼层）的 HTML 字符串，浏览器直接显示即可。也就是说，在服务器端直接就渲染好了，然后一次性打包返回给前端。
  * 优点是有利于 SEO、首屏渲染很快。**总结： 搜索引擎优化 + 首屏速度优化 = 服务端渲染**

## 网页加载

网页资源的加载都是循序渐进的，首先获取 `HTML `​的内容， 然后解析 `HTML `​在发送其他资源的请求，如 `CSS`​，`Javascript`​，图片等。

会解析 引入 文件的路径再去发起资源请求

​![01b27104af8ecfe88afc6f70450cc04682d95f31](assets/01b27104af8ecfe88afc6f70450cc04682d95f31-20231014214443-8mzek1v.jpg)​

## 请求路径

网页中的 URL 主要分为两大类：`相对路径`​​ 与 `绝对路径`​​

开发中，相对路径是相对代码执行的文件为参照

在网页中，相对路径是以请求发起的时候，网页的`URL`​为参照，要留意页面的协议，主机地址，端口号改变导致的路径变动问题

* 绝对路径：可靠性强，而且相对容易理解，在项目中运用较多

  ```js
  http://atguigu.com/web       直接向目标资源发送请求，容易理解。网站的外链会用到此形式   
  //atguigu.com/web            与页面 URL 的协议拼接形成完整 URL 再发送请求。大型网站用的比较多 
  /web                         与页面 URL 的协议、主机名、端口拼接形成完整 URL 再发送请求。
  ```
* 相对路径：在发送请求时，需要与当前页面 URL 路径进行 `计算`​ ，得到完整 URL 后，再发送请求。

  ```js
  此时的网页URL http://www.xxx.com/course
  和当前URL进行拼接
  ./css/app.css       http://www.xxx.com/course/css/app.css
  js/app.js           http://www.xxx.com/course/js/app.js
  ../img/logo.png     http://www.xxx.com/img/logo.png
  ../../mp4/show.mp4  http://www.xxx.com/mp4/show.mp4
  ```

## nodemon

`nodemon`​可以帮我们实时监听项目中代码的变化，并且自动重启服务，而且配置简单。

* 安装：`npm install -g nodemon`​，如果无法使用nodemon，那么要去环境变量中进行配置
* 使用：`nodemon`​运行项目，取代之前的`node app.js`​。如 `nodemon  app.js`​

项目运行之后，`nodemon`​会自动监听代码的改动，并且重新启动服务，大大增加我们开发效率。

* 参见配置

  ```js
  在命令行指定应用的端口号：nodemon ./server.js localhost 8080
  查看帮助，帮助里面有很多选项都是一目了然：nodemon -h 或者 nodemon --help
  运行 debug 模式：nodemon --debug ./server.js 80
  手动重启项目： Nodemon 命令运行的终端 窗口中输入 rs 两个字符，然后再按下回车键，就能手动重启 Nodemon了。
  ```

## __dirname

​`__dirname`​​ 保存着 **当前文件夹所在目录的绝对路径**，可以使用 `__dirname`​​ 与文件名拼接成绝对路径

使用 `fs `​模块的时候，尽量使用 `__dirname`​​ 路径转换为绝对路径，这样可以避免相对路径产生的 `Bug`​​

```js
console.log(__dirname); //  /Users/smyhvae/dselegent
```

## __filename

这是一个常量。表示：当前执行文件的完整目录 + 文件名

```js
console.log(__filename); //  /Users/smyhvae/dselegent/app.js
```

## mime

​`媒体类型`​​（通常称为 `Multipurpose Internet Mail Extensions `​或 `MIME `​类型 ）是一种标准，用来表示文档、文件或字节流的性质和格式。

​`HTTP `​服务可以设置响应头` Content-Type`​ 来表明响应体的 `MIME `​类型，浏览器会根据该类型决定如何处理资源

对于未知的资源类型，可以选择 `application/octet-stream`​ 类型，浏览器在遇到该类型的响应时，会对响应体内容进行独立存储，也就是我们常见的 `下载`​ 效果

```js
mime 类型结构： [type]/[subType]
例如： text/html text/css image/jpeg image/png application/json

常见文件对应的 mime 类型
html: 'text/html',
css:  'text/css',
js:   'text/javascript',
png:  'image/png',
jpg:  'image/jpeg', 
gif:  'image/gif',
mp4:  'video/mp4',
mp3:  'audio/mpeg',
json: 'application/json'
```

## Buffer

### 概念

​`Buffer`​是一个类似于数组的 `对象`​ ，用于表示固定长度的字节序列

​`Buffer`​本质是一段内存空间，专门用来处理 `二进制数据`​

特点：

* ​`Buffer`​大小固定且无法调整
* ​`Buffer`​性能较好，可以直接对计算机内存进行操作
* 每个元素的大小为 1 字节（`byte`​）

### 基本使用

* 创建

  ```js
  // 创建了一个长度为 10 字节的 Buffer，相当于申请了 10 字节的内存空间，每个字节的值为 0
  let buf_1 = Buffer.alloc(10) //=>结果为<Buffer 00 00 00 00 00 00 00 00 00 00>

  // 创建了一个长度为 10 字节的 Buffer，buffer 中可能存在旧数据，可能会影响执行结果，所以叫 unsafe ，但是效率比 alloc 高
  let buf_2 = Buffer.allocUnsafe(10)

  // 通过字符串创建 Buffer
  let buf_3 = Buffer.from('hello')

  // 通过数组创建 Buffer
  let buf_4 = Buffer.from([105, 108, 111, 118, 101, 121, 111, 117])
  ```
* ​`Buffer`​和字符串转化：使用​`toString`​ 方法将 Buffer 转为字符串

  ```js
  et buf_4 = Buffer.from([105, 108, 111, 118, 101, 121, 111, 117])
  console.log(buf_4.toString()) //=>iloveyou
  ```
* ​`Buffer`​读写

  ```js
  let buf_3 = Buffer.from('hello')
  // 读取
  console.log(buf_3[1]) //=>101
  // 修改
  buf_3[1] = 97
  //查看字符串结果
  console.log(buf_3.toString()) //=>hallo
  ```

## 关于数据库

### 数据库种类

不开源而且付费的商用数据库

* ​`Oracle`​：典型的高富帅；
* ​`SQL Server`​：微软自家产品，Windows定制专款；
* ​`DB2`​：IBM的产品，听起来挺高端；
* ​`Sybase`​：曾经跟微软是好基友，后来关系破裂，现在家境惨淡。

开源免费的数据库

* ​`MySQL`​，大家都在用，一般错不了；
* ​`PostgreSQL`​，学术气息有点重，其实挺不错，但知名度没有`MySQL`​高；
* ​`sqlite`​，嵌入式数据库，适合桌面和移动应用。

## 关系型与非关系数据库区别

关系型和非关系型数据库的主要差异是**数据存储**的方式。

* 关系式数据库  
  关系型数据天然就是表格式的，因此存储在数据表的行和列中。数据表可以彼此关联协作存储，也很容易提取数据。  
  与其相反，非关系型数据不适合存储在数据表的行和列中，而是大块组合在一起。非关系型数据通常存储在数据集中，就像文档、键值对或者图结构。你的数据及其特性是选择数据存储和提取方式的首要影响因素。  
  关系型数据库最典型的数据结构是表，由二维表及其之间的联系所组成的一个数据组织  
  优点：  
  1、易于维护：都是使用表结构，格式一致；  
  2、使用方便：SQL语言通用，可用于复杂查询；  
  3、复杂操作：支持SQL，可用于一个表以及多个表之间非常复杂的查询。  
  缺点：  
  1、读写性能比较差，尤其是海量数据的高效率读写；  
  2、固定的表结构，灵活度稍欠；  
  3、高并发读写需求，传统关系型数据库来说，硬盘I/O是一个很大的瓶颈。
* 非关系型数据库  
  非关系型数据库严格上不是一种数据库，应该是一种数据结构化存储方法的集合，可以是文档或者键值对等。  
  优点：  
  1、格式灵活：存储数据的格式可以是key,value形式、文档形式、图片形式等等，文档形式、图片形式等等，使用灵活，应用场景广泛，而关系型数据库则只支持基础类型。  
  2、速度快：nosql可以使用硬盘或者随机存储器作为载体，而关系型数据库只能使用硬盘；  
  3、高扩展性；  
  4、成本低：nosql数据库部署简单，基本都是开源软件。  
  缺点：  
  1、不提供sql支持；  
  2、无事务处理；  
  3、数据结构相对复杂，复杂查询方面稍欠。

‍

## [会话控制](https://juejin.cn/post/6844904034181070861)

​`HTTP`​是一种无状态的协议，它没有办法区分多次的请求是否来自于同一个客户端，无法区分用户

### cookie

​`cookie `​是 `HTTP `​服务器发送到用户浏览器并保存在本地的一小块数据

​`cookie `​是按照域名划分保存的

浏览器向服务器发送请求时，会自动将 `当前域名下`​ 可用的 cookie 设置在请求头中，然后传递给服务器

浏览器限制单个 `cookie`​ 保存的数据不能超过 `4K`​ ，且单个域名下的存储数量也有限制（168？）

​![image](assets/image-20231021202545-xotzynl.png)​

* 在 `express `​中使用

  ```js
  const express = require("express");
  const cookieParser = require("cookie-parser");//安装 cookie-parser  npm i cookie-parser
  const app = express();
  app.use(cookieParser());//设置 cookieParser 中间件

  // 设置cookie
  app.get("/set-cookie", (request, response) => {
    // 不带时效性    会在浏览器关闭的时候，销毁
    response.cookie("username", "wangwu");
    // 带时效性
    response.cookie("email", "23123456@qq.com", { maxAge: 5 * 60 * 1000 });
    //响应
    response.send("Cookie的设置");
  });

  // 读取cookie
  app.get("/get-cookie", (request, response) => {
    //读取 cookie
    console.log(request.cookies);
    //响应体
    response.send("Cookie的读取");
  });

  //删除cookie
  app.get("/delete-cookie", (request, response) => {
    response.clearCookie("username");//删除
    response.send("cookie 的清除");
  });

  //启动服务
  app.listen(3000, () => {
    console.log("服务已经启动....");
  });
  ```

### session

​`session `​是保存在服务器端的一块儿数据，保存当前访问用户的相关信息，可以识别用户的身份，快速获取当前用户的相关信息。

填写账号和密码校验身份，校验通过后创建`session 信息`​，然后将 `session_id`​ 的值通过响应头返回给浏览器

有了cookie，下次发送请求时会自动携带cookie，服务器通过 `cookie`​ ** ** 中的 `session_id`​ ** ** 的值来使用

```js
const express = require("express");
const session = require("express-session"); //安装包 npm i express-session connect-mongo
const MongoStore = require("connect-mongo");
const app = express();

//设置 session 的中间件
app.use(
  session({
    name: "sid", //设置cookie的name，默认值是：connect.sid
    secret: "atguigu", //参与加密的字符串（又称签名）  加盐
    saveUninitialized: false, //是否为每次请求都设置一个cookie用来存储session的id
    resave: true, //是否在每次请求时重新保存session
    store: MongoStore.create({
      mongoUrl: "mongodb://127.0.0.1:27017/project", //数据库的连接配置
    }),
    cookie: {
      httpOnly: true, // 开启后前端无法通过 JS 操作
      maxAge: 1000 * 300, // 这一条 是控制 sessionID 的过期时间的！！！
    },
  })
);

//创建 session  session的设置
app.get("/login", (req, res) => {
  //设置session
  req.session.username = "zhangsan";
  req.session.email = "zhangsan@qq.com";
  res.send("登录成功");
});

//获取 session
app.get("/home", (req, res) => {
  console.log("session的信息", req.session.username);
  // 检测 session 是否存在用户数据
  if (req.session.username) {
    res.send(`你好 ${req.session.username}`);
  } else {
    res.send("登录 注册");
  }
});

//销毁 session
app.get("/logout", (req, res) => {
  req.session.destroy(() => {
    res.send("成功退出");
  });
});
app.listen(3000, () => {
  console.log("服务已经启动, 端口 " + 3000 + " 监听中...");
});
```

### token

​`token`​ 是服务端生成并返回给 HTTP 客户端的一串加密字符串， `token`​ 中保存着`用户信息`​

​`token`​不属于 `http `​标准，完全由前后端协商而定，但 `cookie `​属于 `http `​标准

填写账号和密码校验身份，校验通过后响应 `token`​，`token `​一般是在响应体中返回给客户端的

后续发送请求时，需要`手动`​将 token 添加在请求报文中(`cookie`​是自动携带的)，一般是放在请求头中

服务端压力更小，数据存储在客户端

相对更安全，数据加密，可以避免 CSRF（跨站请求伪造）

扩展性更强，服务间可以共享，增加服务节点更简单

### JWT

​`JWT（JSON Web Token ）`​是目前最流行的跨域认证解决方案，可用于基于 `token`​ 的身份验证

​`JWT`​使 `token`​的生成与校验更规范

```js
//导入 jsonwebtokan
const jwt = require("jsonwebtoken");
// 创建 token jwt.sign(数据, 加密字符串, 配置对象)
let token = jwt.sign({username: "zhangsan",},"atguigu",{expiresIn: 60, //单位是 秒});

// 解析 jwt.verify(token,加密字符串，回调函数)
jwt.verify(token, "atguigu", (err, data) => {
  if (err) {
    console.log("校验失败~~");
    return;
  }
  console.log(data); // { username: '张三', iat: (创建时间), exp:(过期时间)}
});
```

# 内置对象

## fs

### writeFile

语法：`fs.writeFile(file, data[, options], callback)`

```js
- file 文件名
- data 待写入的数据
- options 选项设置(可选)
- callback 写入回调
返回值： `undefined`

// require 是 Node.js 环境中的 '全局' 变量，用来导入模块
const fs = require('fs')

// 将 [三人行，必有我师焉。] 写入到当前文件夹下的 [座右铭.txt] 文件中
fs.writeFile('./座右铭.txt', '三人行，必有我师焉。', err =>{
    // 如果写入失败，则回调函数调用时，会传入错误对象，如写入成功，会传入 null
    if(err){
        console.log(err)
        return
    }
    console.log('写入成功')  
})
```

### writeFileSync

语法：`fs.writeFileSync(file, data[, options])`

```js
try{
    fs.writeFileSync('./座右铭.txt', '三人行，必有我师焉。')
}catch(e){
    console.log(e)
}
```

### appendFile 、 appendFileSync

* 概念：​`appendFile`​ 作用是在文件尾部追加内容，`appendFile`​ 语法与 `writeFile`​ 语法完全相同
* ​`fs.appendFile(file, data[, options], callback)`​、`fs.appendFileSync(file, data[, options])`​

  ```js
  fs.append('./座右铭.txt', '则其善者而从之，其不善者而改之。', err =>{
      if(err) throw err
      console.log('追加成功')
  })

  fs.appendFileSync('./座右铭.txt','\r\n温故而知新，可以为师矣')
  ```

### readFile

**语法**: `fs.readFile(path[, options], callback)`​​ 

```js
// 导入 fs 模块
fs.readFile('./座右铭.txt', (error,data) =>{
    if(err) throw err
    console.log(data)
})

fs.readFile('./座右铭.txt', 'uft-8', (error,data) =>{
    if(err) throw err
    console.log(data)
})
```

### readFileSync

语法：`fs.readFileSync(path[, options])`​

```js
返回值 ：string | Buffer
let data = fs.readFileSync('./座右铭.txt')
let data = fs.readFileSync('./座右铭.txt', 'utf-8')
```

### createWriteStream

语法：`fs.createWriteStream(path[, options])`​

程序打开一个文件是需要消耗资源的，流式写入可以减少打开关闭文件的次数。**流式写入方式适用于** ****大文件写入或者频繁写入****​**的场景，**​`writeFile`​适合于 **写入频率较低的场景**

```js
let ws = fs.createWriteStream('./观书有感.txt')

//写入数据到流
ws.write('半亩方塘一鉴开\r\n')
ws.write('天光云影共徘徊\r\n')
ws.write('问渠那得清如许\r\n')
ws.write('为有源头活水来\r\n')

//关闭写入流，表明已没有数据要被写入可写流
ws.end()
```

### createReadStream

​​语法​​：`fs.createReadStream(path[, options])`​​

```js
const fs = require('fs')
const dataStream = fs.createReadStream('/文本.text')
dataStream.on('data',chunk=>{
  console.log(chunk.toString)//读取文件的内容
})
```

### rename、renameSync

​`fs.rename(oldPath, newPath, callback)`​

​`fs.renameSync(oldPath, newPath)`​

如果还是移动到当前路径，但是修改了名字，就是重命名了

```js
fs.rename("./观书有感.txt", "./论语/观书有感.txt", (err) => {
  if (err) throw err;
  console.log("移动完成");
});

fs.renameSync("./座右铭.txt", "./论语/.我的座右铭.txt");
```

### ​unlink​​、​unlinkSync​​

​`fs.unlink(path, callback)`​

​`fs.unlinkSync(path)`​

```js
const fs = require("fs");
fs.unlink("./test.txt", (err) => {
  if (err) throw err;
  console.log("删除成功");
});

fs.unlinkSync("./test2.txt");
// 调用 rm 方法(同步 rmSync)
fs.rm("./论语.txt", (err) => {
  if (err) {
    console.log("删除失败");
    return;
  }
  console.log("删除成功");
});
```

### mkdir、mkdirSync

​`fs.mkdir(path[, options], callback)`​

​`fs.mkdirSync(path[, options])`​

```js
// 异步创建文件夹  mk  make  制作   dir  directory  文件夹
fs.mkdir("./page", (err) => {
  if (err) throw err;
  console.log("创建成功");
});
// 递归异步创建
fs.mkdir("./1/2/3", { recursive: true }, (err) => {
  if (err) throw err;
  console.log("递归创建成功");
});
// 递归同步创建文件夹
fs.mkdirSync("./x/y/z", { recursive: true });
```

### readdir、readdirSync

​`fs.readdir(path[, options], callback)`​

​`fs.readdirSync(path[, options])`​

```js
// 异步读取
fs.readdir("./论语", (err, data) => {
  if (err) throw err;
  console.log(data);
});
// 同步读取
let data = fs.readdirSync("./论语");
console.log(data);
```

### ​rmdir​​、​​redirSync​​

​`fs.rmdir(path[, options], callback)`​

​`fs.redirSync(path[, options])`​

```js
// 异步删除文件夹  rm  remove 移除
fs.rmdir("./page", (err) => {
  if (err) throw err;
  console.log("删除成功");
});
//异步递归删除文件夹  不推荐
//=>DeprecationWarning: In future versions of Node.js, fs.rmdir(path, { recursive: true }) will be removed. Use fs.rm(path, { recursive: true }) instead
fs.rmdirSync("./1", { recursive: true }, (err) => {
  if (err) {
    console.log(err);
    return;
  }
  console.log("递归删除");
});
//同步递归删除文件夹
fs.rmdirSync("./x", { recursive: true });

// 建议使用
fs.rm("./a", { recursive: true }, (err) => {
  if (err) {
    console.log(err);
    return;
  }
  console.log("删除成功");
});
```

### ​**stat、statSync**

使用`stat`​ 或 `statSync`​ 来查看资源的详细信息

​`fs.stat(path[, options], callback)`​

​`fs.statSync(path[, options])`​

```js
// 异步获取状态
// stat  方法  status 缩写 状态
fs.stat('/data.txt', (err, data) =>{
  if(err) throw err
  console.log(data)
})
// 同步获取状态
let data = fs.statSync('./data.txt')

- size 文件体积
- birthtime 创建时间
- mtime 最后修改时间
- isFile 检测是否为文件
- isDirectory 检测是否为文件夹
```

​​​![3d5f0b54415a2949c04dcbc49a0452e7ec91899b](assets/3d5f0b54415a2949c04dcbc49a0452e7ec91899b-20231014202603-zee6sey.png)​

## path

### dirname

​`path.dirname(filepath)`​：获取文件路径

```js
var path = require('path');
var filepath = '/tmp/demo/js/test.js';
console.log( path.dirname(filepath));//  /tmp/demo/js
```

### basename

​`path.basename(filepath)`​：获取文件名称

```js
var path = require('path');
var filepath = '/tmp/demo/js/test.js';
console.log( path.basename(filepath));//  test
```

### extname

​`path.extname(filepath)`​：获取扩展名

```js
var path = require('path');
var filepath = '/tmp/demo/js/test.js';
console.log( path.dirname(extname));// 输出：.js
```

### **resolve**

​`path.resolve([...myPaths])`​：将路径或路径片段的序列解析为绝对路径，返回的路径是从右往左处理，后面的每个 `myPath `​​被依次解析，直到构造出一个完整的绝对路径。

```js
const path = require('path');
// 假设当前工作路径是 /Users/a/Documents/git-code/nodejs-learning-guide/examples/2016.11.08-node-path

console.log( path.resolve('') )// 输出 /Users/a/Documents/git-code/nodejs-learning-guide/examples/2016.11.08-node-path

console.log( path.resolve('/foo/bar', './baz') );// 输出 /foo/bar/baz

console.log( path.resolve('/foo/bar', '/tmp/file/') );// 输出 /tmp/file

console.log( path.resolve('www', 'js/upload', '../mod.js') );// 输出 /Users/a/Documents/git-code/nodejs-learning-guide/examples/2016.11.08-node-path/www/js/mod.js


注意：
如果 dirname 是以 ./ 、../、不加 / 开头的话，那么 resolve 会找到磁盘下的根目录
var dirname = '../User/Desktop';
var basename = 'abc.txt';
path.join(dirname, basename);  // ../User/Desktop/abc.txt
path.resolve(dirname, basename);  // /Users/Desktop/node/User/Desktop/abc.txt

如果 basename 是以 / 开头的，那么 resolve 就会直接返回 basename 
var dirname = '/User/Desktop';
var basename = '/abc.txt';
path.join(dirname, basename);  // /User/Desktop/abc.txt
path.resolve(dirname, basename);  // /abc.txt
```

### join

​`path.join([...paths]);`​：将路径进行拼接，没有特殊处理，可以理解成前后路径直接拼接

```js
const path = require('path');
const result1 = path.join(__dirname, './app.js');
console.log(result1); // 返回：/Users/smyhvae/dselegent/app.js
const result2 = path.join('/foo1', 'foo2', './foo3');
console.log(result2); // 返回：/foo1/foo2/foo3
const result3 = path.join('/foo1', 'foo2', '/foo3');
console.log(result3); // 返回：/foo1/foo2/foo3
```

### **sep**

获取操作系统的路径分隔符

```js
 // sep 获取路径分隔符
 console.log(path.sep) //=> window \  linux /
```

### **parse**

解析路径并返回文件对象

```js
 // 解析路径
 let str = 'D:\\Desktop\\Node\\code\\04-path\\01-path.js'
 console.log(path.parse(str))  //返回该路径下的文件对象
```

## http

​`http`​也是内置模块, `Node.js`​ 官方提供的、用来创建web 服务器的模块。

​`HTTP（hypertext transport protocol）`​协议；中文叫 超文本传输协议，

是一种基于`TCP/IP`​的应用层通信协议，这个协议详细规定了 `浏览器`​​ 和 万维网 `服务器`​​ 之间互相通信的规则

### 请求报文

用来向服务器发送数据，可以被称之为 ** 请求报文**

* 请求行

  ```js
  - 请求方法（get、post、put、delete等）
  - 请求 URL（统一资源定位器）
    例如：http://www.baidu.com/index.html?a=100&b=200#logo

    - http:          协议 (https、ftp、ssh等)
    - www.baidu.com  域名
    - 80             端口号
    - /index.html    路径
    - a=100&b=200    查询字符串
    - #logo          哈希 (锚点链接)
  ```
* 请求头： `头名：头值`​

  ```js
  Host：主机名
  Connection：连接的设置 keep-alive（保持连接）；close（关闭连接）
  Cache-Control：缓存控制 max-age = 0 （没有缓存）
  Upgrade-Insecure-Requests：将网页中的http请求转化为 https 请求（很少用）老网站升级
  User-Agent：用户代理，客户端字符串标识，服务器可以通过这个标识来识别这个请求来自哪个客户端 ，一般在PC端和手机端的区分
  Accept：设置浏览器接收的数据类型
  Accept-Encoding：设置接收的压缩方式
  Accept-Language：设置接收的语言 q=0.7 为喜好系数，满分为1
  Cookie：后端设置用于保存信息
  ```
* 空行
* 请求体：格式比较随意，可以是空，可以是字符串，可以是`JSON`​

### 响应报文

向客户端返回数据，可以被称之为 **响应报文**

* 响应行:  `HTTP/1.1 200 OK`​

  ```js
  - HTTP/1.1：HTTP协议版本号
  - 200：响应状态码 404 Not Found 500 Internal Server Error
    还有一些状态码，参考：https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status
    OK：响应状态描述
  ```
* 响应头

  ```js
  Cache-Control: 缓存控制 private 私有的，只允许客户端缓存数据
  Connection: 链接设置
  Content-Type:text/html;charset=utf-8: 设置响应体的数据类型以及字符集,响应体为html，字符集utf-8
  Content-Length: 响应体的长度，单位为字节
  ```
* 空行
* 响应体

  ```js
  响应体内容的类型是非常灵活的，常见的类型有 HTML、CSS、JS、图片、JSON
  ```

### 创建web服务

* 基本流程

  ```js
  const http = require('http')
  const server = http.createServer()
  server.listen(3000, () => {
    console.log('已监听到请求')
  })

  server.on('request', (request, response) => {  // request ,response 形参。名字可以改,如非必要不建议改
    console.log('request.method:', request.method)
    console.log('request.url:', request.url)
    // 这里要处理客户端的请求
    console.log('hello html')
    // 设置响应头
    response.setHeader('info', 'good good study daydayup')
    // 设置状态码
    response.statusCode = 404
    // 响应内容给客户端
    response.end('nice to meet you')
  })

  ```
* ​`response`​:常见属性和方法

  ```js
  res.write(data);  给浏览器发送响应体，可以调用多次，从而提供连续的请求体
  res.end();   通知浏览器，所有响应头和响应主体都已被发送，即服务器将其视为已完成。
  res.end(data); 结束请求，并且响应一段内容，相当于res.write(data) + res.end()
  res.statusCode: 响应的的状态码 200 404 500
  res.statusMessage: 响应的状态信息， OK Not Found ,会根据statusCode自动设置。
  res.setHeader(name, value); 设置响应头信息， 比如content-type
  res.writeHead(statusCode, statusMessage, options); 设置响应头，同时可以设置状态码和状态信息。
  ```
* 中文乱码问题：当调用 `res.end()`​ 方法，向客户端发送中文内容时，会出现乱码问题，需要手动设置内容编码格式

  ```js
  //setHeader('Content-Type','text/plain; charset=utf-8');设置内容的格式,让浏览器知道如何解析
  // 普通文本
  res.setHeader('Content-Type', 'text/plain; charset=utf-8');
  // html
  res.setHeader('Content-Type', 'text/html; charset=utf-8');
  ```

### 获取请求报文数据

|**含义**|**语法**|
| ----------------| ------|
|请求方法|​`request.method`​|
|请求版本|​`request.httpVersion`​|
|请求路径|​`request.url`​|
|URL 路径​|​`require('url').parse(request.url).pathname`​|
|URL 查询字符串|​`require('url').parse(request.url,  true).query`​|
|请求头|​`request.headers`​|
|请求体|​`request.on('data', function(chunk){}) request.on('end', function(){})`​|

* 基本使用

  ```js
  // 1. 导入 http 模块
  const http = require('http')
  // 2. 创建服务对象
  const server = http.createServer((request, response) => {
    // 获取请求的方法
    console.log(request.method)  //=>GET
    // 获取请求的 url
    console.log(request.url)  // 只包含 url 中的 路径 与查询字符串
    // 获取 http 协议的版本号
    console.log(request.httpVersion)  //=> 1.1
    // 获取 http 的请求头
    console.log(request.headers) //=>结果是一个对象
    response.end('http') //=>设置响应体
  })

  // 3. 监听端口，启动服务
  server.listen(9000, () => {
    console.log('服务已经启动...')
  })
  ```
* 注意

  ```js
  1. request.url 只能获取路径以及查询字符串，无法获取 URL 中的域名以及协议的内容
  2. request.headers 将请求信息转化成一个对象，并将属性名都转化成了『小写』
  3. 关于路径：如果访问网站的时候，只填写了 IP 地址或者是域名信息，此时请求的路径为『 `/` 』
  4. 关于 favicon.ico：这个请求是属于浏览器自动发送的请求
  ```
* 提取请求体

  ```js
  // 1. 导入 http 模块
  const http = require('http')

  // 2. 创建服务对象
  const server = http.createServer((request, response) => {
    // 1. 声明一个变量
    let body = ''
    // 2. 绑定 data 事件
    request.on('data', chunk => {
      body += chunk
    })
    // 3. 绑定 end 事件
    request.on('end', () => {
      console.log(body)  //=>'username=111&password=111'
      // 响应
      response.end('Hello Http') //=>设置响应体 
    })
  })

  // 3. 监听端口，启动服务
  server.listen(9000, () => {
    console.log('服务已经启动...')
  })
  ```
* 提取 `url `​路径 和查询字符串

  ```js
  1.使用 url 模块
  const http = require('http')
  const url = require('url')
  const server = http.createServer((request, response) => {
    // 2. 解析 request.url
    console.log(request.url)   //=>/search?keyword=h5
    // 使用 parse 解析 request.url 的内容
    // true 将 query 属性将会设置为一个 对象
    let res = url.parse(request.url, true)
    console.log(res)  // 如下图所示，为一个对象
    // 路径
    let pathname = res.pathname
    // 查询字符串
    let keyword = res.query.keyword
    console.log(keyword)   //=>h5
    response.end('url')
  })


  2.使用 URL 类，推荐
  const http = require('http')
  const server = http.createServer((request, response) => {
    // 实例化 url 对象
    // let url = new URL('/search?a=100&b=200','http://127.0.0.1:9000')
    let url = new URL(request.url, 'http://127.0.0.1')
    console.log(url)  //=>如图所示，为一个对象
    // 输出路径
    console.log(url.pathname)  //=>/search
    // 输出 keyword 查询字符串
    console.log(url.searchParams.get('a'))  //=> 100
    response.end('url new')
  })
  ```
* url对象  
  ​![image](assets/image-20231014213312-uybvgtz.png)​

### 设置响应报文

|**作用**|**语法**|
| ------------------| --------------------|
|设置响应状态码|​`response.statusCode`​|
|设置响应状态描述|​`response.statusMessage （ 用的非常少 ）`​​|
|设置响应头信息|​`response.setHeader('头名', '头值')`​​(可以自定义)|
|设置响应体|​`response.write('xx') response.end('xxx')`​​|

* 基本使用

  ```js
  // 1. 设置响应状态码
  response.statusCode = 203
  // 2. 响应状态的描述
  response.statusMessage = 'i love you'
  // 3. 响应头
  response.setHeader('content-type', 'text/html;charset=utf-8')
  // 自定义响应头
  response.setHeader('myHeader', 'test test')
  // 设置多个同名的响应头
  response.setHeader('test', ['a', 'b', 'c'])
  ```
* 设置响应体

  ```js
  // write 和 end 的两种使用情况：
  // 1. write 和 end 的结合使用 响应体相对分散
  response.write('xx');
  response.write('xx');
  response.write('xx');
  response.end(); //每一个请求，在处理的时候必须要执行 end 方法的

  //2. 单独使用 end 方法 响应体相对集中
  response.end('xxx');
  ```

### qs插件

​`qs`​是一个`npm`​仓库所管理的包。可通过`npm install qs`​命令进行安装。

用来解析字符串对象转换的，类似json

```js
const qs = require('qs');

1.qs.parse()
const str = "username='admin'& password='123456'";
console.log(qs.parse(str)); 
// Object { username: "admin", password: "123456" }

2.qs.stringify()
const a = qs.stringify({ username: 'admin', password: '123456' });
console.log(a); 
// username=admin&password=123456
```

## events

实例化 `EventEmitter `​类来绑定和监听事件

```js
// 引入 events 模块
var EventEmitter = require('events');
// 创建 eventEmitter 对象
var event = new EventEmitter();

event.on('someEvent', function(arg1, arg2) { 
    console.log('listener1', arg1, arg2); 
}); 

event.on('someEvent', function(arg1, arg2) { 
    console.log('listener2', arg1, arg2); 
}); 

event.emit('someEvent', 'arg1 参数', 'arg2 参数'); 

//listener1 arg1 参数 arg2 参数
//listener2 arg1 参数 arg2 参数
```

## stream

​`stream`​是`Node.js`​提供的一个仅在服务区端可用的模块，目的是支持“流”这种数据结构。

​流是一种抽象的数据结构。想象水流，当在水管中流动时，就可以从某个地方（例如自来水厂）源源不断地到达另一个地方（比如你家的洗手池）。我们也可以把数据看成是数据流，比如你敲键盘的时候，就可以把每个字符依次连起来，看成字符流。这个流是从键盘输入到应用程序，实际上它还对应着一个名字：**标准输入流（**​**​`stdin`​**​ **）** 。

如果应用程序把字符一个一个输出到显示器上，这也可以看成是一个流，这个流也有名字：**标准输出流（**​**​`stdout`​**​ **）** 。

**流的特点是数据是有序的，而且必须依次读取，或者依次写入，不能像Array那样随机定位。**

有些流用来读取数据，比如从文件读取数据时，可以打开一个文件流，然后从文件流中不断地读取数据。有些流用来写入数据，比如向文件写入数据时，只需要把数据不断地往文件流中写进去就可以了。

在Node.js中，流也是一个对象，我们只需要响应流的事件就可以了：`data`​事件表示流的数据已经可以读取了，`end`​事件表示这个流已经到末尾了，没有数据可以读取了，`error`​事件表示出错了。

### 读取流

`data`​事件可能会有多次，每次传递的`chunk`​是流的一部分数据。

```js
const fs = require('fs');
//创建读取流
let rs = fs.createReadStream('hello.txt', 'utf-8');
rs.on('open', function () {
  console.log('读取的文件已打开');
}).on('close', function () {
  console.log('读取流结束');
}).on('error', err => {
  console.log(err);
}).on('data', function (chunk) {
  //每一批数据流入完成
  console.log('单批数据流入:' + chunk.length);
  console.log(chunk);
});
```

### 写入流

```js
const fs = require('fs');
//创建写入流
let ws = fs.createWriteStream('hello.txt', 'utf-8');

//监听文件打开事件
ws.on('open', function () {
  console.log('文件打开');
});

//监听文件关闭事件
ws.on('close', function () {
  console.log('文件写入完成，关闭');
});

//文件流式写入
ws.write('helloworld1!', function (err) {
  if (err) {
    console.log(err);
  } else {
    console.log('内容1流入完成');
  }
});
ws.write('helloworld2!', function (err) {
  if (err) {
    console.log(err);
  } else {
    console.log('内容2流入完成');
  }
});

//文件写入完成
ws.end(function () {
  console.log('文件写入关闭');
});
```

### pipe

​`pipe`​ 就像可以把两个水管串成一个更长的水管一样，两个流也可以串起来。

一个`Readable`​流和一个`Writable`​流串起来后，所有的数据自动从`Readable`​流进入`Writable`​流，这种操作叫`pipe`​。

在`Node.js`​中 **，**​`Readable`​流有一个`pipe()`​方法，就是用来干这件事的。

用`pipe()`​把一个文件流和另一个文件流串起来，这样源文件的所有数据就自动写入到目标文件里了，所以，这实际上是一个复制文件的程序

* 基本使用

  ```js
  const fs = require('fs');

  //创建读取流
  let rs = fs.createReadStream('video.mp4');
  let ws = fs.createWriteStream('b.mp4');

  rs.on('close', function () {
    console.log('读取流结束');
  });

  rs.pipe(ws);
  ```
* 原理

  ```js
  const fs = require('fs');

  //创建读取流
  let rs = fs.createReadStream('video.mp4');
  let ws = fs.createWriteStream('b.mp4');

  rs.on('close', function () {
    ws.end();
    console.log('读取流结束');
  });

  //每一批数据流入完成
  rs.on('data', function (chunk) {
    console.log('单批数据流入:' + chunk.length);
    ws.write(chunk, () => {
      console.log('单批输入流入完成');
    });
  });
  ```

## zib

浏览器向服务器发起资源请求，比如下载一个`js`​文件，服务器先对资源进行压缩，再返回给浏览器，以此节省流量，加快访问速度。

浏览器通过HTTP请求头部里加上`Accept-Encoding`​，告诉服务器，“你可以用`gzip`​，或者`defalte`​算法压缩资源”。

### 压缩

```js
var fs = require('fs');
var zlib = require('zlib');

var gzip = zlib.createGzip();

var readstream = fs.createReadStream('./extra/fileForCompress.txt');
var writestream = fs.createWriteStream('./extra/fileForCompress.txt.gz');

readstream.pipe(gzip).pipe(writestream);
```

### 解压

```js
var fs = require('fs');
var zlib = require('zlib');

var gunzip = zlib.createGunzip();

var readstream  = fs.createReadStream('./extra/fileForCompress.txt.gz');
var writestream  = fs.createWriteStream('./extra/fileForCompress1.txt');

readstream.pipe(gunzip).pipe(writestream);
```

### 服务端gzip压缩

* 基本使用

  ```js
  // 1.首先判断 是否包含 accept-encoding 首部，且值为 gzip。
  // 2.否：返回未压缩的文件。是：返回gzip压缩后的文件。
  var http = require("http");
  var zlib = require("zlib");
  var fs = require("fs");
  var filepath = "./extra/fileForGzip.html";

  var server = http.createServer(function (req, res) {
    var acceptEncoding = req.headers["accept-encoding"];
    var gzip;
    if (acceptEncoding.indexOf("gzip") != -1) {
      // 判断是否需要gzip压缩
      gzip = zlib.createGzip();
      // 记得响应 Content-Encoding，告诉浏览器：文件被 gzip 压缩过
      res.writeHead(200, {
        "Content-Encoding": "gzip",
      });
      fs.createReadStream(filepath).pipe(gzip).pipe(res);
    } else {
      fs.createReadStream(filepath).pipe(res);
    }
  });
  server.listen("3000");
  ```
* js大文件返回

  ```js
  const fs = require("fs");
  const zlib = require("zlib"); //这两个要写在fs模块后面
  const gzip = zlib.createGzip();
  const http = require("http");

  http
    .createServer((req, res) => {
      let rs = fs.createReadStream("hello.js");
      res.writeHead(200, {
        "Content-Type": "application/x-javascript;charset=utf-8",
        "Content-Encoding": "gzip",
      });
      rs.pipe(gzip).pipe(res);
    })
    .listen(3000, () => {
      console.log("server start");
    });

  ```
* 字符串gzip压缩：`zlib.gzipSync(str)`​

  ```js
  var http = require('http');
  var zlib = require('zlib');

  var responseText = 'hello world';

  var server = http.createServer(function(req, res){
      var acceptEncoding = req.headers['accept-encoding'];
      if(acceptEncoding.indexOf('gzip')!=-1){
          res.writeHead(200, {
              'content-encoding': 'gzip'
          });
          res.end(zlib.gzipSync(responseText) );
      }else{
          res.end(responseText);
      }

  });

  server.listen('3000');
  ```

## crypto

​`crypto`​模块的目的是为了提供通用的加密和哈希算法。用纯`JavaScript`​代码实现这些功能不是不可能，但速度会非常慢。`Nodejs`​用`C/C++`​实现这些算法后，通过`cypto`​这个模块暴露为`JavaScript`​接口，这样用起来方便，运行速度也快。

### hash

​`hash.digest([encoding])`​：计算摘要。`encoding`​可以是`hex`​、`latin1`​或者`base64`​。

如果声明了`encoding`​，那么返回字符串。否则，返回`Buffer`​实例。

注意，调用`hash.digest()`​后，`hash`​对象就作废了，再次调用就会出错。

​`hash.update(data[, input_encoding])：input_encoding`​可以是`utf8`​、`ascii`​或者`latin1`​。

如果`data`​是字符串，且没有指定` input_encoding`​，则默认是`utf8`​。注意，`hash.update()`​方法可以调用多次。

```js
var crypto = require('crypto');
var fs = require('fs');

var content = fs.readFileSync('./test.txt', {encoding: 'utf8'});
var hash = crypto.createHash('sha256');
var output;

hash.update(content);
hash.digest('hex'); 

// 报错：Error: Digest already called
hash.update(content);
// 报错：Error: Digest already called
hash.digest('hex');
```

### hmac

​`HMAC`​的全称是`Hash-based Message Authentication Code`​，也即在`hash`​的**加盐**运算。

跟`hash`​模块差不多，选定`hash`​算法，指定“**盐**”即可。

```js
var crypto = require('crypto');
var fs = require('fs');

var secret = 'secret';
var hmac = crypto.createHmac('sha256', secret);
var input = fs.readFileSync('./test.txt', {encoding: 'utf8'});

hmac.update(input);

console.log( hmac.digest('hex') );
// 输出：
// 734cc62f32841568f45715aeb9f4d7891324e6d948e4c6c60c0621cdac48623a
```

### MD5

​`MD5（Message-Digest Algorithm）`​是计算机安全领域广泛使用的散列函数（又称哈希算法、摘要算法），主要用来确保消息的完整和一致性。常见的应用场景有密码保护、下载文件校验等。

* 特点

  * 运算速度快：对`jquery.js`​求md5值，57254个字符，耗时1.907ms
  * 输出长度固定：输入长度不固定，输出长度固定（128位）。
  * 运算不可逆：已知运算结果的情况下，无法通过通过逆运算得到原始字符串。
  * 高度离散：输入的微小变化，可导致运算结果差异巨大。
  * 弱碰撞性：不同输入的散列值可能相同。
* 应用场景

  * 文件完整性校验：比如从网上下载一个软件，一般网站都会将软件的md5值附在网页上，用户下载完软件后，可对下载到本地的软件进行md5运算，然后跟网站上的md5值进行对比，确保下载的软件是完整的（或正确的）
  * 密码保护：将md5后的密码保存到数据库，而不是保存明文密码，避免拖库等事件发生后，明文密码外泄。
  * 防篡改：比如数字证书的防篡改，就用到了摘要算法。（当然还要结合数字签名等手段）

```js
var crypto = require('crypto');
var md5 = crypto.createHash('md5');
var result = md5.update('a').digest('hex');
// 输出：0cc175b9c0f1b6a831c399e269772661
console.log(result);
```

# express

[express ](https://www.expressjs.com.cn/)是一个基于 `Node.js`​ 平台的极简、灵活的 WEB 应用开发框架

## 基本使用

路由：确定了应用程序如何响应客户端对特定端点的请求

或者简单的来说，判断客户端请求的方法、路径、参数等条件进行判断，返回不同的页面

### 请求响应

```js
//导入 express
const express = require("express");
//创建应用对象
const app = express();
//创建 get 路由
app.get("/home", (req, res) => {
  res.send("网站首页");
});
//首页路由
app.get("/", (req, res) => {
  res.send("我才是真正的首页");
});
//创建 post 路由
app.post("/login", (req, res) => {
  res.send("登录成功");
});
//匹配所有的请求方法
app.all("/search", (req, res) => {
  res.send("1 秒钟为您找到相关结果约 100,000,000 个");
});
//自定义 404 路由
app.all("*", (req, res) => {
  res.send("<h1>404 Not Found</h1>");
});
//监听端口 启动服务
app.listen(3000, () => {
  console.log("服务已经启动, 端口监听为 3000");
});
```

### 获取请求参数

```js
//导入 express
const express = require('express');
//创建应用对象
const app = express();
//获取请求的路由规则
app.get('/request', (req, res) => {
  // 1. 获取报文的方式与原生 HTTP 获取方式是兼容的
  console.log(req.method);
  console.log(req.url);
  console.log(req.httpVersion);
  console.log(req.headers);
  // 2. express 独有的获取报文的方式
  // 获取路径
  console.log(req.path)
  //获取查询字符串
  console.log(req.query); // 『相对重要』对象形式返回所有的查询字符串
  // 获取指定的请求头
  console.log(req.get('host'));
  res.send('请求报文的获取');
});
//启动服务
app.listen(3000, () => {
  console.log('启动成功....')
})
```

### 路由参数

路由参数指的是 `URL`​ 路径中的参数

```js
app.get('/:id.html', (req, res) => {
  res.send('商品详情, 商品 id 为' + req.params.id);
});
```

## 响应设置

```js
//获取请求的路由规则
app.get("/response", (req, res) => {
  //1. express 中设置响应的方式兼容 HTTP 模块的方式
  res.statusCode = 404;
  res.statusMessage = 'xxx';
  res.setHeader('abc','xyz');
  res.write('响应体');
  res.end('xxx');
  //2. express 的响应方法
  res.status(500); //设置响应状态码
  res.set('xxx','yyy');//设置响应头
  res.send('中文响应不乱码');//设置响应体
  //连贯操作
  res.status(404).set('xxx','yyy').send('你好朋友')
  //3. 其他响应
  res.redirect('http://atguigu.com')//重定向
  res.download('./package.json');//下载响应
  res.json();//响应 JSON
  res.sendFile(__dirname + '/home.html') //响应文件内容
});
```

## response

### 方法概述

```js
res.download()//提示要下载的文件。
res.end()//结束响应过程。
res.json()//发送JSON响应。
res.jsonp()//发送带有JSONP支持的JSON响应。
res.redirect()//重定向请求。
res.render()//渲染视图模板。
res.send()//发送各种类型的响应。
res.sendFile()//将文件作为八位字节流发送。
res.sendStatus()//设置响应状态代码，并将其字符串表示形式发送为响应正文。
```

### **redirect**

`response.redirect`​方法允许网址的重定向

```js
response.redirect("/hello/anime");
response.redirect("http://www.example.com");
response.redirect(301, "http://www.example.com"); 
```

### **sendFile**

​`response.sendFile`​方法用于发送文件

```js
response.sendFile("/path/to/anime.mp4");
```

### **render**

​**​`response.render`​**​方法用于渲染网页模板。

```js
//  使用render方法，将message变量传入index模板，渲染成HTML网页
app.get("/", function(request, response) {
  response.render("index", { message: "Hello World" });
});
```

‍

## 中间件

中间件（`Middleware`​​）本质是一个回调函数，作用就是使用函数封装公共操作，简化代码。

简单说，中间件（`middleware`​）就是处理`HTTP`​请求的函数。它最大的特点就是，一个中间件处理完，再传递给下一个中间件。App实例在运行过程中，会调用一系列的中间件。

每个中间件可以从App实例，接收三个参数，依次为`request`​对象（代表HTTP请求）、`response`​对象（代表HTTP回应），`next`​回调函数（代表下一个中间件）。每个中间件都可以对HTTP请求（request对象）进行加工，并且决定是否调用next方法，将request对象再传给下一个中间件。

‍

​​![34b8a3daffba5b366afb6495e4808b12de732403](assets/34b8a3daffba5b366afb6495e4808b12de732403-20231021142716-4py430f.jpg)​

### 全局中间件

* 声明全局中间件

  ```js
  中间件可以访问到 request 和r esponse
  let recordMiddleware = function(request,response,next){
    //实现功能代码
    //.....
    next(); //执行next函数(当如果希望执行完中间件函数之后，仍然继续执行路由中的回调函数，必须调用next)
  }
  ```
* 应用全局中间件

  ```js
  //可以单独使用一个中间件
  app.use(recordMiddleware)

  //可以一次性使用多个中间件
  app.use(function (request, response, next) {
    console.log('定义第一个中间件');
   next();
  })
  app.use(function (request, response, next) {
    console.log('定义第二个中间件');
    next();
  })
  ```

### 路由中间件

只需要对某一些路由进行功能封装 **，则就需要路由中间件**

```js
//中间件函数和全局函数是一样的
app.get('/路径',`中间件函数`,(request,response)=>{
});
app.get('/路径',`中间件函数1`,`中间件函数2`,(request,response)=>{
});
```

### 静态资源中间件

静态资源：`html`​、`js`​、`css`​、图片等

```js
//引入express框架
const express = require('express');
//创建服务对象
const app = express();
//静态资源中间件的设置，将当前文件夹下的public目录作为网站的根目录
app.use(express.static('./public')); //当然这个目录中都是一些静态资源
//如果访问的内容经常变化，还是需要设置路由
//但是，在这里有一个问题，如果public目录下有index.html文件，单独也有index.html的路由
//则谁书写在前，优先执行谁
app.get('/index.html',(request,response)=>{
  respsonse.send('首页');
});
//监听端口
app.listen(3000,()=>{
  console.log('3000 端口启动....');
});
```

### body-parser

用于获取请求体参数的中间件插件

* 安装

  ```js
  npm i body-parser
  ```
* 基本使用

  ```js
  const bodyParser = require('body-parser');
  //处理 querystring 格式的请求体
  let urlParser = bodyParser.urlencoded({extended:false}));
  //处理 JSON 格式的请求体
  let jsonParser = bodyParser.json();

  //设置成路由中间件
  urlParser中间件会对request进行处理，
  app.post('/login', urlParser, (request,response)=>{
    //获取请求体数据
    //console.log(request.body);
    //用户名
    console.log(request.body.username);
   //密码
    console.log(request.body.userpass);
    response.send('获取请求体数据');
  });
  ```

## Router

* 创建路由对象

  ```js
  //1. 导入 express
  const express = require("express");
  //2. 创建路由器对象
  const router = express.Router();
  //3. 在 router 对象身上添加路由
  router.get("/", (req, res) => {
    res.send("首页");
  });

  router.get("/cart", (req, res) => {
    res.send("购物车");
  });
  //4. 暴露
  module.exports = router;
  ```
* 使用`router`​中间件

  ```js
  const express = require('express');
  const app = express();
  //5.引入子路由文件
  const homeRouter = require('./routes/homeRouter');
  //6.设置和使用中间件
  app.use(homeRouter);

  app.listen(3000,()=>{
    console.log('3000 端口启动....');
  })
  ```

# EJS

## 模板引擎

模板引擎是分离​用户界面和业务数据的一种技术

[EJS ](https://ejs.bootcss.com/)是一个高效的 `Javascript`​ 的模板引擎

## 基本使用

* 安装：`npm i ejs --save`​
* 基本使用

  ```js
  //1.引入ejs
  const ejs = require('ejs');
  //2.定义数据
  let person = ['张三','李四','王二麻子'];
  //3.ejs解析模板返回结构
  //<%= %> 是ejs解析内容的标记，作用是输出当前表达式的执行结构
  //"<%= %>"可以直接输出变量或表达式的值，变量或表达式的值将作为一个字符串在浏览器中输出。
  let html = ejs.render(‘<%= person.join(",") %>’, {person:person}); //把 templateValue：value 。把自己定义的 person 值给模板中的 person 变量使用 
  //4.输出结果
  console.log(html);
  ```
* 基本语法

  ```js
  1.执行JS代码
  <% code %>
  2.输出转义的数据到模板上
  <%= code %>
  3.输出非转义的数据到模板上
  <%- code %>
  ```

## person中使用

* 设置模板引擎

  ```js
  const express = require('express')
  const path = require('path')

  const app = express()

  // 1. 设置模板引擎
  app.set('view engine', 'ejs')  // pug  twing

  // 2. 设置模板文件的存放位置
  // 模板文件: 具有模板语法内容的文件
  app.set('views', path.resolve(__dirname, './views'))

  // 创建路由
  app.get('/home', (req, res) => {
    // 3. render 响应
    // res.render('模板的文件名','数据')
    let title = '尚硅谷 - 让天下没有难学的技术'
    res.render('home', { title })
    // 4. 创建模板文件
    // 如下面文件 views/home.ejs
  })

  // 监听端口，启动服务
  app.listen(3000, () => {
    console.log('服务器已启动~~~~')
  })
  ```
* 设置静态资源：​`views/home.ejs`​

  ```js
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
  </head>
  <body>
    <h2>
      <%=title %>
    </h2>
  </body>

  </html>
  ```

# Mongoose

[Mongoose ](http://www.mongoosejs.net/)是一个对象文档模型库，方便使用代码操作 mongodb 数据库

mongoose中的对象：

* ​`Schema`​模式对象（Schema对象定义约束了数据库中的文档结构）
* ​`Model`​模型对象（Model对象作为集合中的所有文档的表示，相当于MongoDB数据库中的集合collection）
* ​`Document `​文档对象（Document表示集合中的具体文档，相当于集合中的一个具体的文档）

## MongoDB

[MongoDB ](https://www.mongodb.com/)是一个基于分布式文件存储的数据库

数据库（DataBase）是按照数据结构来组织、存储和管理数据的 `应用程序`​

* 数据库（database） 数据库是一个数据仓库，数据库服务下可以创建很多数据库，数据库中可以存放很多集合
* 集合（collection） 集合类似于 JS 中的数组，在集合中可以存放很多文档
* 文档（document）   文档是数据库中的最小单位，类似于 JS 中的对象

图形化工具：

* [Robo 3T ](https://github.com/Studio3T/robomongo/releases)免费
* [Navicat ](https://www.navicat.com.cn/)收费

## 数据类型

```js
String：字符串
Number：数字
Boolean：布尔值
Array：数组，也可以使用 [] 来标识
Date：日期
BufferBuffer： 对象
Mixed：任意类型，需要使用 mongoose.Schema.Types.Mixed 指定
ObjectId：对象 ID，需要使用 mongoose.Schema.Types.ObjectId 指定
Decimal128：高精度数字，需要使用 mongoose.Schema.Types.Decimal128 指定
```

## 字段校验

```js
title: {
  type: String, //类型
  required: true // 设置必填项
  default: '男' //默认值
  enum: ['男','女'] //设置的值必须是数组中的
  unique: true//唯一值，unique 需要 重建集合 才能有效果
},
```

## 基本使用

```js
const mongoose = require("mongoose");
//连接数据库
mongoose.connect("mongodb://127.0.0.1:27017/bilibili");
//设置连接回调
mongoose.connection.on("open", () => {
  //创建文档结构对象，设置集合中 文档的属性以及属性值得类型
  let BookSchema = new mongoose.Schema({
    title: String,
    author: String,
    price: Number,
  });
  //创建文档模型对象  对文档操作的封装对象  mongoose会使用集合名称的复数，创建集合
  let BookModel = mongoose.model("book", BookSchema);
  //7. 插入文档
  BookModel.create(
    {
      title: "西游记",
      author: "吴承恩",
      price: 19.9,
    },
    (err, data) => {
      if (err) throw err;
      console.log(data); //输出 data 对象  如果没有出错，则输出插入后的文档对象
      mongoose.disconnect();//断开连接  关闭数据链接 (项目运行过程中，不会添加该代码)
    }
  );
});
//连接出错
mongoose.connection.on("error", () => {
  console.log("连接出错~~");
});
//连接关闭
mongoose.connection.on("close", () => {
  console.log("连接关闭");
});
```

## CURD

增加（create），删除（delete），修改（update），查（read）

* 插入一条 :`DBModel.create`​

  ```js
  SongModel.create(
    {
      title: "给我一首歌的时间",
      author: "Jay",
    },
    function (err, data) {
      //错误
      console.log(err);
      //插入后的数据对象
      console.log(data);
    }
  );
  ```
* 批量插入:`DBModel.insertMany`​

  ```js
  PhoneModel.insertMany(
    [
      {
        brand: "华为",
        color: "灰色",
        price: 2399,
        tags: ["电量大", "屏幕大", "信号好"],
      },
      {
        brand: "小米",
        color: "白色",
        price: 2099,
        tags: ["电量大", "屏幕大", "信号好"],
      },
    ],
    (err, data) => {
      if (err) throw err;
      console.log("写入成功");
      mongoose.connection.close();
    }
  );
  ```
* 删除一条:`DBModel.deleteOne`​

  ```js
  SongModel.deleteOne({ _id: "5dd65f32be6401035cb5b1ed" }, function (err) {
    if (err) throw err;
    console.log("删除成功");
    mongoose.connection.close();
  });
  ```
* 批量删除:`DBModel.deleteMany`​

  ```js
  SongModel.deleteMany({ author: "Jay" }, function (err) {
    if (err) throw err;
    console.log("删除成功");
    mongoose.connection.close();
  });
  ```
* 更新一条:`DBModel.updateOne`​

  ```js
  SongModel.updateOne({ author: "JJ Lin" }, { author: "林俊杰" }, function (err) {
    if (err) throw err;
    mongoose.connection.close();
  });
  ```
* 批量更新:`DBModel.updateMany`​

  ```js
  SongModel.updateMany(
    { author: "Leehom Wang" },
    { author: "王力宏" },
    function (err) {
      if (err) throw err;
      mongoose.connection.close();
    }
  );
  ```
* 查询一条:`DBModel.findOne`​\<span data-type="code">DBModel.findById</span>

  ```js
  SongModel.findOne({ author: "王力宏" }, function (err, data) {
    if (err) throw err;
    console.log(data);
    mongoose.connection.close();
  });
  //根据 id 查询数据
  SongModel.findById("5dd662b5381fc316b44ce167", function (err, data) {
    if (err) throw err;
    console.log(data);
    mongoose.connection.close();
  });
  ```
* 批量查询`DBModel.find`​

  ```js
  //不加条件查询
  SongModel.find(function (err, data) {
    if (err) throw err;
    console.log(data);
    mongoose.connection.close();
  });
  //加条件查询
  SongModel.find({ author: "王力宏" }, function (err, data) {
    if (err) throw err;
    console.log(data);
    mongoose.connection.close();
  });
  ```

## 条件查询

### 运算符

* 在 mongodb 不能 > < >= <= !== 等运算符，需要使用替代符号

  ```js

  >      $gt
  <      $lt
  =      $gte
  <=     $lte
  !==    $ne
  逻辑或  $or
  逻辑与  $and
  ```
* 逻辑运算符

  ```js
  BookModel.find({ price: { $lt: 20 } }, ()=>{});// 价格小于 20 的图书
  BookModel.find({ $or: [{ author: '曹雪芹' }, { author: '余华' }] }, ()=>{})// 曹雪芹或者余华的书
  BookModel.find({ $and: [{ price: { $gt: 30 } }, { price: { $lt: 70 } }] }, ()=>{})// 价格大于 30 且 小于 70
  ```

### 正则

```js
//正则表达式，搜索书籍名称中带有 '三' 的图书
BookModel.find({ name: /三/ }, (err, data) => {});
BookModel.find({ name: new RegExp("三") }, (err, data) => {});
```

### 字段筛选

```js
//0:不要的字段
//1:要的字段
//select  筛选
//exec  继续执行函数
SongModel.find().select({ _id: 0, title: 1 }).exec(function (err, data) {
    if (err) throw err;
    console.log(data);
    mongoose.connection.close();
});
```

### 数据排序

```js
// sort 排序
// 1:升序
// -1:倒序
SongModel.find().sort({hot:1}).exec(function(err,data){
  if(err) throw err;
  console.log(data);
  mongoose.connection.close();
});
```

### 数据截取

```js
//skip 跳过   limit 限定
SongModel.find().skip(10).limit(10).exec(function(err,data){
  if(err) throw err;
  console.log(data);
  mongoose.connection.close();
});
```

‍
