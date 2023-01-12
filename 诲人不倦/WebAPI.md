---
title: WebAPI
date: 2022-07-27T20:16:39Z
lastmod: 2022-10-13T12:04:06Z
---

![image](assets/v2-d5a78cd0626ff0acfe3f0d3a65828108_r-20220727205227-2gkknpd.jpg)

# WebAPI

# WebAPI基础

## Dom

### 获取页面元素

```js
let xx = document.querySelector('选择器')              //根据选择器获取满足条件第一个元素,返回值是dom对象或null
let xxList = document.querySelectorAll('选择器')       //根据选择器获取满足条件所有元素，返回值是伪数组
let btn = document.getElementById('btn')              //根据id名获取dom对象
let liList = document.getElementsByTagName('li')      //根据标签名获取，返回值是伪数组
let boxList = document.getElementsByClassName('box')  //根据类名获取，返回值是伪数组
let genderList = document.getElementsByName('gender') //根据name属性获取表单元素，返回值是伪数组
```

### 元素内容操作

```js
let oH = document.getElementById('h')
oH.innerText = ‘你好’  //对双边标签文本内容进行读写操作
oH.innerHTML =  'hello <big>world</big>'  //对双边标签内容(文本+标签)进行读写操作
```

### 元素属性操作

```js
dom.id  //获取id
dom.className  //获取类名
dom.href  //获取链接（常用于a标签）
dom.src  //获取路径(常用于img标签)
```

### 元素非标属性操作

* 设置非标属性：非标属性设置使用`data-xx='xxx'`

  ```js
  <img id="pic" src="./images/yshg.png" alt="" data-sex='男'>
  ```
* 获取非标属性：`dom.data.xx`

  ```js
  document.querySelector('img').dataset.sex = '新属性'
  ```
* 使用`attribute`：主要用来操作非标属性

  ```js
  dom.getAttribute('属性名')//用于获取标签对象的属性
  dom.setAttribute('属性名', '属性值')//用于设置标签对象的属性
  dom.removeAttribute('属性名')//用于移除标签对象属性
  ```

### 元素样式属性操作

```js
let box = document.querySelector('.box')
box.style.backgroundColor = 'skyblue'  //添加行内样式
box.className = 'color'    //替换类名，原类名会被覆盖
box.classList.add('one')  //新增类名
box.classList.remove('one')  //移除类名
box.classList.toggle('one')  //切换类名，有就增，没就加
const bol =  box.classList.contains('one')  //判断是否有该类名，返回值是boolean
```

### 表单元素属性

```js
let form = document.querySelector('input')
form.value = 'xxx'  //修改表单元素value值，即表达文本
form.disabled = false  //设置表达禁用状态
form.checked  = false  //设置选中状态，用于radio，chenkbox
form.selected = false  //设置选中状态，用于option
```

### 焦点事件

```js
let oInput = document.querySelector('input')
oInput.onfocus = ()=>{}//获得焦点
oInput.onblur = ()=>{}//失去焦点
```

### 定时器

* setTimeout：一次性定时器当指定的毫秒数到达时，才执行指定的函数

  ```js
  setTimeout(()=>{},time)
  //设置一次性定时器，返回定时器标识
  let timer = setTimeout(function(){
    console.log('hello');
  },3000);
  //清除一次性定时器
  clearTimeout(timer);

  ```
* setInterval：重复定时器，每间隔指定的毫秒数，就执行一次指定的 函数

  ```js
  setInterval(()=>{},tiem)
  //设置重复定时器，返回定时器标识
  let timer = setInterval(function(){
    console.log('hello');
  },2000);
  //清除重复定时器
  clearInterval(timer);

  ```

## Node节点

### 节点概念

* 节点：对一个html的DOM树更细致的描述
* 节点的三个信息

  * 节点类型dom.nodeType
  * 节点名称dom.nodeName
  * 节点的值dom.nodeValue

### 获取节点

* 获取兄弟节点

  ```js
  let box= document.getElementById('box');
  box.previousSibling          //获取上一个兄弟节点
  box.previousElementSibling   //获取上一个元素兄弟节点
  box.nextSibling              //获取下一个兄弟节点
  box.nextElementSibling       //获取下一个元素兄弟节点
  ```
* 获取子节点

  ```js
  let oDiv = document.getElementById('box');  
  oDiv.childNodes;// 获取所有的子节点
  oDiv.children;// 获取所有的子元素节点
  oDiv.firstElementChild //获取第1个子元素节点 
  oDiv.lastElementChild//获取最后1个子元素节点
  ```
* 获取父节点

  ```js
  let oDiv = document.getElementById('box');
  oDiv.parentNode  //用于获取父元素节点
  ```

### 元素节点的操作

* 创建节点：**新创建的节点仅位于内存中，必须添加到DOM树上才会有页面效果**

  ```js
  let oH1 = document.createElement('h1')  //用于创建一个标签
  ```
* 追加节点：子节点追加到父节点后面

  ```js
  父节点.appendChild(子节点)
  ```
* 克隆节点：用于对节点进行克隆操作,克隆出来的节点仅位于内存中，参数`boolean值`，`ture`为深克隆

  ```js
  被克隆节点.cloneNode(true)//"深"克隆，包含子元素一同被克隆
  被克隆节点.cloneNode//默认是false，不包含子元素
  ```
* 插入节点：将子节点插入到父节点内的目标节点位置上，**目标节点后移**，如果目标节点不存在则是插入到最前面

  ```js
  父元素.insertBefore(子节点,目标节点)
  ```
* 删除节点：将父节点内的子节点删除，注意只能是父子关系

  ```js
  父节点.removeChild(子节点)
  ```

## 页面尺寸和滚动

### offset

* offsetWidth、offsetHeight

  ```js
  //offsetWidth     获取元素在页面的宽尺寸(width padding border)
  //offsetHeight    获取元素在页面的高尺寸(height padding border)
  //它是一个组合属性，返回值是数值没有单位。
  let oBox = document.querySelector(".box")
  console.log(oBox.style.width);
  console.log(oBox.offsetWidth);
  console.log(oBox.offsetHeight);
  ```
* offsetParent：用于获取相对定位的父元素。

  ```js
  //offsetParent 用于获取相对位置的父元素。
  //如果元素的定义方式是fixed, 元素.offsetParent就是null
  //如果元素的定义方式是非fixed, 
  //元素.offsetParent就是距离它最近具有定位方式的父元素
  //如果没有这样的父元素，元素.offsetParent 就是body
  let oCore = document.querySelector('.core');
  console.log(oCore.offsetParent);
  ```
* offsetLeft、offsetTop

  ```js
  //offsetLeft与offsetTop是用于获取当前元素，距离它的offsetParent的尺寸
  //offsetLeft获取当前元素左边距离它的offsetParent左侧的距离，也是组合尺寸，不可以修改。也是没有单位
  //offsetLeft offsetTop获取的不仅是let与top的值，它是一个组合尺寸,也可以能包含margin
  let oCore = document.querySelector('.core');
  console.log(oCore.offsetLeft);
  console.log(oCore.offsetTop);
  ```

### scroll

```js
let oBox = document.querySelector('.box');
console.log(oBox.scrollWidth);//scrollWidth,可以简单的理解为内容的宽度
console.log(oBox.scrollHeight);// scrollHeight,可以简单的理解为内容的高度
console.log(oBox.scrollTop);//scrollTop，读取/设置内容向上滚动的距离，没有单位
console.log(oBox.scrollLeft);//scrollLeft，读取/设置内容向左滚动的距离，没有单位
dom.onscroll = ()=>{}//滚动事件
```

### client

```js
let oBox = document.querySelector('.box');
console.log(oBox.clientWidth);//clientWidth获取可视区域的宽
console.log(oBox.clientHeight);//clientHeight获取可视区域的高
```

## WebSocket

### WebSocket简介

　　`WebSocket`是一套通讯协议，是属于WebAPI的内容，是H5新增的。

　　`WebSocket`通讯协议的特点：1.长连接；2.服务端可以主动给客户端发消息

* 它基于`http：超文本传输协议`，因为先用http请求建立连接，后面就是`websocke`t的**长链接技术**进行交互

* 协议头：`ws://` 、`wss://`​

### WebSocket基本使用

```js
//创建WebSocket对象
let ws = new WebSocket('连接地址')

//监听服务器发的消息 
ws.onmessage = function (e) {
    // 当服务器有消息，就会自动调用这个函数
    // e.data有服务器返回的数据
}

//先服务器发送消息
ws.send('发送的内容')

//关闭连接
ws.close()

//连接成功时的调用方法
ws.onopen = function () {
    // 连接成功时调用
}

//连接关闭时的调用方法
ws.onclose = function () {
    // 连接关闭时调用
}
```

### socket.io插件

* `socket.io`是一套基于`websocket协议`的框架。有客户端和服务器端版本。一般使用`socket.io`快速搭建`WebSocket`的接口，也可以在浏览器中快速调用接口
* 下载socket.io

  ```js
  //使用npm给项目安装socket.io框架
  npm i socket.io-client
  ```
* 基本使用

  ```js
  导包
  import io from "socket.io-client";

  如多个地方需要使用到socket实例，可以把socket定义在data中
  data () {
      return {
          socket:null
      }
  }

  创建socket.io对象，并建立链接
  const socket = io('连接的服务器')//看后台文档，查看创建链接是否需要携带数据

  向服务器端发送信息
  socket.emit('消息类型', 消息内容)

  监听服务器信息
  socket.on('消息类型', msg => {
    
      // msg就是服务器返回的消息
  })

  ```

## 事件

### 事件概念

　　`js`处理用户交互的一种机制

　　事件三要素：事件源、事件类型、事件处理函数

### 事件注册

```js
//事件源.on事件名 = 事件处理函数，只能注册一个，有重复会覆盖
dom.onClick = function(){}
```

### 事件监听

　　事件监听是为元素绑定事件的另一种方式，这种方式可以为一个对象的同一个事件绑定多个事件处理函数。

* 添加事件监听：,事件名不需要加`on`，可以绑定多个事件

  ```js
  //标签对象.addEventListener('事件名',事件处理函数，boolean)
  dom.addEventListener("click",function(){},boolean)//boolean开启事件补助

  let oBtn = document.querySelector('#btn');
  //可以添加多个函数，会依次执行
  oBtn.addEventListener('click', function(){
    console.log('hello');
  })
  oBtn.addEventListener('click', function(){
    console.log('word');
  })
  ```
* 开启事件捕捉：`标签对象.addEventListener('事件名',事件处理函数，true)`

  ```js
  //默认值是false
  oBtn.addEventListener('click', function(){
    console.log('hello');
  }，true)
  ```
* 移除事件监听

  ```js
  对象.removeEventListener(事件名,事件处理函数名)
  ```

### 事件对象

* 概念：事件对象也是对象，里面存储了**事件触发时候的相关信息**

  ```js
  //获取事件对象
  document.onclick = function(e){
    console.log(e);//这个变量e就是事件对象  
  }

  document.addEventListener('click', function(e){
    console.log(e)//这个变量e就是事件对象  
  })
  ```
* 获取实际事件触发对象

  ```js
  document.onclick = function(e){
    console.log(e.target);//事件实际触发的对象
    console.log(e.currentTarget)//指的是处理该事件的DOM元素,即事件绑定的dom
  }
  ```
* 视频播放事件对象常用属性：设置给video与audio标签，当播放进度发变化时触发该事件

  ```js
  //  currentTime属性：可以用于读取或设置播放进度
  //  ontimeupdate事件：视频播放事件
  //  onlodateddata事件：当vidoe或audio数据加载完毕后触发，准备播放时触发
     

  // 在ontimeupdate事件中读取播放进度
  document.querySelector("video").ontimeupdate = _.throttle(function () {
    localStorage.setItem("cTime", this.currentTime);
  }, 1000);

  // 在onloadeddata事件中从本地读取上次保存的播放进度，并设置给currentTime
  document.querySelector("video").onloadeddata = function () {
    this.currentTime = localStorage.getItem("cTime");
  };
  ```
* 鼠标事件对象常用属性

  ```js
  .onmousedown    鼠标按下
  .onmouseup      鼠标释放
  .onmousemove    鼠标移动

  // 事件对象.pageX         文档：用于获取鼠标点击的位置距离文档左上角水平距离
  // 事件对象.pageY         文档：用于获取鼠标点击的位置距离文档左上角垂直距离

  // 事件对象.clientX       窗口：用于获取鼠标点击的位置距离窗口左上角水平距离
  // 事件对象.clientY       窗口：用于获取鼠标点击的位置距离窗口左上角垂直距离

  // 事件对象.screenX       屏幕：用于获取鼠标点击的位置距离屏幕左上角水平距离
  // 事件对象.screenY       屏幕：用于获取鼠标点击的位置距离屏幕左上角垂直距离

  //  获取event对象
  document.onmousemove= function (e) {
    console.log(e.pageX, e.pageY);
    console.log(e.clientX, e.clientY);
    console.log(e.screenX, e.screenY);
  };
  ```
* 键盘事件对象常用的属性

  ```js
  //keydown          键盘按下事件
  //keyup            键盘按键释放事件
  //事件对象.key      获得按下的键
  //事件对象.keyCode  获得按下键的编码
  //一般键盘事件是设置给文本框，或设置给document
  //通过键盘事件对象，可以识别按键需要使用keyCode(键码)

  let oTxt = document.querySelector("#txt");
  oTxt.onkeydown = function (e) {
    let event = window.event || e;
    console.log(event.key);
    console.log(event.keyCode);
  };
  oTxt.onkeyup = function () {
    // console.log('up....');
  };
  ```

### 事件流

　　事件触发整套流程，win->dom->win，你触发什么事件，就执行这条链上的同名触发事件函数，vue中也有这个情况

　　![image](assets/image-20220923085931-0eifcni.png)​

* **事件捕获阶段**：事件从父元素开始向目标元素传播，从 `Window` 对象开始传播。

  ```js
  事件捕捉默认不开启，从windows开始向事件实际触发dom传播，把开启捕捉的函数在传播过程中都执行一次
  dom.addEventListener("click", function () {
    ...  
  ),true);//加true该函数在事件捕捉时候执行

  dom.addEventListener("click", function () {
    ...  
  ));//不加或false该函数在事件冒泡时候执行
  ```
* **事件目标阶段**：该事件到达目标元素或开始该事件的元素，到了事件实际触发对象后，执行事件冒泡。

  ```js
  事件传播到实际触发事件的dom元素上
  document.onclick = function(e){
    console.log(e.target);//事件实际触发的对象
    console.log(e.currentTarget)//指的是处理该事件的DOM元素,即事件绑定的dom
  }
  ```
* **事件冒泡阶段**：这时与捕获阶段相反，事件向父元素传播，触发同名事件函数，直到 `Window` 对象。

  ```js
  事件冒泡如果没有中断的话，会一直执行，有多少执行多少，最后到windows对象
  有些事件不会触发冒泡：focus，blur，change，submit，reset，select等。
  可以利用事件冒泡的特性来事件委托，可以减少内存消耗，动态绑定事件，减少内存消耗优化页面性能
  ```
* **阻止事件传播**：让事件不再向里或向外传递给其他的元素，即可以阻止冒泡也可以阻止捕获。

  ```js
  dom.addEventListener('click', function(e){
    e.stopPropagation();//执行这个函数，这个事件传播就断了，不会事件不会再执行后面的步骤
  })
  ```
* **禁用默认事件：**禁用原dom的事件触发，执行自己的

  ```js
  如点击a标签，a会跳转，禁用a标签原生事件
  oA.onclick = function(e){
    // 需要使用 事件对象 e.preventDefault()
    e.preventDefault();
  }
  ```
* 示例代码

  ```js
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Document</title>
      <style>
        .outer {
          width: 300px;
          height: 300px;
          background: #09f;
        }

        .middle {
          width: 200px;
          height: 200px;
          background: #90f;
        }

        .inner {
          width: 100px;
          height: 100px;
          background: #f90;
          /* margin-left: 310px; */
        }
      </style>
    </head>

    <body>
      <div class="outer">
        外
        <div class="middle">
          中
          <div class="inner">里</div>
        </div>
      </div>
      <script>
        let oInner = document.querySelector(".inner");
        let oMiddle = document.querySelector(".middle");
        let oOuter = document.querySelector(".outer");

        oOuter.addEventListener(
          "click",
          function () {
            console.log("外1 开启");
          },
          true
        );
        oOuter.addEventListener("click", function () {
          console.log("外2");
        });
        oMiddle.addEventListener("click", function () {
          console.log("中1 关闭");
        });
        oMiddle.addEventListener("click", function () {
          console.log("中2");
        });

        oInner.addEventListener(
          "click",
          function () {
            console.log("里1 开启");
          },
          true
        );
        oInner.addEventListener("click", function () {
          console.log("里2 ");
        });
      </script>
    </body>
  </html>
  ```

## 正则

### 正则表达式

```js
//正则的作用是对字符串进行匹配操作，在代码上的体现是一个正则对象
//正则表达式表示的是一种规则，不具体的一种规则
// 语法1：通过new
//  new RegExp(正则表达式)
let reg1 = new RegExp(/ /)

// 语法2：字面量 //
let reg2 = /\d/;        // 此正则表示的规则1个数字
console.log(reg2);
```

### 正则方法

* 正则校验：检验字符串是否匹配正则

  ```js
  正则对象.test(字符串)//返回值是boolean
  let reg = /123/
  console.log(reg.test('123'))
  ```
* 正则替换：字符串替换

  ```js
  // 字符串.replace(正则,替换内容)
  // 在字符串中，将正则匹配到的部分使用 替换内容 进行替换
        
  // 默认仅替换匹配到的第1个
  console.log('abc3des5d'.replace(/\d/,'*'))   //abc*des5d
  console.log('abc3des5d'.replace(/\d/g,'*'))  //abc*des*d
  ```
* 正则查找：查找字符串中符合正则规则的字符，并返回数组

  ```js
  正则.exec(str)
  作用：查看正则表达式匹配str的结果（是第几个字符满足匹配规则的，是哪个字符满足匹配规则等）
  特点：默认情况下，每一次调用exec只会返回第一个匹配的字符串信息，如果想要返回所有匹配信息
  a.需要设置正则表达式为全局匹配
  b.需要执行多次exec方法
  属性介绍
  index:匹配字符的下标
  下标：0：匹配字符
  1-n：后面的下标只对()分组有效，没有分组则数组只有一个元素

  //示例1：找出第一个匹配字符中包含  数字+单词+单词 字符的内容
  //默认：只能找到第一个匹配的字符
  var str = "1aasjdg2bbjahsgd3cc";
  var reg = /\d\w\w/;
  var resArr = reg.exec ( str );
  console.log ( resArr );//["1aa", index: 0, input: "1aasjdg2bbjahsgd3cc", groups: undefined]

  //示例2：找出所有匹配字符中包含   数字+单词+单词 字符的内容
  var str = "1absjdg2cdjahsgd3ef";
  var reg = /\d\w\w/g;//需要设置全局匹配
  var resArr = reg.exec ( str );
  console.log ( resArr );//["1ab", index: 0, input: "1absjdg2cdjahsgd3ef", groups: undefined]

  //每一次匹配之后，reg会记录当前已经匹配的字符，下一次再执行exec的时候就会往后面匹配
  resArr = reg.exec(str);
  console.log ( resArr );//["2cd", index: 0, input: "1absjdg2cdjahsgd3ef", groups: undefined]
  resArr = reg.exec(str);
  console.log ( resArr );//["3ef", index: 0, input: "1absjdg2cdjahsgd3ef", groups: undefined]

  //以上代码可以简写成
  while(resArr){//只要resArr存在，就继续执行exec
    console.log ( resArr );
    resArr = reg.exec(str);
  };

  //示例3：如果正则有分组，则数组从1下标开始获取分组对应的字符
  var reg1 = /\d(\w)(\w)/;
  console.log ( reg1.exec ( str ) );
  //["1ab", "a", "b", index: 0, input: "1absjdg2cdjahsgd3ef", groups: undefined]
  ```

### 元字符

* 概念

  ```js
  元字符:  与字面意思并不同，是被正则赋予了特殊功能的字符
  原义文本字符:  含义与字面意思相同的字符
  ```
* 预定义类

  ```js
  被正则赋予了特殊含义的单个字符 或 字符组合
  \d  :表示数字
  \D  :表示非数字
  \w  :表示单词字符，大小写字母、数字、下划线
  \W  :非单词字符
  \s  :空白字符，空格、制表符、换行、回车
  \S  :非空白字符
  .  :匹配除 \n \r 之外的字符
  ```
* 字符范围类：表示一个范围中的任何一个字符

  ```js
  []
  [abcXYZ678]  //字符 abcXYZ678 中的任何一个
  [a-z]  //匹配任何一个小写字符
  [a-zA-Z0-9]  //大小写字符以及数字
  ```
* 汉字类：需要通过编码进行匹配`[\u4e00-\u9fa5]`

  ```js
  每个汉字在计算中都有一个编码，汉字编码的范围在 \u4e00-\u9fa5 之间。
  \u表示的是16进制
  ```
* 反向范围匹配：`^`符号必须在 `[ ]` 内(`[^]`)，而且是第1位才是反向范围匹配

  ```js
  [ ]  :表示一个字符范围
  [^]  :反向匹配一个范围之外的字符
  [^a-h]//不包含a-h的任何一个字符
  ```
* 边界：元字符 `^` 与`$`

  ```js
  默认的正则在进行匹配时，只要是字符串中的部分与正则规则匹配就表示成功。
  严格匹配就是要对字符串整体进行匹配，即是整体就要能匹配开头与结尾。
  ^  :表示字符开头
  $  :表示字符结尾
  ```
* 量词：用于表示正则的数量

  ```js
  ?  :出现0次或1次（最多1次）
  +  :出现1次或多次（至少1次）
  *   :不限制
  {3}  :3次
  {3,}  :至少3次
  {3,5}  :3次到5次之间
  ```
* 分组：`()`：用于表示所操作的范围

  ```js
  // 需求：需要得到 'okokok' 字符串
  console.log(/ok{3}/.test('okokok'));      // false
  // ok{3}  {3}是修饰前面的k出现3次
  console.log(/ok{3}/.test('okkk'));        // true

  // 使用()表示 {3}所修饰的范围
  console.log(/(ok){3}/.test('okokok'));    // true
  console.log(/(ok){3}/.test('okkk'));      // false

  ```
* 或运算与分组：用于完成或运算，经常需要借助`()`对或的内容进行分组

  ```js
  // | 用于进行或运算
  console.log(/b|look/.test('book'));   // true
  console.log(/b|look/.test('look'));   // true

  // 或运算与分组
  // 不使用分组   默认是 n 与 l进行或运算
  console.log(/^zhangsan|lisi$/.test('zhangsanisi'));   // true
  console.log(/^zhangsan|lisi$/.test('zhangsalisi'));   // true

  // 使用分组     括号内的 zhangsan 与 lisi 进行或
  console.log(/^(zhangsan|lisi)$/.test('zhangsanisi'));    // false
  console.log(/^(zhangsan|lisi)$/.test('zhangsalisi'));    // false
  console.log(/^(zhangsan|lisi)$/.test('zhangsan'));        // true
  console.log(/^(zhangsan|lisi)$/.test('lisi'));            // true
  ```
* 修饰符

  ```js
  i:正则默认是区分大小写的，使用 i 修改符可以不区分大小写
  console.log(/abc/.test('abc'));   // true
  console.log(/abc/.test('Abc'));   // false
  console.log(/abc/.test('aBC'));   // false
  console.log(/abc/i.test('aBC'));   // true

  g:全局操作：主要用于替换操作，替换字符串里全部匹配到的部分,默认仅替换匹配到的第1个
  console.log('abc3des5d'.replace(/\d/,'*'))   //abc*des5d
  console.log('abc3des5d'.replace(/\d/g,'*'))  //abc*des*d

  /正则表达式/i
  /正则表达式/g
  new RegExp(正则表达式,修饰符)
  ```
* 转义符：`\`：去除某些字符被正则赋予的特殊的功能

  ```js
  //去除某些字符被正则赋予的特殊的功能
  //  . 被正则赋予获取除 \r \n外所有的字符。
  console.log(/./.test('.'));     //true
  console.log(/./.test('a'));     //true
  //  \. 是将  . 的功能转义掉，去匹配最原始的 .
  console.log(/\./.test('.'));     //true
  console.log(/\./.test('a'));     //false
  ```
* 贪婪模式

  ```js
  贪婪模式与非贪婪模式一般用于量词
  贪婪模式：正则表达式在匹配成功的前提下，尽可能多的匹配（默认模式）
  非贪婪模式:正则表达式匹配成功的前提下，尽可能少的匹配（在量词后面加上 ?）
  //1.贪婪模式：正则表达式在匹配成功的前提下，尽可能多的匹配
  var reg = /\d{3,6}/;//匹配3-6位数字
  console.log ( "1234567890".replace ( reg, "X" ) );//X7890   (正则表达式会匹配6位数字)

  //2.非贪婪模式:正则表达式匹配成功的前提下，尽可能少的匹配
  //语法：在量词后面加上 ?
  var reg1 = /\d{3,6}?/;//匹配3-6位数字
  console.log ( "1234567890".replace ( reg1, "X" ) );//X4567890   (正则表达式会匹配3位数字)
  ```

## BOM

### BOM概念

　　Brower Object Model浏览器对象模型，浏览器厂商将浏览器设计成由多个对象组成，BOM定义了一套操作浏览器窗口的API

* BOM主要由五大对象组成：

  * window对象：JS中最大的对象
  * location对象：记录了当前地址的详细信息
  * history对象：维护着浏览过的历史记录信息
  * navigator对象：记录着当前浏览器的信息，例如用的什么浏览器，操作系统版本等
  * screen对象：获取用户电脑的屏幕分辨率。这个一般不用，因为对开发者没啥作用

### BOM与DOM 关系

　　![1637143293811](assets/BOM_DOM-20220804230540-0h0z33j.png)​

### window对象

* 概念：window是js最大的代码，代表的是一个浏览器窗口。window对象上的属性可以直接省略window
* window对象方法

  ```js
  window.open(url,name,options)//打开一个新的窗口,并显示url所指向的页面
  //url：新窗口所要显示的页面的url
  //name：新窗口的name值(window.name的值)
  //options用于对新窗口的外观尺寸进行设置
  window.close()//关闭当前窗口
  ```
* window对象事件

  ```js
  window.onload=function(){}//界面上所有的内容加载完毕之后才触发这个事件，这里存放一些页面加载完毕才能顺利进行的代码
  window.onbeforeunload=function(){}//界面在关闭之前会触发这个事件，一般用于关闭之前保存一些数据
  window.onunload=function(){}//界面在关闭的那一瞬间会触发这个事件
  ```

### location对象

* 概念：记录了当前页面的URL信息
* location对象属性

  ```js
  // let url = 'http://www.qd76.com:5000/api_04/0381/locatioA1.html?name=zhangsan&age=20#hash'
  protocol        http:
  host            www.qd76.com:5000
  hostname        www.qd76.com
  port            5000
  pathname        api_04/0381/locatioA1.html
  search          name=zhangsan&age=20
  hash            哈希(锚点)
  console.log(location.protocol);
  console.log(location.host);
  console.log(location.hostname);
  console.log(location.port);
  console.log(location.pathname);
  console.log(location.search);
  console.log(location.hash);
  ```
* location对象方法：跳转新页面或刷新当前页面的功能

  ```js
  location.assign(url):实现跳转
  location.reload():刷新当前页面

  function clk(){
    location.assign('http://www.baidu.com')
  }
  function clk1(){
    location.reload();
  }
  ```

### history对象

　　history对象主要用于记录你当前窗口的历史记录，并为我们提供了前进与后退的功能。

　　`history.forward()`：实现前进

　　`history.back()`：实现后退

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        // history是window对象的一个子对象
        // 记录着一个会话的浏览记录历史，利用history可以在JS代码中实现前进与后退
        // forward()    下一页
        // back()       上一页
        /* 
        history作用是用于实现前进，后退
            forward()
            back()
        */
    </script>
</head>
<body>
    <a href="./history_1.html">跳转</a>
    <button onclick="history.forward()">下一页</button>
</body>
</html>
```

　　`history_1.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        function b(){
            history.back();
        }
        function f(){
            history.forward();
        }

    </script>
</head>
<body>
    <h1>history_1</h1>
    <a href="./history_2.html">跳转</a>
    <hr>
    <button onclick="b()">上一页</button>
    <button onclick="f()">下一页</button>
</body>
</html>
```

　　`history_2.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>history_2</h1>
</body>
</html>
```

### navigator对象

* 概念：记录了浏览器的相关信息
* navigator对象方法

  ```js
  console.log(window.navigator);
  console.log(navigator.userAgent)//返回当前浏览器的用户代理
  //一般的用来判断打开这个页面的浏览器是什么浏览器. 
  if(navigator.userAgent.indexOf('Chrome') != -1){
    alert('当前是谷歌浏览器');
  }else if(navigator.userAgent.indexOf('Firefox') != -1){
    alert('当前是火狐浏览器');
  }else if(navigator.userAgent.indexOf('Safari') != -1){
    alert('当前是Safari浏览器');
  }else {
    alert('是ie等其他浏览器');
  }
  ```

### screen对象

* 概念：记录了浏览器屏幕相关信息
* screen对象方法

  ```js
  //1.显示屏幕的宽高. 
  console.log(screen.width);
  console.log(screen.height);
  //2.除任务栏之外的屏幕宽高
  console.log(screen.availWidth);
  console.log(screen.availHeight);
  ```

### 本地存储

* 概念：本地指定的是浏览者一端。本地存储就是将一些数据存储在浏览者的电脑上
* localStorage方法：永久存储

  ```js
  localStorage.setItem('属性名','属性值')//进行数据的存储操作
  localStorage.getItem('属性名')//进行数据的读取操作
  localStorage.removeItem('属性名')//进行数据的删除操作
  localStorage.clear()//清空数据
  ```
* sessionStorage方法：只要浏览器关闭就没有了

  ```js
  sessionStorage.setItem('属性名','属性值')//进行数据的存储操作
  sessionStorage.getItem('属性名')//进行数据的读取操作
  sessionStorage.removeItem('属性名')//进行数据的删除操作
  sessionStorage.clear()//清空数据
  ```

　　‍

## 重绘（重排）与回流

　　![image](assets/image-20220804232217-69eb40a.png)​

　　![image](assets/image-20220804232224-6be1mrb.png)​

* `浏览器渲染解析页面完整流程`
* 1.先解析HTML，生成DOM树（重要步骤）
* 2.后解析CSS，生成样式规则
* 3.根据DOM树与样式规则，得到一颗渲染树Render Tree（重要步骤）

  * DOM树：只有结构没有样式
  * 渲染树：DOM树 + CSS样式 （可以理解为附加了样式的DOM树）
* 4.渲染引擎开始工作，解析渲染树。

  * 这个步骤发生`回流(又称重排)`  : 计算节点的尺寸、结构、布局
* 5.开始绘制

  * 这个步骤发生`重绘` : 根据重排结果进行绘制页面
* 6.渲染完成，呈现页面
* 重要概念：`重排一定会引起重绘，但是重绘不一定引起重排`

  * 引起重排操作：主要是修改了尺寸、布局（总结起来就是对布局产生印象）

    * 如：修改盒子的尺寸， 修改盒子位置、修改字体大小、边框等等影响盒子模型布局的都会导致重绘
    * `重排`：会导致页面大面积重绘，甚至是整个页面重新绘制。（影响性能）
  * 引起重绘操作：不影响布局，只是影响盒子自身属性

    * 如：修改颜色，修改透明度等，对布局没有任何影响的

　　![image](assets/image-20220804232257-30sii1t.png)​

　　‍
