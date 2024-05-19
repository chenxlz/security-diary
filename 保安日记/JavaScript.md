![image](assets/30-20230130221844-hxib8nk.jpg)

‍

# JS基础

## 模块化

### 导入导出

```js
1.按需导入
import {sayHi, sayBye} from './say.js';
2.全部导入并重命名
import * as say from './say.js';
say.default //获取默认导出的内容
3.全部导入
import * from './say.js';
4.按需导入并重命名
import {sayHi as hi, sayBye as bye} from './say.js';
5.导入默认内容
import say from './say.js';
import {default as say} from './say.js';//和上面等价

6.导出
export sayHi 
7.导出并重命名
export {sayHi as hi, sayBye as bye};
8.默认导出
export default sayHi 
export { sayHi as default } //和上面等价

9.重新导出sayHi
export {sayHi} from './say.js'; 
10.重新导出 默认导出 并重命名
export {default as User} from './user.js';
export User from './user.js' //无效。这会导致一个语法错误。
11.导出命名的导出，默认导出的内容没有
export * from './user.js'
```

### import

​`import(module)`​ 表达式：加载模块并返回一个 `promise`​，该 `promise `​resolve 为一个包含其所有导出的模块对象。我们可以在代码中的任意位置调用这个表达式。

注意点：

1. 动态导入在常规脚本中工作时，它们不需要 script type="module"
2. 尽管 `import()`​ 看起来像一个函数调用，但它只是一种特殊语法，只是恰好使用了括号（类似于 `super()`​）。
3. 因此，不能将 `import`​ 复制到一个变量中，或者对其使用 `call/apply`​。因为它不是一个函数。

## 变量

### 变量声明

​`js`​中，变量的声明名有三个关键字：`let`​、`const`​、`var`​

* ​`let`​、`const`​的特点

  1. 块级作用域：只在当前函数下声明的变量有效，在代码块和`{ }`​括号之内有效
  2. 暂时性死区：在声明前无法使用该变量
  3. 不可重复声明
* ​`var`​的特点

  1. 存在变量提升：变量提升仅出现在`var`​定义的变量上。在预解析阶段，JS会搜索所有使用`var`​声明的变量，并**将声明**提升到**当前作用域最前面**。`var`​只是声明，并没有赋值
  2. 函数作用域：其作用域为该语句所在的函数，且存在变量提升(提升到函数的顶层)
  3. 可以重复声明
  4. 在未声明前使用，其值是`undefined`​

### 变量提升

1. 预解析：就代码执行之前的一个阶段，这个阶段完成的工作有很多，其中一个就是函数提升。

2. 函数提升：函数提升就是在预解析阶段，JS会搜索所有的函数的定义，并将定义提升最前面，体现在代码上就是书写代码时，函数调用可以放在定义之前。

3. 变量提升：变量提升仅出现在`var`​定义的变量上。在预解析阶段，JS会搜索所有使用`var`​声明的变量，并**将声明**提升到**当前作用域最前面**。`var`​只是声明，并没有赋值

4. 块级作用域：只在当前函数下声明的变量有效，在代码块和`{ }`​括号之内有效

5. 函数作用域：其作用域为该语句所在的函数，且存在变量提升(提升到函数的顶层)

```js
var的函数作用域，会提升到当前作用域的最顶部
function person(status) {
    if (status) {
        var value = "蛙人" 
    } else {
        console.log(value) // undefined
    }
    console.log(value) // undefined
}
person(false)

let的块级作用域
function person(status) {
    if (status) {
        let value = "蛙人" 
    } else {
        console.log(value) // 报错
    }
    console.log(value) // 报错
}
person(false)
```

### 块级作用域

```js
let声明的变量，只在 let 命令所在的代码块内有效
{
    let a = 10;
    var b = 20;
}
console.log(a); //a is not defined
console.log(b); //20
```

### 暂时性死区

```js
在代码块内，使用`let`命令声明变量之前，该变量都是不可用的。这在语法上，称为`暂时性死区`(temporal dead zone，简称 TDZ)

为什么需要块级作用域呢？
1.内层变量可能会覆盖外层变量
function foo(a){
    console.log(a);
    if(1===2){
        var a = 'hello 小马哥';
    }
}
var a = 10;
foo(a);
2.用来计数的循环遍历泄露为全局变量
var arr = []
for(var i = 0; i < 10; i++){
    arr[i] = function(){
        return i;
    }
}
console.log(arr[5]());
```

## 自执行函数

```js
;(function () {
    let num = 20 // 匿名函数内部声明的变量只能在匿名函数内部使用
    console.log(num)
})()
;(function () {
    console.log("789")
}())
```

## 作用域

1. 全局作用域(全局变量): 函数外部声明的变量
2. 局部（函数）作用域(局部变量): **函数内部**声明的变量
3. 块级作用域(块级变量): **分支或循环大括号里声明的变量**

注意：如果一个变量在声明的时候没有使用关键字。 例如： `num = 10`​,此时这个变量**无论写在哪里都是全局变量**。

```js
// 全局作用域
let num = 0

// 局部作用域
function fn1() {
    let num = 1
    console.log(num);
}
fn1() 

//块级作用域
for (let i = 0; i < 1; i++) {
    let num = 2
    console.log(num); //2
}

//块级作用域
if (true) {
    let num = 3
    console.log(num); //3
}
console.log(num); //0
```

## 作用域链

作用域链：由全局作用域与局部作用域形成的 "链条"

全局作用域：script标签、外部js文件

局部作用域：函数

```js
function fn(){
     function fn1(){
         function fn2(){
         }
     }
 }
```

​![image](assets/image-20220808213020-25o2lhv.png)​

链式查找规则：当一个作用域内使用一个变量时，如果当前作用域没有，则会向上一级作用域找。

```js
let a = 10
function fn(){
 let a = 200
 function fn1(){
     let a = 300 
     function fn2(){
         a = 20;
     }
     fn2();
 }
 fn1()
}
fn();
```

​![image](assets/image-20220808213223-rlmdo4q.png)​

## this

* ​`this`​指向

  ```js
  构造函数里的this:代表本次实例化出来的那个对象。
  方法里的this:代表调用方法的对象，也就是谁调用就代表谁。
  箭头函数的this:找上一级函数里的this
  普通函数的this:指向window
  ```
* 修改this指向：通过修改this的指向，达到方法的**借用**

  * call：将方法里的this修改为 obj 对象（this指向obj），并立即调用前面的函数。需要**独立传递参数**

    ```js
    方法.call(obj,参数1,参数2...)
    call会调用前面的方法，并将方法里的this修改为 obj 对象；参数1,参数2... 会传递给前面被调用的方法
    obj1.方法.call(obj2,参数1,参数2...):该方法的this指向obj2，如果obj2没有，即使obj1有，也会报undefined。
    let oa = {
      age: 20,
      say: function (a, b, c) {
        console.log(this.age);
        // console.log('hello');
        console.log(a, b, c);
      },
    };
    let ob = {
      age: 22,
    };
    let oc = {
      age: 25,
    };
    // oa.say()
    oa.say.call(ob, 10, 20, 30); //22,10,20,30
    oa.say.call(oc, "hello", "world", "abc"); //25,hello,world,abc
    ```
  * apply：将方法里的this修改为 obj 对象，并立即调用前面的函数。需要**传递数组**

    ```js
    方法.apply(obj,[参数1,参数2...])
    apply会调用前面的方法，并将方法里的this修改为 obj 对象
    apply方法在内部会将 [参数1,参数2]，解开 再传递给前面的方法。

    let oa = {
     age:20,
     say:function(a,b,c){
         console.log(this.age);
         console.log(a,b,c);
     }
    }

    let ob = {
     age:22
    }

    oa.say.apply(ob,[10,20,20])

    //应用
    let arr = [40, 19, 28, 30, 29, 19, 20, 49];

    // 展开传递
    // let max = Math.max(arr[0],arr[1],arr[2],arr[3],arr[4])
    // console.log(max);

    // 使用aplly就可以避免数组被展开传递
    // let max = Math.max.apply(Math,arr);
    // console.log(max);

    console.log(Math.max(...arr));
    ```
  * bind：不会立即调用前面的函数，会返回一个函数，返回的这个函数是this被obj替换后的一个函数，通过返回的函数可以调用前面的函数。

    ```js
    方法.bind( obj )
    // bind并不会立即调用前面的方法，只会将方法里的this替换为obj
    // bind会返回一个函数，返回的这个函数是this被obj替换后的一个函数，通过返回的函数可以调用前面的函数

    let oa = {
     age:20,
     say:function(a,b,c){
         console.log(ob.age);
     }
    }

    let ob = {
     age:22
    }

    // oa.say.call(ob)     // 22
    // oa.say.apply(ob)    //22
    // call与aplly在修改前面的方法里的this同时方法也会被一同调用。

    let fn = oa.say.bind(ob)// bind此时仅是使用ob替换oa.say里的this,并没有让say调用起来

    oa.say.bind(ob)//为函数
    fn();//22 这条语句才是调用
    ```

## 异常处理

* 创建异常

  ```js
  // new Error(错误描述)
  let er = new  Error('年龄不合法');
  console.log(er);//该对象本身
  //错误对象.message,从错误对象上获取错误信息
  console.log(er.message);//"年龄不合法"
  ```
* 抛出错误：仅仅创建一个错误对象，并不会触发这种严格错误（JS立即停止）。必须抛出错误，JS识别到了，才会停止代码执行。

  ```js
  //语法：throw 错误对象
  throw Error('年龄不合法')
  console.log('hello');//不执行，前面有错误了，执行中断
  ```
* 捕捉异常：当一个错误被抛出后，必须使用特殊的语法进行处理，不处理程序就会中断

  ```js
  try {
    let age = 210;
    if (age > 200) {
      throw Error("年龄不合法.....");
    }
  } catch (err) {
    console.log(err);//错误本身
    console.log(err.message);//"年龄不合法..." 错误对象包含的信息
  } finally {
    console.log("finllay....");//可以正常执行，不管try、catch 中是否 return 或者 再出错，这里面的代码还是会执行
  }
  console.log("hello");//可以正常执行，如果catch中还是有错误或return 这个代码就不会执行了
  ```

## 闭包

* 概念：闭包是`JS`​中一种特殊的代码，体现在代码上为：内部函数使用外层函数的局部变量。

  ```js
  函数是有作用域的，闭包在代码上就是函数嵌套函数，内部函数调用着这个局部作用域里面的局部变量。
  利用闭包相当于在内存开闭了一个区块，这个区块整体上是长效的，是一块‘独立’的内存，里面有自己设置的变量。
  就和一栋大楼，开辟了一个个不会产生相互影响的内存房间。
  函数是由其构成的本身语法和 构建时的环境 组成。这个环境包含了创建该函数时候创建的局部变量。
  ```
* 闭包作用：让函数所操作的数据私有化，常见用在((20220810230845-dr61av6 '节流防抖'))上面。

  ```js
  // 闭包的简单应用
  function outer(){
   let i = 0;
   function inner(){
     i++;
     console.log(i);
   }
   return inner;
  }

  let fn = outer(); // let i = 0 仅在调用外层函数时执行1次。

  fn();//1
  fn();//2
  fn();//3
  fn();//4
  fn();//5
  ```

## 节流防抖

* 节流：在一个时间间隔内触发多次，事件处理函数仅执行1次。节流主要用在高频事件中：鼠标移动、滚动条滚动 、页面resize

  ```js
  function throttle(handler,millsceconds){
    // 定义一个初始时间
    let t = 0
    return function(){
    // 事件触时的事件
    let n = Date.now()
    if(n - t > millsceconds){
      handler()
      //将本次事件处理函数执行的时间赋值给t
      t = n;
      }
    }
  }
  ```
* 防抖：在连续重叠的时间间隔内触发事件，以最后一次为准(从最后一次开始计时)。防抖主要用于搜索栏。

  ```js
  function debounced(handler, millseconds) {
    // 定义变量于存储定时器
    let timer;
    return function () {
      // 清除定时器的操作必须放在创建之前
      clearTimeout(timer);
      // 创建本次的定时器并保存到timer
      timer = setTimeout(handler, millseconds);
    };
  }
  ```

## 垃圾回收机制

* 垃圾回收机制：JS内部有一段代码，负责将程序执行的过程中，将没有办法访问到的数据销毁。
* 内存泄漏：如果有数据无法访问，但JS没有办法回收，这种情况就是内存"泄露"
* 引用计数：对数据的引用进行计数，计数为0，视为垃圾数据，垃圾回收机制会将其回收。

  ```js
  <script>
      // 当出现一个复杂数据类型（对象或数组）时，JS对这个数据记录它的引用次数。

     /*  // 引用次数： 1
      let obj = {
          id:10,
          name:'zhansan'
      }
      // 引用次数： 2
      let obj2 = obj;
      // 引用次数： 1
      obj2 = 'hello';
      // 引用次数： 0
      // 当一个数据引用次数变为0时，这个数据会被视之为"垃圾", 等待垃圾回收机制销毁
      obj = true; */
  </script>

  <script>
      // 引用记数有一个致命问题。
      // 数据{id:10, name:'zhangsan'}引用次数 1
      let obj = {
          id:20,
          name:'zhangsan'
      }
      // 数据{id:30, name:'lisi'}引用次数 1
      let obj2 = {
          id:30,
          name:'lisi'
      }
      // 数据{id:30, name:'lisi'}引用次数 2
      obj.to2 = obj2;
      // 数据{id:10, name:'zhangsan'}引用次数 2
      obj2.to1 = obj;

      obj = 10
      // 数据{id:10, name:'zhangsan'}引用次数 1
      obj2 = 20;
      // 数据{id:30, name:'lisi'}引用次数 1
      // 但是栈区已没有变量来引用这个数据
  </script>
  ```
* 标记清除：扫描使用过的内存空间, 看栈区是否有变量指向。如果没有则标记为垃圾，后续垃圾回收机制会将其回收。

  ```js
  <script>
   // 情况1： 局部变量
   //  当函数执行结束后，垃圾回收机制会将函数内的使用过局部变量，标记不垃圾，后续垃圾回收机制会将其回收。
   function fn(){
       let i = 10;
   }
   fn();
   // 情况2： 对象的相互引用
   //  垃圾回收机制 会扫描使用过的内存空间, 看栈区是否有变量指向，如果没有标记为垃圾，后续垃圾回收机制会将其回收。
  </script>
  ```

## 构造函数

构造函数就是函数，使用`new`​关键字调用，来创建对象的函数。如果一个函数使用 `new`​ 关键字调用，那么这个函数就是构造函数。

构造函数，是面向对象三大特性的`封装性`​的体现。可以将数据与操作数据的方法定义到一个主体上，方法内通过this来访问数据，实例化出来的每个对象上各自保留有自己的数据与操作数据的方法。

* 实例化

  * 由构造函数得到实例对象的过程就是实例化
  * 使用 `new`​ 关键字调用函数的行为被称为实例化
  * 实例化构造函数时没有参数时可以省略`（）`​
* 实例

  * 实例化的结果得到的是一个对象，这个对象通常称之为实例。
  * 通过构造函数创建的对象称为实例对象。
* 注意

  * **构造函数首字母一般大写**, 为了提醒调用者不要忘记`new`​关键字
  * 构造函数的返回值即为新创建的对象
  * 如果在构造函数内部手动`return`​

    * ​`return`​值类型：无效，还是返回new创建的对象
    * ​`return`​引用类型：有效，会覆盖new创建的对象

## 动态参数

* 概念：`argument`​：是用于接收函数实参的另一种方式，这种方式的特点可以不定义形参
* 注意：

  1. ​`argument`​**只能在函数内部使用;**
  2. ​`argument`​是一个伪数组;
  3. 有数组三要素(元素、下标、长度)，但是不能使用数组的方法，内部修改参数的值`arguments`​存储的值也会修改。
* 应用：一般用户参数数量不限的函数，如`arr.push()`​ 等函数实参数量不限，底层原理就是使用`arguments`​来接收所有的实参

  ```js
  function fn(a, b, c) {
    console.log(arguments) //1,2,3
    // 内部修改参数的值, arguments存储的值也会修改
    c = 10
    console.log(arguments) //1,2,10
  }
  fn()
  fn(1, 2, 3)

  ```

## 展开运算符

展开运算符符号：`...`​，可以用于将数组展开，展开成由逗号分隔的多个值。

也可以获取函数剩余的所有实参，一般情况下,可以替代动态参数`argument`​。

```js
1.获取函数参数
function fn(a, ...b) {
  console.log(b)
  console.log(arguments)
}
fn()
fn(1, 2, 3)

2.展开数组
let arr = [10, 20, 30, 40]      // ...arr      10, 20, 30, 40 
console.log(...arr);            // 10 20 30 40
// 求最大值与最小值
let arr = [9,10,28,17,32,15]
let max = Math.max(...arr)
console.log(max);

let min = Math.min(...arr)
console.log(min);
```

## 定时器

* ​`setTimeout(callback):timer`​：一次性定时器当指定的毫秒数到达时，才执行指定的函数

  ```js
  setTimeout(()=>{},time)
  //设置一次性定时器，返回定时器标识
  let timer = setTimeout(function(){
    console.log('hello');
  },3000);
  //清除一次性定时器
  clearTimeout(timer);
  ```
* ​`setInterval(callback):timer`​：重复定时器，每间隔指定的毫秒数，就执行一次指定的函数

  ```js
  setInterval(()=>{},tiem)
  //设置重复定时器，返回定时器标识
  let timer = setInterval(function(){
    console.log('hello');
  },2000);
  //清除重复定时器
  clearInterval(timer);
  ```

## 可选链操作符

* [概念](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Optional_chaining)：允许读取位于连接对象链深处的属性的值，而不必明确验证链中的每个引用是否有效。
* 使用：利用点语法，会先判断问号前面是否存在，存在就可以点，不存在就`nudefined`​

  ```js

  obj?.prop       // obj.dog?.name
  obj?.[expr]     // obj.dog?.['name']
  arr?.[index]    // arr = null   arr?.[0]
  func?.(args)    // fn?.()
  ```
* 非空断言：**可选链操作符不能放在 赋值操作符= 的左边**

  ```js
  ts中使用可选链操作的时候可能放在 = 的左边

  const imgRef = ref<HTMLImageElement | null>(null)
  onMounted(() => {
    imgRef.value!.src = '2.jpg'//我肯定是有的 !. 非空断言
  })
  ```

## 空值合并操作符

空值合并操作符：**当左侧值为** ***null*** **或** ***undefined*** **时，返回 ?? 符号右边的值**

```js
a = 0
b = 2
a ?? (b=3) //虽然0会隐式转成false，但是还是执行左边的，因为 ?? 只有左边是null或者undefined才会执行右边，这样的话，可以避免 0 这种数据被转换的导致逻辑有误？
b//b:2
-------
a = nudefined
b = 2
a ?? (b=3) 
b//b:3
```

## 检测数据类型

* ​`typeof`​：一元运算符，它返回一个字符串，说明运算数的类型

  ```js
  typeof xxx 或 typeof(xxx)

  const type =  typeof '你好'; // string
  typeof 666; // number
  typeof true; // boolean
  typeof undefined; // undefined
  typeof Symbol(); // symbol
  typeof 1n; // bigint
  typeof () => {}; // function

  typeof []; // object
  typeof {}; // object
  typeof new String('xxx'); // object

  typeof null; // object

  typeof只能准确判断原始数据类型和函数（函数其实是对象，并不属于另一种数据类型，但也能够使用 typeof 进行区分），无法精确判断出引用数据类型（统统返回 object）。
  有一点需要注意，调用typeof null返回的是object，这是因为特殊值null被认为是一个对空对象的引用（也叫空对象指针）
  ```
* ​`instanceof`​：作用于判断构造函数与实例对象的关系，依据是构造函数对应的原型对象是否在以实例为起点的原型链上。

  ```js
  //对象 instanceof 函数 ,返回值是boolean
  function Human(){}
  function Person(){}
  let oa = new Person();
  console.log(oa instanceof Person);   // true
  console.log(oa instanceof Human);    // false
  console.log(oa instanceof Object);   // true
  ```
* 区别

  ```js
  //typeof 虽然可以判断原始数据类型（null 除外），但是无法判断引用数据类型（function 除外）
  //instanceof 可以准确判断引用数据类型，但是不能正确判断原始数据类型
  //typeof 会返回一个运算数的基本类型，instanceof 返回的是布尔值

  Object.prototype.toString({})       // "[object Object]"
  Object.prototype.toString.call({})  // 同上结果，加上call也ok
  Object.prototype.toString.call(1)    // "[object Number]"
  Object.prototype.toString.call('1')  // "[object String]"
  Object.prototype.toString.call(true)  // "[object Boolean]"
  Object.prototype.toString.call(function(){})  // "[object Function]"
  Object.prototype.toString.call(null)   //"[object Null]"
  Object.prototype.toString.call(undefined) //"[object Undefined]"
  Object.prototype.toString.call(/123/g)    //"[object RegExp]"
  Object.prototype.toString.call(new Date()) //"[object Date]"
  Object.prototype.toString.call([])       //"[object Array]"
  Object.prototype.toString.call(document)  //"[object HTMLDocument]"
  Object.prototype.toString.call(window)   //"[object Window]"
  ```

## 数据类型转换

* 概念：`JavaScript`​是弱数据类型，`JavaScript`​也不知道变量到底属于那种数据类型，只有赋值了才清楚
* 转number类型

  ```js
  // 1. 字符串转数字类型: parseInt(), parseFloat()
  // 2. 非字符串转数字类型: Number()
  // parseInt() 转整数, 从前往后去运行, 保留整数部分, 遇到不能转换的, 就舍弃,如果一开始就是非数值部分, 转为NaN
  console.log(parseInt("8.8")) // 8
  console.log(typeof parseInt("8.8")) // number
  console.log(parseInt("8.8a")) // 8
  console.log(parseInt("88a")) // 88
  console.log(parseInt("a88")) // NaN
  //parseFloat() 转小数, 可以保留小数部分
  console.log(parseFloat("8.8")) // 8.8
  console.log(parseFloat("8.8a")) // 8.8
  console.log(parseFloat("a8.8")) // NaN
  //Number()  非字符串类型转为数字使用比较方便
  // 转字符串时注意, 有非数值部分就报NaN
  console.log(Number("8.8")) //8.8
  console.log(Number("8.8a")) //NaN
  console.log(Number(true)) //1
  console.log(Number(false)) //0
  console.log(Number(undefined)) //NaN
  console.log(Number(null)) //0
  ```
* NaN：number类型特殊值，与任何数值进行运算都是NaN

  ```js
  console.log(Number("8.8a")) // NaN
  let num
  console.log(num + 1) //NaN。NaN不能参与运算的, 与任何数值进行运算都是NaN

  // isNaN(数据) 返回true, 意味着数据为NaN,  返回false, 说明数据正常
  // 注意,会有隐式转换, 数据会先用Number()转为数值类型再去判定
  console.log(isNaN("123")) // Number('123') => 123  结果为fasle
  console.log(isNaN("abc")) // Number('abc') => NaN  结果为true
  console.log(isNaN("字符串")) // Number('字符串') => NaN  结果为true
  console.log(isNaN(NaN)) // true
  console.log(isNaN("123abc" - "123")) //得到的结果为NaN, 返回true
  ```
* 转string类型

  ```js
  // 1. String() 常用
  // 2. 变量.toString()   undefined和null 不能使用, 会报错
  // 使用场景: 转换进
  console.log(String(123)) // '123'
  console.log(typeof String(123)) // string
  console.log(String(true)) // 'true'
  console.log(String(false)) // 'false'
  console.log(String(undefined)) // 'undefined'
  console.log(String(null)) // 'null
  // 变量.toString(), undefined和null不能使用此方法会报错
  let num = 16
  console.log(num.toString()) //'123
  // 使用场景: 进制转换
  // 十六进制, 每一位0-9,a-f之间
  console.log(num.toString(16)) //'2'
  // 二进制, 每一位0-1
  console.log(num.toString(2)) //'10'
  ```
* 转boolean类型：`0`​, `-0`​, `''`​, `undefined`​, `null`​, `NaN`​, `false`​7种数据会转为false，其他都是`true`​

  ```js
  console.log(Boolean(0))
  console.log(Boolean(-0))
  console.log(Boolean("")) // 空字符串,不能加空格
  console.log(Boolean(undefined))
  console.log(Boolean(null))
  console.log(Boolean(NaN))
  console.log(Boolean(false)
  console.log(Boolean(-1)) // true
  console.log(Boolean("寂寞的天")) // true
  ```
* 隐式转换：代码执行时，系统对处理的数据自动进行数据类型转换

  ```js
  //显示转换: 程序员主动做的, 阅读性高
  //转数值类型: Number()  parseInt() parseFloat()
  //转字符串类型: String()  变量.toString()
  //转布尔类型: Boolean()

  //隐式转换 ： 
  //当数据不能直接计算, 或者运算符两边的 ‘数据类型不一致’ 的时候，编译器会转成一致后运算

  //转数值类型  有算术运算符的时候, 会发生数值类型转换
  console.log("5" * "3") // Number('5') * Number("3") ==> 5*3      15
  //console.log("5a" * "3") // Number('5a') * Number("3") ==> NaN*3    Na

  //转字符串类型  +为连接符功能时
  console.log("这是一个寂寞的天" + 1) // '这是一个寂寞的天1'
  console.log("" + 1) // '1'

  //转布尔类型  逻辑非
  console.log(!5) // !Boolean(5)  false
  console.log(!0) // !Boolean(0)  true
  ```

## 数据解构

* 概念：解构数据的时候，数据结构上要是一致的，才能赋值到自己设置的变量
* **注意**：如果解构赋值给的数据是`undefined`​，那么会优先使用自己的默认（原本）的值
* 数组解构

  ```js
  let [x,y] = [10]
  console.log(x);// 10
  console.log(y);// nudefiend   数组不够分


  //数组解构层级
  let [a, [b1, b2]] = [10,['hello','world']]
  console.log(a);  //10
  console.log(b1, b2);   //'hello','world'

  let [, b1] = [10, ['hello', 'world']]//
  // console.log(b1);//['hello', 'world']


  let arr = [10, 20, 30, 40];

  1. 按需解构
  let [a,b] = arr;  // 解构前两个
  console.log(a, b);

  let [a,,b] = arr;    // 解构第1个与第3个
  console.log(a,c);

  2. 解构中也可以使用剩余参数
  let [a, ...b] = arr;
  console.log(a);  //10
  console.log(b);  //[20,30,40]

  let arr2 = [10, ['hello', 'world']]
  let [a,[b1,b2]] =  [10, ['hello', 'world']]
  console.log(a);  //10
  console.log(b1,b2);  //'hello', 'world'
  ```
* 对象解构：将对象中与变量同名的成员读取出来，并赋值给同名变量。如果没有找到同名的则返回`undefined`​。**真正被赋值的是后者，而不是前者。**

  ```js
  //基本语法:let {变量1, 变量2, 变量3} = 对象
  //解构重命名:let {原名:新名, 原名:新名, 原名:新名} = 对象

  //基本解构
  let obj = {
   id:10,
   age:20,
   gender:'男'
  }
  let {age, id, gender,uname} = obj;
  console.log(id, age, gender,uname);


  //解构重命名
  let obj = {
   id:10,
   age:20,
   gender:'男'
  }
  let {id:i,age:a,gender:g} = obj;
  console.log(i, a, g);

  //多级对象解构
  let arr2 = [
      {
          uname:'zhangsan',
          age:20,
          car:{
              price:'50w',
              color:'red'
          },
          phone:{
              price:'1w',
              color:'blue'
          }
      }
  ]

  let [{uname,age,car,car:{price,color}}] = arr2;
  console.log(uname,age,car,price,color);

  ```

## 创建对象方法

* 构造函数法：配合 `new`​ 关键字使用  

  1. 构造函数首字母一般大写, 为了提醒调用者不要忘记 `new `​关键字
  2. 如果在构造函数内部手动`return`​：简单数据类型， 无效，还是返回创建的对象；引用类型，有效，会覆盖new创建的对象

  ```js
  /* 
  new关键字创造对象的过程: 
          (1)创建空对象  {}
          (2)this指向这个对象  this = {}
          (3)对象赋值  this.name = name
          (4)返回这个对象  return this
  this: 代表当前对象的一个引用, 会随着调用者变化而改变  
  */
  function Person(name, sex) {
    this.name = name
    this.sex = sex
    this.sayHi = function () {
      alert(`大家好! 我是${name}`)
      alert("大家好! 我是" + this.name)
    }
  }
  let obj1 = new Person("张三", "man")
  console.log(obj1)
  obj1.sayHi()
  ```
* 内置对象法

  ```js
  // 内置对象法声明对象 new Object()
  // 与字面量声明效果相同, 使用对象属性新增方式添加属性和方法
  let obj = new Object()
  obj.name = "张三"
  obj.sex = "man"
  obj.sayHi = function () {
      alert("大家好!")
  }
  console.log(obj) 

  // 将内置对象法创建对象的过程封装成为一个函数, 每次创建一个人就调用一次, 也可以批量创建对象
  function person(name, sex) {
      let obj = new Object()
      obj.name = name
      obj.sex = sex
      obj.sayHi = function () {
          alert(`大家好!我是${name}`)
      }
      return obj
  }

  let obj1 = person("张三", "man")
  console.log(obj1)
  let obj2 = person("李四", "woman")
  console.log(obj2)
  ```
* 工厂函数法创建对象

  ```js
  //工厂函数:用于创建对象的函数
  //工厂函数:就是把字面量创建对象的过程封装成函数, 可以像工厂的流水线一样批量创建对象
  function createPerson(name, age, sex) {
      //1.创建对象
      let p = {}
      //2.给对象赋值
      p.name = name
      p.age = age
      p.sex = sex
      //3. 返回对象
      return p
  }
  let p1 = createPerson("张三", 20, "男")
  let p2 = createPerson("李四", 22, "女")
  console.log(p1, p2)
  ```

‍

## 原型对象

在`JS`​中只要有一个函数存在，JS就会创建一个与之对应的对象。这个对象就是原型对象。默认只有一个`constructor`​属性

原型对象可以说是解决了内存浪费的问题，构造函数所创建的对象的共享方法放在原型对象里面，这样可以节约内存空间

### 构造函数、原型对象、实例对象

```js
构造函数.prototype    指向  原型对象
实例对象.__proto__    指向  原型对象
原型对象.constructor  指向  构造函数
实例对象.__proto__.constructor 指向 构造函数
构造函数、实例对象没有直接链接关系，要通过原型对象来中转
```

### 原型继承

原型继承：也称之为替换原型继承

主要思想是使用父的实例对象来替换子的原型对象。实现了一个对象可以使用另一个对象的属性和方法。

用该构造函数创建的实体来替换成所需要函数的原型对象。

就是说，利用函数实例化的对象，给需要的函数统一赋予所需要的方法和属性

```js
替换之前实例化的对象，访问之前的原型对象上的方法。
替换之后实例化的对象，访问之后的原型对象上的方法。
function Fn(){}
// 向默认的原型对象上添加成员
Fn.prototype.run = function(){
 console.log('我跑的很快...');
}
let oA = new Fn();
// oA.run();
// 创建一个对象，把fn默认的原型对象替换掉
let obj = {}
// ---------------------------
Fn.prototype = obj
let oB = new Fn();
oA.run();//我跑很快
oB.run();//undefined   替换了，没有该方法了
```

### 原型链

* 概念：原型对象形成的一个链条。**原型链是通过__proto__形成的链接**。
* 原型链查找规则：就近原则

  ```js
  原型的作用是当实例对象上访问某个成员不存在时，则会到原型对象上找。
  而原型链是当原型对象上访问某个成员不存在时，会到原型对象的原型对象上找，直到null

  function Person(){
  }

  let p = new Person();
  // 一级原型
  console.log(p.__proto__);
  // 一级原型对象的构造函数是Person
  console.log(p.__proto__.constructor === Person);

  // 二级原型
  console.log(p.__proto__.__proto__);
  // 二级原型，对象的构造函数是Object
  console.log(p.__proto__.__proto__.constructor === Object);

  // Object构造函数对应的原型对象，是其他实例对象的二级原型对象
  console.log(Object.prototype === p.__proto__.__proto__);

  // 二级原型的原型
  console.log(p.__proto__.__proto__.__proto__);

  //二级原型再向上找则是null
  ```

## 装饰器

修饰器本质是个函数，可以用于修饰类、属性、方法，装饰器其实是js中的内容

装饰器的运行时机：在装饰器使用的时候就已经运行了

装饰器的运行顺序：从下到上（实际被装饰的对象实在最下面）

### 类装饰器

类装饰器接收一个参数，该参数表示类本身（即构造函数）

```javascript
//虽然class的本质是函数，但是ts是不知道这个是个类函数函数
//如果说该装饰器是类装饰器，为了告诉ts该参数是类，需要如下声明
function test1(target:new (...args:ang [])=>object){ // ...args 是剩余参数，是个数组
  ...
}

function test2(){ // ...args 是剩余参数，是个数组
  return function(target:new (...args:ang [])=>object){
    ...
  }
}

@test
class class1{}


@test()//装饰器也可以主动调用，因为他本身就是函数，类装饰器主动调用的情况，需要有显示的返回函数
//这种叫做装饰器工厂，执行之后返回装饰器,还可以自己传递参数进去
class class2{}


```

### 成员装饰器

1. 属性装饰器

    1. 属性装饰器也是一个函数，该函数有三个参数
    2. 第一个参数：如果是静态属性，则是**类本身**，如果是成员属性，则是**类的原型对象**
    3. 第二个参数：固定是一个字符串，是属性的key值
    4. 第三个参数：描述符对象
2. 方法装饰器

# 内建类

## String

### str.startsWith

用来判断当前字符串是否以另外一个给定的子字符串开头，并区分大小写，返回值是`true`​ 或 `false`​

```js
str.startsWith(searchString[, position])
searchString：要搜索的子字符串。
position可选：在 str 中搜索 searchString 的开始位置，默认值为 0。

const str1 = 'Saturday night plans';
console.log(str1.startsWith('Sat'));//true
console.log(str1.startsWith('Sat', 3));//false
console.log(str1.startsWith('Sat',0));//true
```

### str.endsWith

用来判断当前字符串是否以另外一个给定的子字符串“结尾”的，并区分大小写，返回值是`true`​ 或 `false`​

```js
str.endsWith(searchString[, length])
searchString：要搜索的子字符串。
length 可选：作为 str 的长度。默认值为 str.length。
已长度为最后截至部分，来判断是否符合。


const str1 = 'Cats are the best!';
console.log(str1.endsWith('best!'));//true
console.log(str1.endsWith('best', 17));//true

const str2 = 'Is this a question?';
console.log(str2.endsWith('question'));//false
```

### str.includes

方法用于判断一个字符串是否包含在另一个字符串中，返回值是`true`​ 或 `false`​

```js
str.includes(searchString[, position])
searchString：要在此字符串中搜索的字符串。
position 可选：从当前字符串的哪个索引位置开始搜寻子字符串，默认值为 0。

let url = 'http://www.xxx.com/admin/index.html';
console.log(url.includes('.com'));//true
console.log(url.includes('.com',15))//false 从c开始，没有 . 所有false
```

### str.padStart

方法用另一个字符串填充当前字符串 (如果需要的话，会重复多次)，以便产生的字符串达到给定的长度。从当前字符串的左侧开始填充。

```js
str.padStart(targetLength [, padString])
targetLength：当前字符串需要填充到的目标长度。如果这个数值小于当前字符串的长度，则返回当前字符串本身。
padString可选：填充字符串。如字符串太长，填充后的字符串长度超过了目标长度，则保留最左侧的部分，其他部分会被截断。默认值为 ""

let str = "0";
console.log(str.padStart(5, "0"));//00000
console.log("123".padStart(5, "0"));//00123
```

### str.padEnd

方法会用一个字符串填充当前字符串（如果需要的话则重复填充），返回填充后达到指定长度的字符串。从末尾（右侧）开始填充。

```js
str.padEnd(targetLength [, padString])
targetLength：当前字符串需要填充到的目标长度。如果这个数值小于当前字符串的长度，则返回当前字符串本身。
padString 填充字符串。如字符串太长，填充后的字符串长度超过了目标长度，则保留最左侧的部分，其他部分会被截断。默认值为 ""

let str = "0";
console.log(str.padEnd(5, "0"));//00000
console.log("123".padEnd(5, "0"));//12300
```

### str.split

使用指定的分隔符字符串将一个String对象分割成子字符串数组，以一个指定的分割字串来决定每个拆分的位置。不改变原字符串。

```js
//字符串.split(分割符)
//可以使用正则

let str = 'one-tow-three';
let arr = str.split('-');
console.log(arr);//['one', 'tow', 'three']
console.log(str.split(''));//['o', 'n', 'e', '-', 't', 'o', 'w', '-', 't', 'h', 'r', 'e', 'e']
console.log(str);//one-tow-three
```

### str.substring

在字符串中，读取从开始位置至结束位置之间的字符，含头不含尾，不改变原字符串

```js
str.substring(indexStart[, indexEnd])
indexStart：需要截取的第一个字符的索引，该索引位置的字符作为返回的字符串的首字母。
indexEnd可选：一个 0 到字符串长度之间的整数，以该数字为索引的字符不包含在截取的字符串内。


var anyString = "Mozilla";

// 输出 "Moz"
console.log(anyString.substring(0,3));
console.log(anyString.substring(3,0));
console.log(anyString.substring(3,-3));
console.log(anyString.substring(3,NaN));
console.log(anyString.substring(-2,3));
console.log(anyString.substring(NaN,3));

// 输出 "lla"
console.log(anyString.substring(4,7));
console.log(anyString.substring(7,4));

// 输出 ""
console.log(anyString.substring(4,4));

// 输出 "Mozill"
console.log(anyString.substring(0,6));

// 输出 "Mozilla"
console.log(anyString.substring(0,7));
console.log(anyString.substring(0,10));

// 字符中也有下标的概念
console.log(url[0]);// h
console.log(url[1]);// t
console.log(url[2]);// t

console.log(url.substring(7, 21));// www.baidu.com/
console.log(url)// http://www.baidu.com/admin/index.html
```

### str.toUpperCase

调用该方法的字符串转为大写形式并返回（如果调用该方法的值不是字符串类型会被强制转换）。

```js
str.toUpperCase()//点语法，调用str原型对象的方法。不改变原字符串

const str= 'The quick brown fox jumps over the lazy dog.';
console.log(str.toUpperCase());//"THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG."
```

### str.toLowerCase

调用该方法的字符串转为小写形式并返回（如果调用该方法的值不是字符串类型会被强制转换）。

```js
str.toLowerCase()//点语法，调用str原型对象的方法。不改变原字符串

console.log( "ALPHABET".toLowerCase() );// "alphabet"
```

### str.indexOf

功能：获取某一个字符第一次出现的下标（判断一个字符串在一个另一个字符串中）

```js
str.indexOf(searchValue [, fromIndex])
searchValue ：要被查找的字符串值，没写，代表'undefined'（是字符串'undefined'）
fromIndex 可选：数字表示开始查找的位置，默认值为 0。
fromIndex 的值小于 0，或者大于 str.length ，那么查找分别从 0 和str.length 开始。
fromIndex 的值小于 0，等同于为空情况； fromIndex 的值大于或等于 str.length ，那么结果会直接返回 -1 。）
注意：不支持正则

var str = 'abcdefgabcd';
console.log ( str.indexOf ( "b" ) );//1  如果存在则返回下标
console.log ( str.indexOf ( "z" ) );//-1  如果不存在则返回固定值-1
```

### str.lastIndexOf

返回调用String对象的指定值最后一次出现的索引，在一个字符串中的指定位置 `fromIndex`​处**从后向前搜索**。如果没找到这个特定值则返回-1 。

```js
str.lastIndexOf(searchValue[, fromIndex])
searchValue：一个字符串，表示被查找的值。如果searchValue是空字符串，则返回fromIndex。
fromIndex可选：待匹配字符串 searchValue 的开头一位字符从 str 的第 fromIndex 位开始向左回向查找。
fromIndex默认值是 +Infinity。如果 fromIndex >= str.length ，则会搜索整个字符串。如果 fromIndex < 0 ，则等同于 fromIndex == 0。

'canal'.lastIndexOf('a'); 
// returns 3（没有指明fromIndex则从末尾l处开始反向检索到的第一个a出现在l的后面，即index为3的位置）

'canal'.lastIndexOf('a', 2);  
// returns 1（指明 fromIndex 为 2 则从 n 处反向向回检索到其后面就是 a，即 index 为 1 的位置）

'canal'.lastIndexOf('a', 0); 
// returns -1(指明 fromIndex 为 0 则从 c 处向左回向检索 a 发现没有，故返回-1)

'canal'.lastIndexOf('x'); // returns -1
'canal'.lastIndexOf('c', -5);// returns 0（指明 fromIndex 为-5 则视同 0，从 c 处向左回向查找发现自己就是，故返回 0）
'canal'.lastIndexOf('c', 0); // returns 0（指明 fromIndex 为 0 则从 c 处向左回向查找 c 发现自己就是，故返回自己的索引 0）
'canal'.lastIndexOf(''); // returns 5
'canal'.lastIndexOf('', 2); // returns 2
```

### str.replace

把字符串中的字符替换成想要的字符，不改变原字符串本身。

```js
str.replace('subStr ','newSubStr')
subStr ：希望被替换的字符
newSubStr：用于替换的字符
注意：
str.replace只会替换一次成功后就直接返回，不会整个字符串进行替换，需要整个替换可以使用正则的严格模式。/  /g

const p = '1223334444'
const p2 = p.replace('3','5')
console.log(p);//'1223334444'
console.log(p2)；//'1225334444' 替换一个就返回，不改变原字符串

const i = /4/;  // 正则表达式
const p3 = p.replace(i,'5')
console.log(p3);//1223335444  正则非严格模式，也是只替换一个

const j = /4/g;
const p4 = p.replace(j,'5')
console.log(p4);//11223335555
```

### str.trim

去掉字符串的左右空格，不会改变原字符串

```js
str.trim（）
系统自带的trim方法只能去掉左右空格,无法去掉中间
要去掉所有的空格，使用str.replace（）方法，利用正则的严格模式去掉

var str = '   safga  sdv  dsfgs    ';
console.log ( str.trim () )//safga  sdv  dsfgs

//去掉所有空格思路：使用正则全局匹配空格`\s`,替换成空字符
console.log ( str.replace ( /\s/g, "" ) );//safgasdvdsfgs
```

### str.search

返回第一次和**正则匹配**的索引，如果没有则返回-1

```js
str.search(regexp)
regexp：一个正则表达式对象。如果传入一个非正则表达式对象 regexp，则会使用 new RegExp(regexp) 隐式地将其转换为正则表达式对象。


var str = 'abcdefg100xyz';
console.log ( str.search ( /\d/g ) );//7   此方法会忽略全局匹配g
```

### str.match

提取字符串中与**正则表达式**相匹配的文本，返回值是数组,找不到则返回null

```js
str.match(regexp)
regexp：一个正则表达式对象。如果传入一个非正则表达式对象，则会隐式地使用 new RegExp(obj) 将其转换为一个 RegExp 。
如果你没有给出任何参数并直接使用 match() 方法 ，你将会得到一 个包含空字符串的 Array ：[""] 。
返回值：
如果使用 g 标志，则将返回与完整正则表达式匹配的所有结果，但不会返回捕获组。
如果未使用 g 标志，则仅返回第一个完整匹配及其相关的捕获组（Array）。

var str = 'abcdefg100xyzabc';
//返回值是数组,找不到则返回null
console.log ( str.match ( /a/ ) );//["a", index: 0, input: "abcdefg100xyzabc", groups: undefined]
console.log ( str.match ( /a/g ) );// ["a", "a"]    此方法支持全局匹配g
```

## Number

### number.toFixed

功能：对数值保留指定小数倍，并四舍五入

```js
//要保证number类型
let price = 10
console.log(price.toFixed(2));//10.00
let num = 10.456;
console.log(num.toFixed(2));//10.46
```

### number.toString

功能：可以进行进制转换，结果使用字符串表示；如果省略进制，则表示转换为10进制

```js
let n = 10;
console.log(n.toString(2));//1010
console.log(n.toString(8));//12
console.log(n.toString(16));//a
console.log(n.toString());//'10'
```

## Array

### 基本方法

```css
app.push()//数组后面添加
arr.pop()//数组后面删除
arr.unshift()//数组前面添加
arr.shift()//数组前面删除
```

### arr.toString()

将数组变成内容变成字符串，不改变原数组。

```js
arr1 = [1, 2, 3,]

console.log(arr1.toString());  //‘1,2,3’
console.log(arr1);  //[1, 2, 3,]

arr.join('指定连接符号') 可以选定连接符号
console.log(arr1.join("--")) //'1--2--3'
```

### arr.join()

将数组内容进行拼接，返回一个字符串，不改变原数组

```js
arr.join('指定连接符号') 可以选定连接符号
arr = [1, 2, 3,]
console.log(arr.join("--"));//1--2--3
console.log(arr1);// [1, 2, 3,]
```

### arr.concat()

合并两个或多个数组。不会更改现有数组，而是返回一个新数组。

```js
let arr =  [1, 2, 3]
let arr2 = [4, 5, 6]
let arr3 = [7, 8, 9]
console.log(arr1.concat(arr2, arr3)) // [1, 2, 3, 4, 5, 6, 7, 8, 9]
console.log(arr1)// [1, 2, 3]
```

### arr.reverse()

将数组中元素的位置颠倒，并返回该数组。该方法会改变原数组。

```js
let arr1 = [1, 2, 3]
console.log(arr1.reverse()) // [3,2,1]
console.log(arr1) // [3,2,1]
```

### arr.sort()

对数组进行排序，并返回数组，会改变原数组。

默认排序顺序是在将元素转换为字符串，然后比较它们的 UTF-16 代码单元值序列时构建的。

```js
let arr1 = [1, 5, 2, 4, 3]

// 不传参时按照默认顺序排列
console.log(arr1.sort())//[1, 2, 3, 4, 5]
console.log(arr1)//[1, 2, 3, 4, 5]

//可以传入函数控制数组排序顺序
function fn(a, b) {
        //return a-b //正序(默认顺序)
    return b - a //倒序(数字倒排)
}
console.log(arr1.sort(fn))
```

### arr.splice()

通过**删除**或**替换**现有元素或者原地**添加**新的元素来修改数组，并以**数组形式返回被修改的内容**。此方法会改变原数组。

```js
1.数组删除
//arr.splice(起始位置, 删除个数) 
//其实位置从0开始，从起始位置开始删除，返回值是被删除的元素组成的数组
let arr = [1,2,3,4,5,6]
console.log(arr.splice(1, 2)) // [2, 3]
console.log(arr) //[1, 4, 5, 6]

2.数组添加
//arr.splice(起始位置, 删除个数,新增元素...) 
//删除个数写0，就是在起始位置前面面新增元素
let arr = [1,2,3,4,5,6]
console.log(arr.splice(1, 0,'插入元素1','插入元素2')) // []
console.log(arr) //[1, '插入元素1', '插入元素2', 2, 3, 4, 5, 6]

3.数组替换
//arr.splice(起始位置, 删除个数,新增元素...) 
//删除位置这里写数字，就是几个元素被后面的新增元素给替换了
let arr = [1,2,3,4,5,6]
console.log(arr.splice(1, 2,'插入元素1','插入元素2')) // [2, 3]
console.log(arr) //[1, '插入元素1', '插入元素2', 4, 5, 6]
```

### arr.slice()

返回一个新的数组，这一数组是一个由 `begin`​ 和 `end`​ 决定的原数组的 **浅拷贝** （包括 `begin`​，不包括`end`​）。原始数组不会被改变。

```js
//slice(起始, 结束(不包括))   如果不写结束参数, 就直接截取到结束
let arr= [1,2,3,4,5,6]
console.log(arr.slice(1, 3)) //[2, 3]
console.log(arr.slice(1)) //[2, 3, 4, 5, 6]
console.log(arr) //[1, 2, 3, 4, 5, 6] 
```

### arr.filter()

遍历数组，将符合条件的数组元素返回，组成新数组，没有符合条件的元素则返回空数组。不改变原数组

```js
//  arr.filter(function(item){return item})
let arr1 = [10, 20, 30, 0, -20, -5, 60]
const arr2 = arr1.filter( (item) =>{
  return item > 0
  })

console.log(arr1)//[10, 20, 30, 0, -20, -5, 60]
console.log(arr2)//[10, 20, 30, 60]
```

### arr.forEach()

用于对前面的数组进行遍历操作，次对数据进行遍历时，都会调用一次回调函数，**return不会中断对数组的遍历过程**

```js
// 数组.forEach( (item,index,arr)=>{} )
//item是每次遍历到的元素的值
//index是每次遍历到的元素的下标
//arr就是遍历的数据(实际意义不大)
let arr = [10, 9, 28, 19, 14, 20];
arr.forEach(function (item, index, arr) {
  console.log(item);
  console.log(index);
  console.log(arr);
});

```

### arr.reduce()

对数组中的每个元素按序执行一个**reducer**函数，每一次运行**reducer**会将**先前元素的计算结果作为参数传入**，最后将其结果汇总为单个返回值。

第一次执行回调函数时，不存在“上一次的计算结果”。如果需要回调函数从数组索引为 0 的元素开始执行，则需要传递初始值。否则，数组索引为 0 的元素将被作为初始值，迭代器将从第二个元素开始执行（索引为 1 而不是 0）。

```js
// 数组.reduce((prev, item, index, arr)=>{}, 初始值 )
// prev参数是上次调用function(){}时的返回值
// 由于第1次调用是没有上一次的返回值，所以reduce会将 初始值 传递第1次调用 function(){}时的pre参数
// 如果省略初始值reduce会从第2个元素开始遍历数组，同时将第1个元素的值作为初始值传递给pre，不推荐省略

let arr = [100, 15, 9, 18, 12];

let ret = arr.reduce( function(pre, item){
    return pre + item;  
}, 0)
console.log(ret);//154

// 省略初始值  不推荐,虽然系统会自己给初始值
let ret2 = arr.reduce(function (pre, item) {
  return pre + item;
});
console.log(ret2);//154
```

### arr.find()

用于查找满足条件的第1个元素并返回，如果没有满足条件的元素则返回`undefined`​。

```js
// 数组.find(function(item,index,arr){
//     return 条件;
// })

let arr = [10, 15, 9, 18, 8, 12];
let ret = arr.find(function (item) {
  return item < 10;
});
console.log(ret);//9 返回第一个满足这个条件的值
```

### arr.findIndex()

返回数组中满足提供的测试函数的第一个元素的索引 。若没有找到对应元素则返回-1。

```js
// 数组.findIndex(function(item,index,arr){
//     return 条件;
// })
const arr1 = [5, 12, 8, 130, 44];
const isLargeNumber = (item) => item> 13;//返回元素大于13
console.log(arr1.findIndex(isLargeNumber));//3
```

### arr.every()

判断数组中是否所有的元素都满足指定的条件，所有的元素必须都满足才返回true。

```js
// 数组.every(function(item,index,arr){
//     return 条件
// })

// 需求：判断是否所有元素都是偶数
let arr1 = [10, 20, 30, 40, 10];
let res = arr1.every(item=>item% 2==0);
console.log(res);//true
```

### arr.some()

判断数组中是否所有的满足指定的条件的元素，只要有元素满足就返回true

```js
// 数组.some(function(item,index,arr){
//     return 条件
// })

// 需求：判断是否有奇数元素
let arr2 = [10, 9, 20, 30, 40, 10];
let ret = arr2.some((item) => item % 2 == 1);
console.log(ret);//true

```

### arr.includes()

用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 `true`​，否则返回 `false`​。

注意：**includes只可以判断简单数据类型**。它不像`arr.some`​那样，有`item`​可以使用点语法。

```js
const array1 = [1, 2, 3];
console.log(array1.includes(2));//true

const pets = ['cat', 'dog', 'bat'];
console.log(pets.includes('cat'));//true
console.log(pets.includes('at'));//false

const arr = [{a:1},{b:2}]
console.log(arr.includes({a:1}))//false
```

### arr.map()

创建一个新数组，这个新数组由原数组中的每个元素都调用一次提供的函数后的返回值组成。

```js
const arr = [1, 4, 9, 16];
const map = arr.map((item,index) => item * 2);//每个元素*2后当作新数组的元素返回
console.log(map);//[2,8,18,32]
```

## Object

### Object.keys()

作用：用于获取指定对象所有的成员名，返回数组

注意：会返回带有 `enumerable`​ 标志的非 symbol 键/值

```js
let obj = {
  id: 20,
  age: 22,
  name: "zhangsan",
};
let ks = Object.keys(obj);
console.log(ks);//[id,age,name]
```

### Object.values()

作用：用于获取指定对象所有的成员值，返回数组

注意：会返回带有 `enumerable`​ 标志的非 symbol 键/值

```js
let obj = {
  id: 20,
  age: 22,
  name: "zhangsan",
};
let vs = Object.values(obj);
console.log(vs);//[20，22，'zhangsan']
```

### Object.assign()

作用：将原对象成员复制到目标对象上，有相同属性会被覆盖，如果成员是的值是简单数据类型，实现的是深拷贝

**如果成员是的值是复杂数据类型，实现的是浅拷贝**：对象里面有对象，里面那个对象还是引用地址

```js
let tar = {
  age:222222
}

let obj = {
  age:20,
  gender:'男',
  name:'zhangsan'
}
Object.assign(tar, obj, {score:100,phone:'12312312312'});
console.log(tar);
//{age:20,gender:'男',name:'zhangsan',score:100,phone:'12312312312'}
```

### Object.​getOwnPropertyNames

​`Object.getOwnPropertyNames(obj)`​：返回`obj`​上的`非symbol`​属性数组

### Object.getOwnPropertySymbols

​`Object.getOwnPropertySymbols(obj)`​：返回`obj`​上的`symbol`​属性数组

‍

‍

## Proxy

一个 `Proxy`​ 对象包装另一个对象并拦截诸如读取/写入属性和其他操作，可以选择自行处理它们，或者透明地允许该对象处理它们。

```javascript
let proxy = new Proxy(target, handler)
```

* ​`target`​ —— 是要包装的对象，可以是任何东西，包括函数。
* ​`handler`​ —— 代理配置：带有“捕捉器”（“traps”，即拦截操作的方法）的对象。比如 `get`​ 捕捉器用于读取 `target`​ 的属性，`set`​ 捕捉器用于写入 `target`​ 的属性，等等。

对 `proxy`​ 进行操作，如果在 `handler`​ 中存在相应的捕捉器，则它将运行，并且 `Proxy `​有机会对其进行处理，否则将直接对 `target `​进行处理。

---

### 捕捉器

对于对象的大多数操作，JavaScript 规范中有一个所谓的“内部方法”，它描述了最底层的工作方式。

例如 `[[Get]]`​，用于读取属性的内部方法，`[[Set]]`​，用于写入属性的内部方法，等等。**这些方法仅在规范中使用，我们不能直接通过方法名调用它们。**

Proxy 捕捉器会拦截这些方法的调用。

对于每个内部方法，此表中都有一个捕捉器：可用于添加到 `new Proxy`​ 的 `handler`​ 参数中以拦截操作的方法名称：

|内部方法|Handler 方法|何时触发|
| ----------| --------------| -------------|
|​`[[Get]]`​|​`get`​|读取属性|
|​`[[Set]]`​|​`set`​|写入属性|
|​`[[HasProperty]]`​|​`has`​|​`in`​ 操作符|
|​`[[Delete]]`​|​`deleteProperty`​|​`delete`​ 操作符|
|​`[[Call]]`​|​`apply`​|函数调用|
|​`[[Construct]]`​|​`construct`​|​`new`​ 操作符|
|​`[[GetPrototypeOf]]`​|​`getPrototypeOf`​|[Object.getPrototypeOf](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getPrototypeOf)|
|​`[[SetPrototypeOf]]`​|​`setPrototypeOf`​|[Object.setPrototypeOf](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/setPrototypeOf)|
|​`[[IsExtensible]]`​|​`isExtensible`​|[Object.isExtensible](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/isExtensible)|
|​`[[PreventExtensions]]`​|​`preventExtensions`​|[Object.preventExtensions](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)|
|​`[[DefineOwnProperty]]`​|​`defineProperty`​|[Object.defineProperty](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty), [Object.defineProperties](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties)|
|​`[[GetOwnProperty]]`​|​`getOwnPropertyDescriptor`​|[Object.getOwnPropertyDescriptor](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor), `for..in`​, `Object.keys/values/entries`​|
|​`[[OwnPropertyKeys]]`​|​`ownKeys`​|[Object.getOwnPropertyNames](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyNames), [Object.getOwnPropertySymbols](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols), `for..in`​, `Object.keys/values/entries`​|

### get

最常见的捕捉器是用于读取/写入的属性。

要拦截读取操作，`handler`​ 应该有 `get(target, property, receiver)`​ 方法。

读取属性时触发该方法，参数如下：

* ​`target`​ —— 是目标对象，该对象被作为第一个参数传递给 `new Proxy`​，
* ​`property`​ —— 目标属性名，
* ​`receiver`​ —— 如果目标属性是一个 `getter `​访问器属性，则 `receiver`​ 就是本次读取属性所在的 `this`​ 对象。通常，这就是 `proxy`​ 对象本身（或者，如果我们从 proxy 继承，则是从该 proxy 继承的对象）。

```javascript
let numbers = [0, 1, 2];

numbers = new Proxy(numbers, {
  get(target, prop) {
    if (prop in target) {
      return target[prop];
    } else {
      return 0; // 默认值
    }
  }
});

alert( numbers[1] ); // 1
alert( numbers[123] ); // 0（没有这个数组项）
```

### set

​`set(target, property, value, receiver)`​：

* ​`target`​ —— 是目标对象，该对象被作为第一个参数传递给 `new Proxy`​，
* ​`property`​ —— 目标属性名称，
* ​`value`​ —— 目标属性的值，
* ​`receiver`​ —— 与 `get`​ 捕捉器类似，仅与 setter 访问器属性相关。

如果写入操作（setting）成功，`set`​ 捕捉器应该返回 `true`​，否则返回 `false`​（触发 `TypeError`​）。

让我们用它来验证新值：

```javascript
let numbers = [];

numbers = new Proxy(numbers, { // (*)
  set(target, prop, val) { // 拦截写入属性操作
    if (typeof val == 'number') {
      target[prop] = val;
      return true;
    } else {
      return false;
    }
  }
});

numbers.push(1); // 添加成功
numbers.push(2); // 添加成功
alert("Length is: " + numbers.length); // 2

numbers.push("test"); // TypeError（proxy 的 'set' 返回 false）

alert("This line is never reached (error in the line above)");
```

请注意：数组的内建方法依然有效！值被使用 `push`​ 方法添加到数组。当值被添加到数组后，数组的 `length`​ 属性会自动增加。我们的代理对象 proxy 不会破坏任何东西。

我们不必重写诸如 `push`​ 和 `unshift`​ 等添加元素的数组方法，就可以在其中添加检查，因为在内部它们使用代理所拦截的 `[[Set]]`​ 操作。

### ownKeys、getOwnPropertyDescriptor

​`Object.keys`​，`for..in`​ 循环和大多数其他遍历对象属性的方法都使用内部方法 `[[OwnPropertyKeys]]`​（由 `ownKeys`​ 捕捉器拦截) 来获取属性列表。

* ​`Object.getOwnPropertyNames(obj)`​ 返回非 symbol 键。
* ​`Object.getOwnPropertySymbols(obj)`​ 返回 symbol 键。
* ​`Object.keys/values()`​ 返回带有 `enumerable`​ 标志的非 symbol 键/值。
* ​`for..in`​ 循环遍历所有带有 `enumerable`​ 标志的非 symbol 键，以及原型对象的键。

在下面这个示例中，我们使用 `ownKeys`​ 捕捉器拦截 `for..in`​ 对 `user`​ 的遍历，并使用 `Object.keys`​ 和 `Object.values`​ 来跳过以下划线 `_`​ 开头的属性：

```javascript
let user = {
  name: "John",
  age: 30,
  _password: "***"
};

user = new Proxy(user, {
  ownKeys(target) {
    return Object.keys(target).filter(key => !key.startsWith('_'));
  }
});

// "ownKeys" 过滤掉了 _password
for(let key in user) alert(key); // name，然后是 age

// 对这些方法的效果相同：
alert( Object.keys(user) ); // name,age
alert( Object.values(user) ); // John,30
```

如果我们返回对象中不存在的键，`Object.keys`​ 并不会列出这些键：

```javascript
let user = { };

user = new Proxy(user, {
  ownKeys(target) {
    return ['a', 'b', 'c'];
  }
});

alert( Object.keys(user) ); // <empty>
```

​`Object.keys`​ 仅返回带有 `enumerable`​ 标志的属性。为了检查它，该方法会对每个属性调用内部方法 `[[GetOwnProperty]]`​ 来获取 [它的描述符（descriptor）](https://zh.javascript.info/property-descriptors)。在这里，由于没有属性，其描述符为空，没有 `enumerable`​ 标志，因此它被略过。

为了让 `Object.keys`​ 返回一个属性，我们需要它要么存在于带有 `enumerable`​ 标志的对象，要么我们可以拦截对`[[GetOwnProperty]]`​ 的调用（捕捉器 `getOwnPropertyDescriptor`​ 可以做到这一点)，并返回带有 `enumerable: true`​ 的描述符。

```javascript
let user = { };

user = new Proxy(user, {
  ownKeys(target) { // 一旦要获取属性列表就会被调用
    return ['a', 'b', 'c'];
  },

  getOwnPropertyDescriptor(target, prop) { // 被每个属性调用
    return {
      enumerable: true,
      configurable: true
      /* ...其他标志，可能是 "value:..." */
    };
  }

});

alert( Object.keys(user) ); // a, b, c
```

### deletePropert

​`deleteProperty(target,propty,receiver)`​：返回true则正常删除，返回false则删除失败

```javascript
let user = {
  name: "John",
  _password: "***"
};
user = new Proxy(user, {
  deleteProperty(target, prop) { // 拦截属性删除
    if (prop.startsWith('_')) {
      throw new Error("Access denied");
    } else {
      delete target[prop];
      return true;
    }
  },

});
// "deleteProperty" 不允许删除 _password
try {
  delete user._password; // Error: Access denied
} catch(e) { alert(e.message); }
```

### has

​`has`​ 捕捉器会拦截 `in`​ 调用。

​`has(target, property)`​

* ​`target`​ —— 是目标对象，被作为第一个参数传递给 `new Proxy`​，
* ​`property`​ —— 属性名称。

示例如下

```javascript
let range = {
  start: 1,
  end: 10
};

range = new Proxy(range, {
  has(target, prop) {
    return prop >= target.start && prop <= target.end;
  }
});

alert(5 in range); // true
alert(50 in range); // false
```

### apply

​`apply(target, thisArg, args)`​ 捕捉器能使代理以函数的方式被调用：

* ​`target`​ 是目标对象（在 JavaScript 中，函数就是一个对象），
* ​`thisArg`​ 是 `this`​ 的值。
* ​`args`​ 是参数列表。

```javascript
function delay(f, ms) {
  return new Proxy(f, {
    apply(target, thisArg, args) {
      setTimeout(() => target.apply(thisArg, args), ms);
    }
  });
}

function sayHi(user) {
  alert(`Hello, ${user}!`);
}

sayHi = delay(sayHi, 3000);

alert(sayHi.length); // 1 (*) proxy 将“获取 length”的操作转发给目标对象

sayHi("John"); // Hello, John!（3 秒后）
```

### [内部插槽（Internal slot）](https://zh.javascript.info/proxy#nei-jian-dui-xiang-nei-bu-cha-cao-internalslot)

许多内建对象，例如 `Map`​，`Set`​，`Date`​，`Promise`​ 等，都使用了所谓的“内部插槽”。

它们类似于属性，但仅限于内部使用，仅用于规范目的。例如，`Map`​ 将项目（item）存储在 `[[MapData]]`​ 中。内建方法可以直接访问它们，而不通过 `[[Get]]/[[Set]]`​ 内部方法。所以 `Proxy`​ 无法拦截它们。

在类似这样的内建对象被代理后，代理对象没有这些内部插槽，因此内建方法将会失败。

```javascript
let map = new Map();
let proxy = new Proxy(map, {});
proxy.set('test', 1); // Error
```

在内部，一个 `Map`​ 将所有数据存储在其 `[[MapData]]`​ 内部插槽中。代理对象没有这样的插槽。[内建方法 ](https://tc39.es/ecma262/#sec-map.prototype.set)​[`Map.prototype.set`](https://tc39.es/ecma262/#sec-map.prototype.set)​ 方法试图访问内部属性 `this.[[MapData]]`​，但由于 `this=proxy`​，在 `proxy`​ 中无法找到它，只能失败。

幸运的是，这有一种解决方法：

```javascript
let map = new Map();

let proxy = new Proxy(map, {
  get(target, prop, receiver) {
    let value = Reflect.get(...arguments);
    return typeof value == 'function' ? value.bind(target) : value;
  }
});

proxy.set('test', 1);
alert(proxy.get('test')); // 1（工作了！）
```

现在它正常工作了，因为 `get`​ 捕捉器将函数属性（例如 `map.set`​）绑定到了目标对象（`map`​）本身。

与前面的示例不同，`proxy.set(...)`​ 内部 `this`​ 的值并不是 `proxy`​，而是原始的 `map`​。因此，当`set`​ 捕捉器的内部实现尝试访问 `this.[[MapData]]`​ 内部插槽时，它会成功。

> ​`Array`​ **没有内部插槽**
>
> 一个值得注意的例外：内建 `Array`​ 没有使用内部插槽。那是出于历史原因，因为它出现于很久以前。
>
> 所以，代理数组时没有这种问题。

### 可撤销 Proxy

一个 **可撤销** 的代理是可以被禁用的代理。

假设我们有一个资源，并且想随时关闭对该资源的访问。

我们可以做的是将它包装成可一个撤销的代理，没有任何捕捉器。这样的代理会将操作转发给对象，并且我们可以随时将其禁用。

语法为：

```javascript
let {proxy, revoke} = Proxy.revocable(target, handler)
```

该调用返回一个带有 `proxy`​ 和 `revoke`​ 函数的对象以将其禁用。

```javascript
let object = {
  data: "Valuable data"
};

let {proxy, revoke} = Proxy.revocable(object, {});

// 将 proxy 传递到其他某处，而不是对象...
alert(proxy.data); // Valuable data

// 稍后，在我们的代码中
revoke();

// proxy 不再工作（revoked）
alert(proxy.data); // Error
```

对 `revoke()`​ 的调用会从代理中删除对目标对象的所有内部引用，因此它们之间再无连接。

## Reflect

​`Reflect`​ 是一个内建对象，可简化 `Proxy`​ 的创建。

例如 `[[Get]]`​ 和 `[[Set]]`​ 等，都只是规范性的，不能直接调用。

​`Reflect`​ 对象使调用这些内部方法成为了可能。它的方法是内部方法的最小包装。

|操作|​`Reflect`​​ 调用|内部方法|
| --------| -------------| ----------|
|​`obj[prop]`​​|​`Reflect.get(obj, prop)`​​|​`[[Get]]`​​|
|​`obj[prop] = value`​​|​`Reflect.set(obj, prop, value)`​​|​`[[Set]]`​​|
|​`delete obj[prop]`​​|​`Reflect.deleteProperty(obj, prop)`​​|​`[[Delete]]`​​|
|​`new F(value)`​​|​`Reflect.construct(F, value)`​​|​`[[Construct]]`​​|

```js
let user = {};

Reflect.set(user, 'name', 'John');

alert(user.name); // John
```

**对于每个可被** **​`Proxy`​**​ **捕获的内部方法，在** **​`Reflect`​**​ **中都有一个对应的方法，其名称和参数与** **​`Proxy`​**​ **捕捉器相同。**

所以，我们可以使用 `Reflect`​ 来将操作转发给原始对象。

在下面这个示例中，捕捉器 `get`​ 和 `set`​ 均透明地（好像它们都不存在一样）将读取/写入操作转发到对象，并显示一条消息：

```javascript
let user = {
  name: "John",
};

user = new Proxy(user, {
  get(target, prop, receiver) {
    alert(`GET ${prop}`);
    return Reflect.get(target, prop, receiver); // (1)
  },
  set(target, prop, val, receiver) {
    alert(`SET ${prop}=${val}`);
    return Reflect.set(target, prop, val, receiver); // (2)
  }
});

let name = user.name; // 显示 "GET name"
user.name = "Pete"; // 显示 "SET name=Pete"
```

这里：

* ​`Reflect.get`​ 读取一个对象属性。
* ​`Reflect.set`​ 写入一个对象属性，如果写入成功则返回 `true`​，否则返回 `false`​。

如果一个捕捉器想要将调用转发给对象，则只需使用相同的参数调用 `Reflect.<method>`​ 就足够了。

## Math

```js
// 1. 生成随机数, [0-1)包括0不包括1
console.log(Math.random())
console.log(Math.random() * 10) //0-10  不包含10
console.log(parseInt(Math.random() * 11)) // 0-9

//2. 向上取整  Math.ceil()
console.log(Math.ceil(9.003))//10

//3. 向下取整 Math.floor()
console.log(Math.floor(9.99))//9

//4. 找最大最小数
console.log(Math.max(2, 3, 15, 8, 4))//15
console.log(Math.min(2, 3, 15, 8, 4))//2

//5. 幂运算  10的3次幂运算  Math.pow(值, 次方数)
console.log(Math.pow(2, 3))//8

//6. 绝对值
console.log(Math.abs(-10.01))//10.01
```

## Date

```js
// Date对象 : 日期对象
//1. 获取当前日期和时间
let d = new Date()
console.log(d) // 当前日期对象
console.log(typeof d) //object
console.log(d.toString()) //当前日期字符串
console.log(typeof d.toString()) //string

//  转为本地时间格式字符串
console.log( d.toLocaleString() )//2022/1/18 下午3:02:53
console.log( d.toLocaleDateString() )//2022/1/18
console.log( d.toLocaleTimeString() )//下午3:03:29

//2. 获取年份
console.log( d.getFullYear() )//2022

//3. 获取月份(0-11)
console.log(d.getMonth() + 1)//0开始

//4. 获取当前几号(1-31)
console.log(d.getDate())

//5. 获取星期几(0-6, 星期天为0)
console.log(d.getDay())

//6. 获取时间
console.log( d.getHours() )//15
console.log( d.getMinutes() )//5
console.log( d.getSeconds() )//44

//7. 获取 1970 年 1 月 1 日至今的毫秒数。
console.log(d.getTime())//时间戳作用 ： 解决浏览器时区兼容性
```

## Set

* 概念：`set`​是无序集合，和数组相比，是无序的，并且元素不能重复
* 相关方法：`set`​无法去重引用类型的数据

  ```js
  set.add(value)//添加元素到集合内，会维护插入时的顺序
  set.delete(value)//删除元素的指定元素
  set.clear()//清空集合内元素
  set.forEach(callbackFn,[,context])//遍历集合内所有元素，并作为CallbackFn的参数进行调用
  set.has(value)//检查集合内是否含有某元素
  ```
* 使用：数组去重

  ```js
  const arr = [1,1,2,2,3,3,4,4,5,5];
  const setData = Array.from(new Set(arr));//Array.from(),转化为数组
  console.log(setData);//[1,2,3,4,5]
  ```

## Map

* 概念：`Map`​ 对象保存键值对，并且能够记住键的原始插入顺序。任何值（对象或者[基本类型](https://developer.mozilla.org/zh-CN/docs/Glossary/Primitive)）都可以作为一个键或一个值。

  ```js
  map 是有序的"对象"，接受一个数组参数，该数组里面的参数也是数组形式，如 [key,value]
  const map1 = new Map();
  const map2 = new Map([
    [key,value]
  ]);
  ```
* 和对象区别

  ```js
  1.map需要显示添加键，对象有其原型对象
  2.map的键可以是任意的 函数、对象、基本类型； 对象的键是string或symbol，对象的key会被强转成string哦
  3.map是有顺序的，依据插入的顺序来；对象是"无序"的，其实也有一定顺序，不过这个顺序其实不是那么明确
  4.map可以使用size属性获取个数，map可迭代； 对象不能直接获取键值对格式，且没有迭代协议，只能通过keys、values方法
  ```
* ​`map`​常用方法

  ```js
  const map= new Map();
  map.set("Jessie", { phone: "213-555-1234", address: "123 N 1st Ave" });
  map.has("Jessie"); // true
  map.get("Hilary"); // undefined
  map.delete("Jessie"); // true
  map.delete("Raymond"); // false
  map.clean() // 清空所有元素
  console.log(contacts.size); // 1
  ```

## Promise

​`new Promise`​ 构造器返回的 `promise`​ 对象具有以下内部属性：

* ​`state`​ —— 最初是 `"pending"`​，然后在 `resolve`​ 被调用时变为 `"fulfilled"`​，或者在 `reject`​ 被调用时变为 `"rejected"`​。
* ​`result`​ —— 最初是 `undefined`​，然后在 `resolve(value)`​ 被调用时变为 `value`​，或者在 `reject(error)`​ 被调用时变为 `error`​。

​![image](assets/image-20240309203927-vdwaaou.png)​

### Promist.all()

​`Promise.all`​ 接受一个可迭代对象（通常是一个数组项为 `promise `​的数组），并返回一个新的 `promise`​。

当所有给定的 `promise`​ 都 `resolve`​时，新的 `promise `​才会 `resolve`​，并且其结果数组将成为新 `promise`​ 的结果。

```js
let promise = Promise.all(iterable);

Promise.all([
  new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
  new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
  new Promise(resolve => setTimeout(() => resolve(3), 1000))  // 3
]).then(alert); // 1,2,3 当上面这些 promise 准备好时：每个 promise 都贡献了数组中的一个元素
```

请注意，结果数组中元素的顺序与其在源 promise 中的顺序相同。即使第一个 promise 花费了最长的时间才 resolve，但它仍是结果数组中的第一个。

**如果任意一个 promise 被 reject，由** **​`Promise.all`​**​ **返回的 promise 就会立即 reject，并且带有的就是这个 error。**

```js
Promise.all([
  new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
  new Promise((resolve, reject) => setTimeout(() => reject(new Error("Whoops!")), 2000)),
  new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000))
]).catch(alert); // Error: Whoops!
```

### Promise.allSettled

​`Promise.allSettled`​ 等待所有的 `promise `​都被 `settle`​，无论结果如何。结果数组具有：

* ​`{status:"fulfilled", value:result}`​ 对于成功的响应，
* ​`{status:"rejected", reason:error}`​ 对于 error。

```js
let urls = [
  "https://api.github.com/users/iliakan",
  "https://api.github.com/users/remy",
  "https://no-such-url",
];

Promise.allSettled(urls.map((url) => fetch(url))).then((results) => {
  // (*)
  results.forEach((result, num) => {
    if (result.status == "fulfilled") {
      alert(`${urls[num]}: ${result.value.status}`);
    }
    if (result.status == "rejected") {
      alert(`${urls[num]}: ${result.reason}`);
    }
  });
});
```

上面的 `(*)`​ 行中的 `results`​ 将会是：

```javascript
[
  {status: 'fulfilled', value: ...response...},
  {status: 'fulfilled', value: ...response...},
  {status: 'rejected', reason: ...error object...}
]
```

### Promise.race

只等待第一个 settled 的 promise 并获取其结果（或 error）。

例如，这里的结果将是 `1`​：

```javascript
Promise.race([
  new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
  new Promise((resolve, reject) => setTimeout(() => reject(new Error("Whoops!")), 2000)),
  new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000))
]).then(alert); // 1
```

### Promise.any

​`Promise.race`​ 类似，区别在于 `Promise.any`​ 只等待第一个 `fulfilled `​的 `promise`​，并将这个 `fulfilled `​的 `promise `​返回。如果给出的 `promise `​都 `rejected`​，那么返回的 `promise `​会带有 [`AggregateError`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/AggregateError)​ —— 一个特殊的 error 对象，在其 `errors`​ 属性中存储着所有 promise error。

语法如下：

```javascript
let promise = Promise.any(iterable);
```

例如，这里的结果将是 `1`​：

```javascript
Promise.any([
  new Promise((resolve, reject) => setTimeout(() => reject(new Error("Whoops!")), 1000)),
  new Promise((resolve, reject) => setTimeout(() => resolve(1), 2000)),
  new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000))
]).then(alert); // 1
```

### Promise.resolve

​`Promise.resolve(value)`​ 用结果 `value`​ 创建一个 `resolved `​的 `promise`​。

```javascript
let promise = new Promise(resolve => resolve(value));
```

### Promise.reject

​`Promise.reject(error)`​ 用 `error`​ 创建一个 rejected 的 promise。

```javascript
let promise = new Promise((resolve, reject) => reject(error));
```

### Promist.then()

​`.then`​ 的第一个参数是一个函数，该函数将在 promise resolved 且接收到结果后执行。

​`.then`​ 的第二个参数也是一个函数，该函数将在 promise rejected 且接收到 error 信息后执行。

```js
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("done!"), 1000);
});

// resolve 运行 .then 中的第一个函数
promise.then(
  result => alert(result), // 1 秒后显示 "done!"
  error => alert(error) // 不运行
);
```

### Promist.catch()

如果我们只对 error 感兴趣，那么我们可以使用 `null`​ 作为第一个参数：`.then(null, errorHandlingFunction)`​。或者我们也可以使用 `.catch(errorHandlingFunction)`​，其实是一样的：

​`.catch(f)`​ 调用是 `.then(null, f)`​ 的完全的模拟，它只是一个简写形式。

```js
let promise = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});

// .catch(f) 与 promise.then(null, f) 一样
promise.catch(alert); // 1 秒后显示 "Error: Whoops!"
```

### Promist.finally()

就像常规 `try {...} catch {...}`​ 中的 `finally`​ 子句一样，`promise `​中也有 `finally`​。

调用 `.finally(f)`​ 类似于 `.then(f, f)`​，因为当 promise settled 时 `f`​ 就会执行：无论 promise 被 resolve 还是 reject。

​`finally`​ 的功能是设置一个处理程序在前面的操作完成后，执行清理/终结。

例如，停止加载指示器，关闭不再需要的连接等。

把它想象成派对的终结者。无论派对是好是坏，有多少朋友参加，我们都需要（或者至少应该）在它之后进行清理。

```js
new Promise((resolve, reject) => {
  /* 做一些需要时间的事，之后调用可能会 resolve 也可能会 reject */
})
  // 在 promise 为 settled 时运行，无论成功与否
  .finally(() => stop loading indicator)
  // 所以，加载指示器（loading indicator）始终会在我们继续之前停止
  .then(result => show result, err => show error)
```

请注意，`finally(f)`​ 并不完全是 `then(f,f)`​ 的别名。

它们之间有重要的区别：

1. ​`finally`​ 处理程序（handler）没有参数。在 `finally`​ 中，我们不知道 promise 是否成功。没关系，因为我们的任务通常是执行“常规”的完成程序（finalizing procedures）。  
    请看上面的例子：如你所见，`finally`​ 处理程序没有参数，promise 的结果由下一个处理程序处理。
2. ​`finally`​ 处理程序将结果或 error “传递”给下一个合适的处理程序。  
    例如，在这结果被从 `finally`​ 传递给了 `then`​：

    ```js
    new Promise((resolve, reject) => {
      setTimeout(() => resolve("value"), 2000)
    })
      .finally(() => alert("Promise ready")) // 先触发
      .then(result => alert(result)); // <-- .then 显示 "value"

    ```

    正如我们所看到的，第一个 promise 返回的 `value`​ 通过 `finally`​ 被传递给了下一个 `then`​。

    这非常方便，因为 `finally`​ 并不意味着处理一个 promise 的结果。如前所述，无论结果是什么，它都是进行常规清理的地方。

    下面是一个 promise 返回结果为 error 的示例，让我们看看它是如何通过 `finally`​ 被传递给 `catch`​ 的：

    ```js
    new Promise((resolve, reject) => {
      throw new Error("error");
    })
      .finally(() => alert("Promise ready")) // 先触发
      .catch(err => alert(err));  // <-- .catch 显示这个 error
    ```
3. ​`finally`​ 处理程序也不应该返回任何内容。如果它返回了，返回的值会默认被忽略。  
    此规则的唯一例外是当 `finally`​ 处理程序抛出 error 时。此时这个 error（而不是任何之前的结果）会被转到下一个处理程序。

### 隐式try...catch

​`promise `​的执行者（executor）和 `promise `​的处理程序周围有一个“隐式的 `try..catch`​”。如果发生异常，它就会被捕获，并被视为 rejection 进行处理。

```js
new Promise((resolve, reject) => {
  throw new Error("Whoops!");
}).catch(alert); // Error: Whoops!
```

与下面这段代码工作上完全相同：

```javascript
new Promise((resolve, reject) => {
  reject(new Error("Whoops!"));
}).catch(alert); // Error: Whoops!
```

在 executor 周围的“隐式 `try..catch`​”自动捕获了 error，并将其变为 rejected promise。

这不仅仅发生在 executor 函数中，同样也发生在其处理程序中。如果我们在 `.then`​ 处理程序中 `throw`​，这意味着 promise rejected，因此控制权移交至最近的 error 处理程序。

这是一个例子：

```javascript
new Promise((resolve, reject) => {
  resolve("ok");
}).then((result) => {
  throw new Error("Whoops!"); // reject 这个 promise
}).catch(alert); // Error: Whoops!
```

对于所有的 error 都会发生这种情况，而不仅仅是由 `throw`​ 语句导致的这些 error。例如，一个编程错误：

```javascript
new Promise((resolve, reject) => {
  resolve("ok");
}).then((result) => {
  blabla(); // 没有这个函数
}).catch(alert); // ReferenceError: blabla is not defined
```

最后的 `.catch`​ 不仅会捕获显式的 rejection，还会捕获它上面的处理程序中意外出现的 error。

### 未处理的rejection

当一个 error 没有被处理会发生什么？例如，我们忘了在链的尾端附加 `.catch`​，像这样：

```javascript
new Promise(function() {
  noSuchFunction(); // 这里出现 error（没有这个函数）
})
  .then(() => {
    // 一个或多个成功的 promise 处理程序
  }); // 尾端没有 .catch！
```

如果出现 error，promise 的状态将变为 “rejected”，然后执行应该跳转至最近的 rejection 处理程序。但上面这个例子中并没有这样的处理程序。因此 error 会“卡住”。没有代码来处理它。

在实际开发中，就像代码中常规的未处理的 error 一样，这意味着某些东西出了问题。

当发生一个常规的 error 并且未被 `try..catch`​ 捕获时会发生什么？脚本死了，并在控制台中留下了一个信息。对于在 promise 中未被处理的 rejection，也会发生类似的事。

**JavaScript 引擎会跟踪此类 rejection，在这种情况下会生成一个全局的 error。** 如果你运行上面这个代码，你可以在控制台中看到。

在浏览器中，我们可以使用 `unhandledrejection`​ 事件来捕获这类 error：

```javascript
window.addEventListener('unhandledrejection', function(event) {
  // 这个事件对象有两个特殊的属性：
  alert(event.promise); // [object Promise] —— 生成该全局 error 的 promise
  alert(event.reason); // Error: Whoops! —— 未处理的 error 对象
});

new Promise(function() {
  throw new Error("Whoops!");
}); // 没有用来处理 error 的 catch
```

这个事件是 HTML 标准 的一部分。

如果出现了一个 error，并且在这没有 `.catch`​，那么 `unhandledrejection`​ 处理程序就会被触发，并获取具有 error 相关信息的 `event`​ 对象，所以我们就能做一些后续处理了。

通常此类 error 是无法恢复的，所以我们最好的解决方案是将问题告知用户，并且可以将事件报告给服务器。

在 Node.js 等非浏览器环境中，有其他用于跟踪未处理的 error 的方法。

### async

* 概念：**本质是语法糖，是用promise包装过的，被async声明过的函数都是异步函数，适用await用于等待异步方法的执行完毕**

  ```js
  async fn(){
    await promise  //await 会等这边返回结果之后才会执行下一步，相当于可以把异步操作变成了同步操作
    ...
    return xxx //返回的数据会被包装成一个promise对象
    //没有返回值会返回undefined 也是promise对象
  }

  ```
* 基本使用

  ```js
  async fn(){
    ...
    const res = await request()
    const res2 = await request()
    ... 
  }
  ```
* 关于错误：**await获取到的是后面Promise成功状态传递的参数**，一旦有错误访问则需要进行处理

  ```js
  try{}.catch(error){}//只能获取同步代码，异步的错误无法获取，所以使用await是很好用的

  async fn(){
    ...
    try{
      const res = await request()
      const res2 = await request()
    }.catch(error){
      //错误抛出后执行的代码，不会影响后面代码的执行
    }
    ... 
  }

  //另一种思路：axios请求返回的结果其实都是通过promse包装的
  async fn(){
    ...
    //利用promise本身的.then或.catch方法来处理错误之后再把值return
    const res = await request().catch(err=>{ return err })//直接处理出错
    const res2 = await request().then(res=>{},err=>{return err})
    ... 
  }
  ```

## ArrayBuffer

与其他语言相比，JavaScript 中的二进制数据是以非标准方式实现的。

**基本的二进制对象是** **​`ArrayBuffer`​**​  **—— 对固定长度的连续内存空间的引用。**

```js
// 创建一个长度为 16 的 buffer
// 它会分配一个 16 字节的连续内存空间，并用 0 进行预填充。 
let buffer = new ArrayBuffer(16); 
alert(buffer.byteLength); // 16
```

`ArrayBuffer`​ 与 `Array`​ 没有任何共同之处：

* 它的长度是固定的，我们无法增加或减少它的长度。
* 它正好占用了内存中的那么多空间。
* **要访问单个字节，需要另一个“视图”对象**，而不是 `buffer[index]`​。

​`ArrayBuffer`​​ 是一个内存区域。它里面存储了什么？无从判断。只是一个原始的字节序列。

**如要操作** **​`ArrayBuffer`​**​ **，需要使用“视图”对象。**

视图对象本身并不存储任何东西。它是一副“眼镜”，透过它来解释存储在 `ArrayBuffer`​ 中的字节。

* ​**​`Uint8Array`​**​ —— 将 `ArrayBuffer`​ 中的每个字节视为 0 到 255 之间的单个数字（每个字节是 8 位，因此只能容纳那么多）。这称为 “8 位无符号整数”。
* ​**​`Uint16Array`​**​ —— 将每 2 个字节视为一个 0 到 65535 之间的整数。这称为 “16 位无符号整数”。
* ​**​`Uint32Array`​**​ —— 将每 4 个字节视为一个 0 到 4294967295 之间的整数。这称为 “32 位无符号整数”。
* ​**​`Float64Array`​**​ —— 将每 8 个字节视为一个 5.0x10<sup>-324</sup> 到 1.8x10<sup>308</sup> 之间的浮点数。

​![image](assets/image-20240226193315-7ijdlhv.png)​

### TypedArray

所有这些视图（`Uint8Array`​，`Uint32Array`​ 等）的通用术语是 [TypedArray](https://tc39.github.io/ecma262/#sec-typedarray-objects)。它们共享同一方法和属性集。

**请注意，没有名为** **​`TypedArray`​** **的构造器，它只是表示** **​`ArrayBuffer`​** **上的视图之一的通用总称术语：**​**​`Int8Array`​**​ **，**​**​`Uint8Array`​** **及其他。**

**当看到** **​`new TypedArray`​** **之类的内容时，它表示** **​`new Int8Array`​**​ **、**​**​`new Uint8Array`​** **及其他中之一。**

类型化数组的行为类似于常规数组：具有索引，并且是可迭代的。

一个类型化数组的构造器（无论是 `Int8Array`​ 或 `Float64Array`​，都无关紧要），其行为各不相同，并且取决于参数类型。

```js
new TypedArray(buffer, [byteOffset], [length]);
new TypedArray(object);
new TypedArray(typedArray);
new TypedArray(length);
new TypedArray();
```

1. 如果给定的是 `ArrayBuffer`​ 参数，则会在其上创建视图。  
    可选，我们可以给定起始位置 `byteOffset`​（默认为 0）以及 `length`​（默认至 buffer 的末尾），这样视图将仅涵盖 `buffer`​ 的一部分。
2. 如果给定的是 `Array`​，或任何类数组对象，则会创建一个相同长度的类型化数组，并复制其内容。

    ```js
    我们可以使用它来预填充数组的数据：
    let arr = new Uint8Array([0, 1, 2, 3]);
    alert( arr.length ); // 4，创建了相同长度的二进制数组
    alert( arr[1] ); // 1，用给定值填充了 4 个字节（无符号 8 位整数）
    ```
3. 如果给定的是另一个 `TypedArray`​，也是如此：创建一个相同长度的类型化数组，并复制其内容。如果需要的话，数据在此过程中会被转换为新的类型。

    ```js
    let arr16 = new Uint16Array([1, 1000]);
    let arr8 = new Uint8Array(arr16);
    alert( arr8[0] ); // 1
    alert( arr8[1] ); // 232，试图复制 1000，但无法将 1000 放进 8 位字节中（详述见下文）。
    ```
4. 对于数字参数 `length`​ —— 创建类型化数组以包含这么多元素。它的字节长度将是 `length`​ 乘以单个 `TypedArray.BYTES_PER_ELEMENT`​ 中的字节数：

    ```js
    let arr = new Uint16Array(4); // 为 4 个整数创建类型化数组
    alert( Uint16Array.BYTES_PER_ELEMENT ); // 每个整数 2 个字节
    alert( arr.byteLength ); // 8（字节中的大小）
    ```
5. 不带参数的情况下，创建长度为零的类型化数组。

我们可以直接创建一个 `TypedArray`​，而无需提及 `ArrayBuffer`​。但是，视图离不开底层的 `ArrayBuffer`​，因此，除第一种情况（已提供 `ArrayBuffer`​）外，其他所有情况都会自动创建 `ArrayBuffer`​。

如要访问底层的 `ArrayBuffer`​，那么在 `TypedArray`​ 中有如下的属性：

* ​`arr.buffer`​ —— 引用 `ArrayBuffer`​。
* ​`arr.byteLength`​ —— `ArrayBuffer`​ 的长度。

```js
可以从一个视图转到另一个视图：
let arr8 = new Uint8Array([0, 1, 2, 3]);
// 同一数据的另一个视图
let arr16 = new Uint16Array(arr8.buffer);
```

### TypedArray方法

​`TypedArray`​ 具有常规的 `Array`​ 方法，但有个明显的例外。

我们可以遍历（iterate），`map`​，`slice`​，`find`​ 和 `reduce`​ 等。

但有几件事我们做不了：

* 没有 `splice`​ —— 我们无法“删除”一个值，因为**类型化数组是缓冲区（buffer）上的视图**，并且缓冲区（buffer）是固定的、连续的内存区域。我们所能做的就是分配一个零值。
* 无 `concat`​ 方法。

还有两种其他方法：

* ​`arr.set(fromArr, [offset])`​ 从 `offset`​（默认为 0）开始，将 `fromArr`​ 中的所有元素复制到 `arr`​。
* ​`arr.subarray([begin, end])`​ 创建一个从 `begin`​ 到 `end`​（不包括）相同类型的新视图。这类似于 `slice`​ 方法（同样也支持），但不复制任何内容 —— 只是创建一个新视图，以对给定片段的数据进行操作。

有了这些方法，我们可以复制、混合类型化数组，从现有数组创建新数组等。

### DataView

[DataView](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/DataView) 是在 `ArrayBuffer`​ 上的一种特殊的超灵活“未类型化”视图。它允许以任何格式访问任何偏移量（offset）的数据。

* 对于类型化的数组，构造器决定了其格式。整个数组应该是统一的。第 i 个数字是 `arr[i]`​。
* 通过 `DataView`​，我们可以使用 `.getUint8(i)`​ 或 `.getUint16(i)`​ 之类的方法访问数据。我们在调用方法时选择格式，而不是在构造的时候。

```js
new DataView(buffer, [byteOffset], [byteLength])
```

* ​**​`buffer`​**​ —— 底层的 `ArrayBuffer`​。与类型化数组不同，`DataView`​ 不会自行创建缓冲区（buffer）。我们需要事先准备好。
* ​**​`byteOffset`​**​ —— 视图的起始字节位置（默认为 0）。
* ​**​`byteLength`​**​ —— 视图的字节长度（默认至 `buffer`​ 的末尾）。

```js
// 4 个字节的二进制数组，每个都是最大值 255
let buffer = new Uint8Array([255, 255, 255, 255]).buffer;

let dataView = new DataView(buffer);

// 在偏移量为 0 处获取 8 位数字
alert( dataView.getUint8(0) ); // 255

// 现在在偏移量为 0 处获取 16 位数字，它由 2 个字节组成，一起解析为 65535
alert( dataView.getUint16(0) ); // 65535（最大的 16 位无符号整数）

// 在偏移量为 0 处获取 32 位数字
alert( dataView.getUint32(0) ); // 4294967295（最大的 32 位无符号整数）

dataView.setUint32(0, 0); // 将 4 个字节的数字设为 0，即将所有字节都设为 0
```

当我们将混合格式的数据存储在同一缓冲区（buffer）中时，`DataView`​ 非常有用。例如，当我们存储一个成对序列（16 位整数，32 位浮点数）时，用 `DataView`​ 可以轻松访问它们。

‍

​`ArrayBufferView`​​ 是所有这些视图的总称。

​`BufferSource`​ 是 `ArrayBuffer`​ 或 `ArrayBufferView`​ 的总称。`BufferSource`​ 是最常用的术语之一，因为它的意思是“任何类型的二进制数据” —— `ArrayBuffer`​ 或其上的视图。

​![image](assets/image-20240226195108-s5zskzj.png)​

## TextDecoder

内建的 [TextDecoder](https://encoding.spec.whatwg.org/#interface-textdecoder) 对象在给定缓冲区（buffer）和编码格式（encoding）的情况下，允许将值读取为实际的 JavaScript 字符串。

```js
1.创建对象
let decoder = new TextDecoder([label], [options]);
label —— 编码格式，默认为 utf-8，但同时也支持 big5，windows-1251 等许多其他编码格式。
options —— 可选对象：
fatal —— 布尔值，如果为 true 则为无效（不可解码）字符抛出异常，否则（默认）用字符 \uFFFD 替换无效字符。
ignoreBOM —— 布尔值，如果为 true 则忽略 BOM（可选的字节顺序 Unicode 标记），很少需要使用。

2.解码
let str = decoder.decode([input], [options]);
input —— 要被解码的 BufferSource。
options —— 可选对象：
stream —— 对于解码流，为 true，则将传入的数据块（chunk）作为参数重复调用 decoder。在这种情况下，多字节的字符可能偶尔会在块与块之间被分割。这个选项告诉 TextDecoder 记住“未完成”的字符，并在下一个数据块来的时候进行解码。
```

例如：

```js
let uint8Array = new Uint8Array([72, 101, 108, 108, 111]);

alert( new TextDecoder().decode(uint8Array) ); // Hello
```

## TextEncoder

[TextEncoder](https://encoding.spec.whatwg.org/#interface-textencoder) 做相反的事情 —— 将字符串转换为字节。

```js
let encoder = new TextEncoder();
只支持 utf-8 编码。
它有两种方法：
encode(str) —— 从字符串返回 Uint8Array。
encodeInto(str, destination) —— 将 str 编码到 destination 中，该目标必须为 Uint8Array。

//示例
let encoder = new TextEncoder();
let uint8Array = encoder.encode("Hello");
alert(uint8Array); // 72,101,108,108,111
```

## Blob

### 概念

​`arrayBuffer`​ 和视图（view）都是 ECMA 标准的一部分，是 JavaScript 的一部分。

在浏览器中，还有其他更高级的对象，特别是 `Blob`​，在 [File API](https://www.w3.org/TR/FileAPI/) 中有相关描述。

​`Blob`​ 由一个可选的字符串 `type`​（通常是 MIME 类型）和 `blobParts`​ 组成 —— 一系列其他 `Blob`​ 对象，字符串和 `BufferSource`​。

​![image](assets/image-20240226200732-i1z8xm2.png)​

```js
new Blob(blobParts, options);
blobParts 是 Blob/BufferSource/String 类型的值的数组。
options 可选对象：
type —— Blob 类型，通常是 MIME 类型，例如 image/png，
endings —— 是否转换换行符，使 Blob 对应于当前操作系统的换行符（\r\n 或 \n）。默认为 "transparent"（啥也不做），不过也可以是 "native"（转换）。

// 从字符串创建 Blob
let blob = new Blob(["<html>…</html>"], {type: 'text/html'});
// 请注意：第一个参数必须是一个数组 [...]

// 从类型化数组（typed array）和字符串创建 Blob
let hello = new Uint8Array([72, 101, 108, 108, 111]); // 二进制格式的 "hello"
let blob = new Blob([hello, ' ', 'world'], {type: 'text/plain'});
```

可以用 `slice`​ 方法来提取 `Blob`​ 片段：

```js
blob.slice([byteStart], [byteEnd], [contentType]);
byteStart —— 起始字节，默认为 0。
byteEnd —— 最后一个字节（不包括，默认为最后）。
contentType —— 新 blob 的 type，默认与源 blob 相同。
参数值类似于 array.slice，也允许是负数。
```

​`Blob`​ **对象是不可改变的**

* 我们无法直接在 `Blob`​ 中更改数据，但我们可以通过 `slice`​ 获得 `Blob`​ 的多个部分，从这些部分创建新的 `Blob`​ 对象，将它们组成新的 `Blob`​，等。
* 这种行为类似于 JavaScript 字符串：我们无法更改字符串中的字符，但可以生成一个新的改动过的字符串。

### Blob用于URL

Blob 可以很容易用作 `<a>`​、`<img>`​ 或其他标签的 URL，来显示它们的内容。

多亏了 `type`​，让我们也可以下载/上传 `Blob`​ 对象，而在网络请求中，`type`​ 自然地变成了 `Content-Type`​。

‍

通过点击链接，你可以下载一个具有动态生成的内容为 `hello world`​ 的 `Blob`​ 的文件：

```js
<!-- download 特性（attribute）强制浏览器下载而不是导航 -->
<a download="hello.txt" href='#' id="link">Download</a>

<script>
let blob = new Blob(["Hello, world!"], {type: 'text/plain'});
link.href = URL.createObjectURL(blob);
</script>
```

‍

可以在 Javascript 中动态创建一个链接，通过 `link.click()`​ 模拟一个点击，然后便自动下载了。

```js
let link = document.createElement('a');
link.download = 'hello.txt';
let blob = new Blob(['Hello, world!'], {type: 'text/plain'});
link.href = URL.createObjectURL(blob);
link.click();
URL.revokeObjectURL(link.href);
```

​`URL.createObjectURL`​ 取一个 `Blob`​，并为其创建一个唯一的 URL，形式为 `blob:<origin>/<uuid>`​。

也就是 `link.href`​ 的值的样子：

```js
blob:https://javascript.info/1e67e00e-860d-40a5-89ae-6ab0cbee6273
```

浏览器内部为每个通过 `URL.createObjectURL`​ 生成的 URL 存储了一个 URL → `Blob`​ 映射。因此，此类 URL 很短，但可以访问 `Blob`​。

生成的 URL（即其链接）仅在当前文档打开的状态下才有效。它允许引用 `<img>`​、`<a>`​ 中的 `Blob`​，以及基本上任何其他期望 URL 的对象。

不过它有个副作用。虽然这里有 `Blob`​ 的映射，但 `Blob`​ 本身只保存在内存中的。浏览器无法释放它。

在文档退出时（unload），该映射会被自动清除，因此 `Blob`​ 也相应被释放了。但是，如果应用程序寿命很长，那这个释放就不会很快发生。

**因此，如果我们创建一个 URL，那么即使我们不再需要该** **​`Blob`​**​ **了，它也会被挂在内存中。**

​`URL.revokeObjectURL(url)`​ 从内部映射中移除引用，因此允许 `Blob`​ 被删除（如果没有其他引用的话），并释放内存。

在上面最后一个示例中，我们打算仅使用一次 `Blob`​，来进行即时下载，因此我们立即调用 `URL.revokeObjectURL(link.href)`​。

而在前一个带有可点击的 HTML 链接的示例中，我们不调用 `URL.revokeObjectURL(link.href)`​，因为那样会使 `Blob`​ URL 无效。在调用该方法后，由于映射被删除了，因此该 URL 也就不再起作用了。

### Blob转base64

​`URL.createObjectURL`​ 的一个替代方法是，将 `Blob`​ 转换为 base64-编码的字符串。

这种编码将二进制数据表示为一个由 0 到 64 的 `ASCII`​码组成的字符串，非常安全且“可读“。更重要的是 —— 我们可以在 “data-url” 中使用此编码。

[“data-url”](https://developer.mozilla.org/zh/docs/Web/http/Data_URIs) 的形式为 `data:[<mediatype>][;base64],<data>`​。我们可以在任何地方使用这种 url，和使用“常规” url 一样。

```js
<img src="data:image/png;base64,R0lGODlhDAAMAKIFAF5LAP/zxAAAANyuAP/gaP///wAAAAAAACH5BAEAAAUALAAAAAAMAAwAAAMlWLPcGjDKFYi9lxKBOaGcF35DhWHamZUW0K4mAbiwWtuf0uxFAgA7">
```

‍

使用内建的 `FileReader`​ 对象来将 `Blob`​ 转换为 base64。它可以将 `Blob`​ 中的数据读取为多种格式。

```js
let link = document.createElement('a');
link.download = 'hello.txt';

let blob = new Blob(['Hello, world!'], {type: 'text/plain'});

let reader = new FileReader();
reader.readAsDataURL(blob); // 将 Blob 转换为 base64 并调用 onload

reader.onload = function() {
  link.href = reader.result; // data url
  link.click();
};
```

这两种从 `Blob`​ 创建 URL 的方法都可以用。但通常 `URL.createObjectURL(blob)`​ 更简单快捷。

​`URL.createObjectURL(blob)`​

* 如果介意内存，我们需要撤销（revoke）它们
* 直接访问 `Blob`​，无需“编码/解码”

Blob 转换为 data url

* 无需撤销（revoke）任何操作。
* 对大的 `Blob`​ 进行编码时，性能和内存会有损耗。

### Blob 转换为 ArrayBuffer

Blob 构造器允许从几乎任何东西创建 blob，包括任何 BufferSource。

但是，如果我们需要执行低级别的处理时，我们可以从 `blob.arrayBuffer()`​ 中获取最低级别的 `ArrayBuffer`​：

```js
// 从 blob 获取 arrayBuffer
const bufferPromise = await blob.arrayBuffer();

// 或
blob.arrayBuffer().then(buffer => /* 处理 ArrayBuffer */);
```

### Blob 转换为 Stream

当我们读取和写入超过 `2 GB`​ 的 blob 时，将其转换为 `arrayBuffer`​ 的使用对我们来说会更加占用内存。这种情况下，我们可以直接将 blob 转换为 stream 进行处理。

stream 是一种特殊的对象，我们可以从它那里逐部分地读取（或写入）。

​`Blob`​ 接口里的 `stream()`​ 方法返回一个 `ReadableStream`​，在被读取时可以返回 `Blob`​ 中包含的数据。

```js
// 从 blob 获取可读流（readableStream）
const readableStream = blob.stream();
const stream = readableStream.getReader();

while (true) {
  // 对于每次迭代：value 是下一个 blob 数据片段
  let { done, value } = await stream.read();
  if (done) {
    // 读取完毕，stream 里已经没有数据了
    console.log('all blob processed.');
    break;
  }

  // 对刚从 blob 中读取的数据片段做一些处理
  console.log(value);
}
```

​`arrayBuffer`​​，`Uint8Array`​​ 及其他 `BufferSource`​​ 是“二进制数据”，而 `Blob `​​则表示“具有类型的二进制数据”。

## File

[File](https://www.w3.org/TR/FileAPI/#dfn-file) 对象继承自 `Blob`​，并扩展了与文件系统相关的功能。

有两种方式可以获取它。

第一种，与 `Blob`​ 类似，有一个构造器：

```js
new File(fileParts, fileName, [options])
fileParts —— Blob/BufferSource/String 类型值的数组。
fileName —— 文件名字符串。
options —— 可选对象：
lastModified —— 最后一次修改的时间戳（整数日期）。
```

第二种，更常见的是，我们从 `<input type="file">`​ 或拖放或其他浏览器接口来获取文件。在这种情况下，file 将从操作系统（OS）获得 this 信息。

由于 `File`​ 是继承自 `Blob`​ 的，所以 `File`​ 对象具有相同的属性，附加：

* ​`name`​ —— 文件名，
* ​`lastModified`​ —— 最后一次修改的时间戳。

这就是我们从 `<input type="file">`​ 中获取 `File`​ 对象的方式：

```js
<input type="file" onchange="showFile(this)">

<script>
function showFile(input) {
  let file = input.files[0];
  alert(`File name: ${file.name}`); // 例如 my.png
  alert(`Last modified: ${file.lastModified}`); // 例如 1552830408824
}
</script>

```

注意：输入（input）可以选择多个文件，因此 `input.files`​ 是一个类数组对象。这里我们只有一个文件，所以我们只取 `input.files[0]`​。

## FileReader

[FileReader](https://www.w3.org/TR/FileAPI/#dfn-filereader) 是一个对象，其唯一目的是从 `Blob`​（因此也从 `File`​）对象中读取数据。

它使用事件来传递数据，因为从磁盘读取数据可能比较费时间。

构造函数：

```js
let reader = new FileReader(); // 没有参数
```

---

* ​**​`readAsArrayBuffer(blob)`​** ​ —— 将数据读取为二进制格式的 `ArrayBuffer`​。
* ​**​`readAsText(blob, [encoding])`​** ​ —— 将数据读取为给定编码（默认为 `utf-8`​ 编码）的文本字符串。
* ​**​`readAsDataURL(blob)`​** ​ —— 读取二进制数据，并将其编码为 base64 的 data url。
* ​**​`abort()`​** ​ —— 取消操作。

​`read*`​ 方法的选择，取决于我们喜欢哪种格式，以及如何使用数据。

* ​`readAsArrayBuffer`​ —— 用于二进制文件，执行低级别的二进制操作。对于诸如切片（slicing）之类的高级别的操作，`File`​ 是继承自 `Blob`​ 的，所以我们可以直接调用它们，而无需读取。
* ​`readAsText`​ —— 用于文本文件，当我们想要获取字符串时。
* ​`readAsDataURL`​ —— 当我们想在 `src`​ 中使用此数据，并将其用于 `img`​ 或其他标签时。正如我们在 [Blob](https://zh.javascript.info/blob) 一章中所讲的，还有一种用于此的读取文件的替代方案：`URL.createObjectURL(file)`​。

读取过程中，有以下事件：

* ​`loadstart`​ —— 开始加载。
* ​`progress`​ —— 在读取过程中出现。
* ​`load`​ —— 读取完成，没有 error。
* ​`abort`​ —— 调用了 `abort()`​。
* ​`error`​ —— 出现 error。
* ​`loadend`​ —— 读取完成，无论成功还是失败。

读取完成后，我们可以通过以下方式访问读取结果：

* ​`reader.result`​ 是结果（如果成功）
* ​`reader.error`​​ 是 error（如果失败）

```js
<input type="file" onchange="readFile(this)">

<script>
function readFile(input) {
  let file = input.files[0];

  let reader = new FileReader();

  reader.readAsText(file);

  reader.onload = function() {
    console.log(reader.result);
  };

  reader.onerror = function() {
    console.log(reader.error);
  };

}
</script>
```

> **在 Web Workers 中可以使用** `FileReaderSync`​
>
> 对于 Web Worker，还有一种同步的 `FileReader`​ 变体，称为 [FileReaderSync](https://www.w3.org/TR/FileAPI/#FileReaderSync)。
>
> 它的读取方法 `read*`​ 不会生成事件，但是会像常规函数那样返回一个结果。
>
> 不过，这仅在 Web Worker 中可用，因为在读取文件的时候，同步调用会有延迟，而在 Web Worker 中，这种延迟并不是很重要。它不会影响页面。

## FormData

​`FormData`​表示 HTML 表单数据的对象。表单始终以 `Content-Type: multipart/form-data`​ 来发送数据

```js
let formData = new FormData([form]);
```

​`FormData`​ 的特殊之处在于网络方法（network methods），例如 `fetch`​ 可以接受一个 `FormData`​ 对象作为 body。它会被编码并发送出去，带有 `Content-Type: multipart/form-data`​。

```html
<form id="formElem">
  <input type="text" name="name" value="John">
  <input type="text" name="surname" value="Smith">
  <input type="submit">
</form>

<script>
  formElem.onsubmit = async (e) => {
    e.preventDefault();
    let response = await fetch('/article/formdata/post/user', {
      method: 'POST',
      body: new FormData(formElem)
    });
    let result = await response.json();
    alert(result.message);
  };
</script>
```

---

我们可以使用以下方法修改 `FormData`​ 中的字段：

* ​`formData.append(name, value)`​ —— 添加具有给定 `name`​ 和 `value`​ 的表单字段，
* ​`formData.append(name, blob, fileName)`​ —— 添加一个字段，就像它是 `<input type="file">`​，第三个参数 `fileName`​ 设置文件名（而不是表单字段名），因为它是用户文件系统中文件的名称，
* ​`formData.delete(name)`​ —— 移除带有给定 `name`​ 的字段，
* ​`formData.get(name)`​ —— 获取带有给定 `name`​ 的字段值，
* ​`formData.has(name)`​ —— 如果存在带有给定 `name`​ 的字段，则返回 `true`​，否则返回 `false`​。

从技术上来讲，一个表单可以包含多个具有相同 `name`​ 的字段，因此，多次调用 `append`​ 将会添加多个具有相同名称的字段。

还有一个 `set`​ 方法，语法与 `append`​ 相同。不同之处在于 `.set`​ 移除所有具有给定 `name`​ 的字段，然后附加一个新字段。因此，它确保了只有一个具有这种 `name`​ 的字段，其他的和`append`​ 一样：

* ​`formData.set(name, value)`​，
* ​`formData.set(name, blob, fileName)`​​。

我们也可以使用 `for..of`​ 循环迭代 formData 字段：

```js
let formData = new FormData();
formData.append('key1', 'value1');
formData.append('key2', 'value2');

// 列出 key/value 对
for(let [name, value] of formData) {
  alert(`${name} = ${value}`); // key1 = value1，然后是 key2 = value2
}
```

## URL

### 基本概念

内建的 `URL`​ 类提供了用于创建和解析 URL 的便捷接口。

创建一个新 `URL`​ 对象的语法：

```javascript
new URL(url, [base])
```

* ​**​`url`​**​ —— 完整的 URL，或者仅路径（如果设置了 base），
* ​**​`base`​**​ —— 可选的 base URL：如果设置了此参数，且参数 `url`​ 只有路径，则会根据这个 `base`​ 生成 URL。

例如：

```javascript
let url = new URL('https://javascript.info/profile/admin');
```

下面这两个 URL 是一样的：

```javascript
let url1 = new URL('https://javascript.info/profile/admin');
let url2 = new URL('/profile/admin', 'https://javascript.info');

alert(url1); // https://javascript.info/profile/admin
alert(url2); // https://javascript.info/profile/admin
```

我们可以根据相对于现有 URL 的路径轻松创建一个新的 URL：

```javascript
let url = new URL('https://javascript.info/profile/admin');
let newUrl = new URL('tester', url);

alert(newUrl); // https://javascript.info/profile/tester
```

​`URL`​ 对象立即允许我们访问其组件，因此这是一个解析 url 的好方法，例如：

```javascript
let url = new URL('https://javascript.info/url');
alert(url.protocol); // https:
alert(url.host);     // javascript.info
alert(url.pathname); // /url
```

这是 URL 组件的备忘单：

​![image](assets/image-20240310104227-wjm0mh6.png)​

* ​`href`​ 是完整的 URL，与 `url.toString()`​ 相同
* ​`protocol`​ 以冒号字符 `:`​ 结尾
* ​`search`​ —— 以问号 `?`​ 开头的一串参数
* ​`hash`​ 以哈希字符 `#`​ 开头
* 如果存在 HTTP 身份验证，则这里可能还会有 `user`​ 和 `password`​ 属性：`http://login:password@site.com`​（图片上没有，很少被用到）。

> **我们可以将** `URL`​ **对象传递给网络（和大多数其他）方法，而不是字符串**
>
> 我们可以在 `fetch`​ 或 `XMLHttpRequest`​ 中使用 `URL`​ 对象，几乎可以在任何需要 URL 字符串的地方都能使用 `URL`​ 对象。
>
> 通常，`URL`​ 对象可以替代字符串传递给任何方法，因为大多数方法都会执行字符串转换，这会将 `URL`​ 对象转换为具有完整 URL 的字符串。

### [SearchParams](https://zh.javascript.info/url#searchparams)

假设，我们想要创建一个具有给定搜索参数的 url，例如：`https://google.com/search?query=JavaScript`​。

我们可以在 URL 字符串中提供它们：

```javascript
new URL('https://google.com/search?query=JavaScript')
```

但是，如果参数中包含空格，非拉丁字母等（具体参见下文），参数就需要被编码。

因此，有一个 URL 属性用于解决这个问题：`url.searchParams`​，[URLSearchParams](https://url.spec.whatwg.org/#urlsearchparams) 类型的对象。

它为搜索参数提供了简便的方法：

* ​**​`append(name, value)`​** ​ —— 按照 `name`​ 添加参数，
* ​**​`delete(name)`​** ​ —— 按照 `name`​ 移除参数，
* ​**​`get(name)`​** ​ —— 按照 `name`​ 获取参数，
* ​**​`getAll(name)`​** ​ —— 获取相同 `name`​ 的所有参数（这是可行的，例如 `?user=John&user=Pete`​），
* ​**​`has(name)`​** ​ —— 按照 `name`​ 检查参数是否存在，
* ​**​`set(name, value)`​** ​ —— set/replace 参数，
* ​**​`sort()`​** ​ —— 按 name 对参数进行排序，很少使用，
* 并且它是可迭代的，类似于 `Map`​。

包含空格和标点符号的参数的示例：

```javascript
let url = new URL('https://google.com/search');
url.searchParams.set('q', 'test me!'); // 添加带有一个空格和一个 ! 的参数
alert(url); // https://google.com/search?q=test+me%21
url.searchParams.set('tbs', 'qdr:y'); // 添加带有一个冒号 : 的参数
// 参数会被自动编码
alert(url); // https://google.com/search?q=test+me%21&tbs=qdr%3Ay
// 遍历搜索参数（被解码）
for(let [name, value] of url.searchParams) {
  alert(`${name}=${value}`); // q=test me!，然后是 tbs=qdr:y
}
```

### [编码（encoding）](https://zh.javascript.info/url#bian-ma-encoding)

[RFC3986](https://tools.ietf.org/html/rfc3986) 标准定义了 URL 中允许哪些字符，不允许哪些字符。

那些不被允许的字符必须被编码，例如非拉丁字母和空格 —— 用其 UTF-8 代码代替，前缀为 `%`​，例如 `%20`​（由于历史原因，空格可以用 `+`​ 编码，但这是一个例外）。

好消息是 `URL`​ 对象会自动处理这些。我们仅需提供未编码的参数，然后将 `URL`​ 转换为字符串：

```javascript
// 在此示例中使用一些西里尔字符

let url = new URL('https://ru.wikipedia.org/wiki/Тест');

url.searchParams.set('key', 'ъ');
alert(url); //https://ru.wikipedia.org/wiki/%D0%A2%D0%B5%D1%81%D1%82?key=%D1%8A
```

正如你所看到的，url 路径中的 `Тест`​ 和 `ъ`​ 参数都被编码了。

URL 变长了，因为每个西里尔字母用 UTF-8 编码的两个字节表示，因此这里有两个 `%..`​ 实体（entities）。

### [编码字符串](https://zh.javascript.info/url#bian-ma-zi-fu-chuan)

在过去，在出现 `URL`​ 对象之前，人们使用字符串作为 URL。

而现在，`URL`​ 对象通常更方便，但是仍然可以使用字符串。在很多情况下，使用字符串可以使代码更短。

如果使用字符串，则需要手动编码/解码特殊字符。

下面是用于编码/解码 URL 的内建函数：

* [encodeURI](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/encodeURI) —— 编码整个 URL。
* [decodeURI](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/decodeURI) —— 解码为编码前的状态。
* [encodeURIComponent](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent) —— 编码 URL 组件，例如搜索参数，或者 hash，或者 pathname。
* [decodeURIComponent](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/decodeURIComponent) —— 解码为编码前的状态。

一个自然的问题：“`encodeURIComponent`​ 和 `encodeURI`​ 之间有什么区别？我们什么时候应该使用哪个？”

如果我们看一个 URL，就容易理解了，它被分解为本文上面图中所示的组件形式：

```none
https://site.com:8080/path/page?p1=v1&p2=v2#hash
```

正如我们所看到的，在 URL 中 `:`​，`?`​，`=`​，`&`​，`#`​ 这类字符是被允许的。

另一方面，对于 URL 的单个组件，例如一个搜索参数，则必须对这些字符进行编码，以免破坏 URL 的格式。

* ​`encodeURI`​ 仅编码 URL 中完全禁止的字符。
* ​`encodeURIComponent`​ 也编码这类字符，此外，还编码 `#`​，`$`​，`&`​，`+`​，`,`​，`/`​，`:`​，`;`​，`=`​，`?`​ 和 `@`​ 字符。

所以，对于一个 URL 整体，我们可以使用 `encodeURI`​：

```javascript
// 在 url 路径中使用西里尔字符
let url = encodeURI('http://site.com/привет');

alert(url); // http://site.com/%D0%BF%D1%80%D0%B8%D0%B2%D0%B5%D1%82
```

……而对于 URL 参数，我们应该改用 `encodeURIComponent`​：

```javascript
let music = encodeURIComponent('Rock&Roll');

let url = `https://google.com/search?q=${music}`;
alert(url); // https://google.com/search?q=Rock%26Roll
```

将其与 `encodeURI`​ 进行比较：

```javascript
let music = encodeURI('Rock&Roll');

let url = `https://google.com/search?q=${music}`;
alert(url); // https://google.com/search?q=Rock&Roll
```

我们可以看到，`encodeURI`​ 没有对 `&`​ 进行编码，因为它对于整个 URL 来说是合法的字符。

但是，我们应该编码在搜索参数中的 `&`​ 字符，否则，我们将得到 `q=Rock&Roll`​ —— 实际上是 `q=Rock`​ 加上某个晦涩的参数 `Roll`​。不符合预期。

因此，对于每个搜索参数，我们应该使用 `encodeURIComponent`​，以将其正确地插入到 URL 字符串中。最安全的方式是对 name 和 value 都进行编码，除非我们能够绝对确保它只包含允许的字符。

> ​`encode*`​ **与** `URL`​ **之间的编码差异**
>
> [URL](https://url.spec.whatwg.org/#url-class) 和 [URLSearchParams](https://url.spec.whatwg.org/#interface-urlsearchparams) 基于最新的 URL 规范：[RFC3986](https://tools.ietf.org/html/rfc3986)，而 `encode*`​ 函数是基于过时的 [RFC2396](https://www.ietf.org/rfc/rfc2396.txt)。
>
> 它们之间有一些区别，例如对 IPv6 地址的编码方式不同：
>
> ```javascript
> // IPv6 地址的合法 url
> let url = 'http://[2607:f8b0:4005:802::1007]/';
>
> alert(encodeURI(url)); // http://%5B2607:f8b0:4005:802::1007%5D/
> alert(new URL(url)); // http://[2607:f8b0:4005:802::1007]/
> ```
>
> 正如我们所看到的，`encodeURI`​ 替换了方括号 `[...]`​，这是不正确的，原因是：在 RFC2396 (August 1998) 时代，还不存在 IPv6 url。
>
> 这种情况很少见，`encode*`​ 函数在大多数情况下都能正常工作。

## 正则

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
* 汉字类：需要通过编码进行匹配`[\u4e00-\u9fa5]`​

  ```js
  每个汉字在计算中都有一个编码，汉字编码的范围在 \u4e00-\u9fa5 之间。
  \u表示的是16进制
  ```
* 反向范围匹配：`^`​符号必须在 `[ ]`​ 内(`[^]`​)，而且是第1位才是反向范围匹配

  ```js
  [ ]  :表示一个字符范围
  [^]  :反向匹配一个范围之外的字符
  [^a-h]//不包含a-h的任何一个字符
  ```
* 边界：元字符 `^`​ 与`$`​

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
* 分组：`()`​：用于表示所操作的范围

  ```js
  // 需求：需要得到 'okokok' 字符串
  console.log(/ok{3}/.test('okokok'));      // false
  // ok{3}  {3}是修饰前面的k出现3次
  console.log(/ok{3}/.test('okkk'));        // true

  // 使用()表示 {3}所修饰的范围
  console.log(/(ok){3}/.test('okokok'));    // true
  console.log(/(ok){3}/.test('okkk'));      // false

  ```
* 或运算与分组：用于完成或运算，经常需要借助`()`​对或的内容进行分组

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
* 转义符：`\`​：去除某些字符被正则赋予的特殊的功能

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

### string.test(reg)

​`string.test(reg)`​:正则校验，检验字符串是否匹配正则

```js
正则对象.test(字符串)//返回值是boolean
let reg = /123/
console.log(reg.test('123'))
```

### string.replace(reg)

​`string.replace(reg)`：正则替换，字符串替换

```js
// 字符串.replace(正则,替换内容)
// 在字符串中，将正则匹配到的部分使用 替换内容 进行替换
      
// 默认仅替换匹配到的第1个
console.log('abc3des5d'.replace(/\d/,'*'))   //abc*des5d
console.log('abc3des5d'.replace(/\d/g,'*'))  //abc*des*d
```

### reg.exec(string)

​`reg.exec(string)`​：正则查找，查找字符串中符合正则规则的字符，并返回类数组对象，还有 index、input 属性，表示匹配的字符串下标，和匹配到的内容

作用：查看正则表达式匹配str的结果（是第几个字符满足匹配规则的，是哪个字符满足匹配规则等）  
特点：默认情况下，每一次调用exec只会返回第一个匹配的字符串信息，如果想要返回所有匹配信息，需要设置正则表达式为全局匹配或者需要执行多次exec方法

```js
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

# WebAPI

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

### 元素标准属性操作

```js
dom.id  //获取id
dom.className  //获取类名
dom.href  //获取链接（常用于a标签）
dom.src  //获取路径(常用于img标签)
```

### 元素非标属性操作

* 设置非标属性：非标属性设置使用`data-xx='xxx'`​

  ```js
  <img id="pic" src="./images/yshg.png" alt="" data-sex='男'>
  ```
* 获取非标属性：**​`dom.data.xx`​**​

  ```js
  document.querySelector('img').data.sex//获取dom的sex属性（标签内为data-sex="boy"）
  document.querySelector('img').dataset.sex = '新属性'
  ```
* 使用`attribute`​：主要用来操作非标属性

  ```js
  dom.getAttribute('属性名')//用于获取标签对象的属性
  dom.setAttribute('属性名', '属性值')//用于设置标签对象的属性
  dom.removeAttribute('属性名')//用于移除标签对象属性
  ```

### 元素样式属性操作

```js
//box.style.xxx 只能获取dom行内样式属性，使用style标签给dom添加的样式是无法获取的，需要使用getComputedStyle(dom).xxx
let box = document.querySelector('.box')
box.style.backgroundColor = 'skyblue'  //添加行内样式，只能获取到行内样式
getComputedStyle(box).width;//可以获取到css给dom添加的样式

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

## Node节点

### 节点概念

* 节点：对​html的DOM树​更细致的**描述**
* 节点的三个信息

  * 节点类型：dom.nodeType
  * 节点名称：dom.nodeName
  * 节点的值：dom.nodeValue

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

### 节点操作

* 创建节点：**新创建的节点仅位于内存中，必须添加到DOM树上才会有页面效果**

  ```js
  let oH1 = document.createElement('h1')  //用于创建一个标签
  ```
* 追加节点：子节点追加到父节点后面

  ```js
  父节点.appendChild(子节点)
  ```
* 克隆节点：用于对节点进行克隆操作,克隆出来的节点仅位于内存中，参数`boolean值`​，`ture`​为深克隆

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

​![image](assets/image-20240515203016-wq0ueue.png)​

### offset

* ​`offsetWidth`​、`offsetHeight`​

  ```js
  //offsetWidth     获取元素在页面的宽尺寸(width padding border)
  //offsetHeight    获取元素在页面的高尺寸(height padding border)
  //它是一个组合属性，返回值是数值没有单位。
  let oBox = document.querySelector(".box")
  console.log(oBox.style.width);
  console.log(oBox.offsetWidth);
  console.log(oBox.offsetHeight);
  ```
* ​`offsetParent`​：用于获取相对定位的父元素。

  ```js
  //offsetParent 用于获取相对位置的父元素。
  //如果元素的定义方式是fixed, 元素.offsetParent就是null
  //如果元素的定义方式是非fixed, 
  //元素.offsetParent就是距离它最近具有定位方式的父元素
  //如果没有这样的父元素，元素.offsetParent 就是body
  let oCore = document.querySelector('.core');
  console.log(oCore.offsetParent);
  ```
* ​`offsetLeft`​、`offsetTop`​

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

## 事件

### 事件概念

​`js`​处理用户交互的一种机制

事件三要素：事件源、事件类型、事件处理函数

### 事件注册

```js
//事件源.on事件名 = 事件处理函数，只能注册一个，有重复会覆盖
dom.onClick = function(){}
```

### 事件监听

事件监听是为元素绑定事件的另一种方式，这种方式可以为一个对象的同一个事件绑定多个事件处理函数。

* 添加事件监听：事件名不需要加`on`​，可以绑定多个事件

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
* 开启事件捕捉：`标签对象.addEventListener('事件名',事件处理函数，true)`​

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

​![image](assets/image-20220923085931-0eifcni.png)​

* **事件捕获阶段**：事件从父元素开始向目标元素传播，从 `Window`​ 对象开始传播。

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
* **事件冒泡阶段**：这时与捕获阶段相反，事件向父元素传播，触发同名事件函数，直到 `Window`​ 对象。

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
* **禁用默认事件：** 禁用原dom的事件触发，执行自己的

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

### 事件类型

```js
1.鼠标事件
click : 鼠标单击时触发
dblclick : 鼠标双击时触发
mousemove : 鼠标移动时触发
mouseenter/mouseover :鼠标经过时触发
mouseleave/mouseout : 鼠标离开时触发
其中mouseenter 和 mouseleave  不支持冒泡 (推荐使用)
mouseover 和 mouseout  支持冒泡

2.焦点事件
focus : 获得焦点时触发
blus : 失去焦点时触发

3.文本事件
input : (主要是表单)输入时触发
change : 当文本框（input或textarea）内容改变且失去焦点后触发

4.键盘事件
keydown :  按下键盘时触发
keyup : 键盘弹起时触发

5.页面加载事件
load : 当资源加载完毕执行的事件
监听页面所有资源加载完毕 : 给 window 添加 load 事件
window.addEventListener('load',函数)
监听页面DOM加载完毕 : 给 document 添加 DOMContentLoaded 事件
document.addEventListener('DOMContentLoaded',函数)
监听页面某个资源(例如:图片)加载完毕 :
document.addEventListener('load',函数)
应用场景: 当script标签写在前面,由于代码从上往下执行,此时会导致js获取不到元素,这时就要给给 window添加load事件

6.元素滚动事件
scroll : 滚动条滚动时触发(谁有滚动条给谁添加该事件)
注意点 :
如果浏览器窗口有滚动条就给window加scroll,此时scrollTop是html的值(兼容性问题,有些浏览器scrollTop是body的值)
如果没有书写代码时没有写<!DOCTYPE html>,此时的scrollTop是body的值
scrollTop : 获取滚动位置,可以获取也可以重新赋值
元素.scrollTo(x,y)  滚动到具体位置

7.页面尺寸事件
resize : 当窗口的大小发生改变时触发

8.移动端touch触屏事件
touchstart : 触摸
touchend : 离开
touchmove : 移动

9.播放事件
audio标签
play : 播放事件
pause : 暂停事件
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

​![1637143293811](assets/BOM_DOM-20220804230540-0h0z33j.png)​

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
* 内置对象的分类

  ```js
  1）值属性，这些全局属性返回一个简单值，这些值没有自己的属性和方法。例如 Infinity、NaN、undefined、null 字面量
  （2）函数属性，全局函数可以直接调用，不需要在调用时指定所属对象，执行结束后会将结果直接返回给调用者。例如 eval()、parseFloat()、parseInt() 等
  （3）基本对象，基本对象是定义或使用其他对象的基础。基本对象包括一般对象、函数对象和错误对象。例如 Object、Function、Boolean、Symbol、Error 等
  （4）数字和日期对象，用来表示数字、日期和执行数学计算的对象。例如 Number、Math、Date
  （5）字符串，用来表示和操作字符串的对象。例如 String、RegExp
  （6）可索引的集合对象，这些对象表示按照索引值来排序的数据集合，包括数组和类型数组，以及类数组结构的对象。例如 Array
  （7）使用键的集合对象，这些集合对象在存储数据时会使用到键，支持按照插入顺序来迭代元素。
  例如 Map、Set、WeakMap、WeakSet
  （8）矢量集合，SIMD 矢量集合中的数据会被组织为一个数据序列。
  例如 SIMD 等
  （9）结构化数据，这些对象用来表示和操作结构化的缓冲区数据，或使用 JSON 编码的数据。例如 JSON 等
  （10）控制抽象对象
  例如 Promise、Generator 等
  （11）反射。例如 Reflect、Proxy
  （12）国际化，为了支持多语言处理而加入 ECMAScript 的对象。例如 Intl、Intl.Collator 等
  （13）WebAssembly
  （14）其他。例如 arguments

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
  ```

### history对象

history对象主要用于记录你当前窗口的历史记录，并为我们提供了前进与后退的功能。

​`history.forward()`​：实现前进

​`history.back()`​：实现后退

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

​![image](assets/image-20220804232217-69eb40a.png)​

​![image](assets/image-20220804232224-6be1mrb.png)​

* ​`浏览器渲染解析页面完整流程`​
* 1.先解析HTML，生成DOM树（重要步骤）
* 2.后解析CSS，生成样式规则
* 3.根据DOM树与样式规则，得到一颗渲染树Render Tree（重要步骤）

  * DOM树：只有结构没有样式
  * 渲染树：DOM树 + CSS样式 （可以理解为附加了样式的DOM树）
* 4.渲染引擎开始工作，解析渲染树。

  * 这个步骤发生`回流(又称重排)`​  : 计算节点的尺寸、结构、布局
* 5.开始绘制

  * 这个步骤发生`重绘`​ : 根据重排结果进行绘制页面
* 6.渲染完成，呈现页面
* 重要概念：`重排一定会引起重绘，但是重绘不一定引起重排`​

  * 引起重排操作：主要是修改了尺寸、布局（总结起来就是对布局产生印象）

    * 如：修改盒子的尺寸， 修改盒子位置、修改字体大小、边框等等影响盒子模型布局的都会导致重绘
    * ​`重排`​：会导致页面大面积重绘，甚至是整个页面重新绘制。（影响性能）
  * 引起重绘操作：不影响布局，只是影响盒子自身属性

    * 如：修改颜色，修改透明度等，对布局没有任何影响的

​![image](assets/image-20220804232257-30sii1t.png)

---

# 网络请求

## ajax

**AJAX**（Asynchronous [JavaScript](https://developer.mozilla.org/zh-CN/docs/Glossary/JavaScript) And [XML](https://developer.mozilla.org/zh-CN/docs/Glossary/XML) ）是一种使用 [XMLHttpRequest](https://developer.mozilla.org/zh-CN/docs/Glossary/XMLHttpRequest) 技术构建更复杂，动态的网页的编程实践。

AJAX 允许只更新一个 [HTML](https://developer.mozilla.org/zh-CN/docs/Glossary/HTML) 页面的部分 [DOM](https://developer.mozilla.org/zh-CN/docs/Glossary/DOM)，而无须重新加载整个页面。AJAX 还允许异步工作，这意味着当网页的一部分正试图重新加载时，你的代码可以继续运行（相比之下，同步会阻止代码继续运行，直到这部分的网页完成重新加载）。

通过交互式网站和现代 Web 标准，AJAX 正在逐渐被 JavaScript 框架中的函数和官方的 [`Fetch API`](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API)​ 标准取代。

## 同源跨域

同源指的是两个 URL 地址具有相同的**协议地址、主机名、端口号。**

**其中有一个不一样就是跨域**

**跨域请求产生时，请求是发出去了，也是有响应的，仅仅是浏览器同源策略，认为不安全，拦截了结果，不将数据传递我们使用罢了**

### 安全请求

有两种类型的**跨源请求**：

1. 安全请求。
2. 所有其他请求。

如果一个请求满足下面这两个条件，则该请求是安全的：

1. [安全的方法](https://fetch.spec.whatwg.org/#cors-safelisted-method)：GET，POST 或 HEAD
2. [安全的 header](https://fetch.spec.whatwg.org/#cors-safelisted-request-header) —— 仅允许自定义下列 header：

    * ​`Accept`​，
    * ​`Accept-Language`​，
    * ​`Content-Language`​，
    * ​`Content-Type`​ 的值为 `application/x-www-form-urlencoded`​，`multipart/form-data`​ 或 `text/plain`​。

任何其他请求都被认为是“非安全”请求。例如，具有 `PUT`​ 方法或 `API-Key`​ HTTP-header 的请求就不是安全请求。

**本质区别在于，可以使用**  **​`<form>`​** ​ **或**  **​`<script>`​** ​ **进行安全请求，而无需任何其他特殊方法。** 因此，即使是非常旧的服务器也能很好地接收安全请求。

与此相反，带有非标准 header 或者例如 `DELETE`​ 方法的请求，无法通过这种方式创建。

当我们尝试发送一个非安全请求时，浏览器会发送一个特殊的“预检（preflight）”请求到服务器 —— 询问服务器，你接受此类跨源请求吗？

并且，除非服务器明确通过 header 进行确认，否则非安全请求不会被发送。

如果一个请求是跨源的，浏览器始终会向其添加 `Origin`​ header。

例如，如果我们从 `https://javascript.info/page`​ 请求 `https://anywhere.com/request`​，请求的 header 将如下所示：

```http
GET /request
Host: anywhere.com
Origin: https://javascript.info
...
```

正如你所看到的，`Origin`​ 包含了确切的源（domain/protocol/port），没有路径（path）。

服务器可以检查 `Origin`​，如果同意接受这样的请求，就会在响应中添加一个特殊的 header `Access-Control-Allow-Origin`​。该 header 包含了允许的源（在我们的示例中是 `https://javascript.info`​），或者一个星号 `*`​。然后响应成功，否则报错。

浏览器在这里扮演受被信任的中间人的角色：

1. 它确保发送的跨源请求带有正确的 `Origin`​。
2. 它检查响应中的许可 `Access-Control-Allow-Origin`​，如果存在，则允许 JavaScript 访问响应，否则将失败并报错。

​![image](assets/image-20240310103027-w9z9teg.png)​

这是一个带有服务器许可的响应示例：

```http
200 OK
Content-Type:text/html; charset=UTF-8
Access-Control-Allow-Origin: https://javascript.info
```

---

**对于安全请求：**

* → 浏览器发送带有源的 `Origin`​ header。
* ← 对于没有凭据的请求（默认不发送），服务器应该设置：

  * ​`Access-Control-Allow-Origin`​ 为 `*`​ 或与 `Origin`​ 的值相同
* ← 对于具有凭据的请求，服务器应该设置：

  * ​`Access-Control-Allow-Origin`​ 值与 `Origin`​ 的相同
  * ​`Access-Control-Allow-Credentials`​ 为 `true`​

此外，要授予 JavaScript 访问除 `Cache-Control`​，`Content-Language`​，`Content-Type`​，`Expires`​，`Last-Modified`​ 或 `Pragma`​ 外的任何 response header 的权限，服务器应该在 header `Access-Control-Expose-Headers`​ 中列出允许的那些 header。

**对于非安全请求，会在请求之前发出初步“预检”请求：**

* → 浏览器将具有以下 header 的 `OPTIONS`​ 请求发送到相同的 URL：

  * ​`Access-Control-Request-Method`​ 有请求方法。
  * ​`Access-Control-Request-Headers`​ 以逗号分隔的“非安全” header 列表。
* ← 服务器应该响应状态码为 200 和 header：

  * ​`Access-Control-Allow-Methods`​ 带有允许的方法的列表，
  * ​`Access-Control-Allow-Headers`​ 带有允许的 header 的列表，
  * ​`Access-Control-Max-Age`​ 带有指定缓存权限的秒数。
* 然后，发送实际的请求，并应用之前的“安全”方案。

### [非安全请求](https://zh.javascript.info/fetch-crossorigin#fei-an-quan-qing-qiu)

我们可以使用任何 HTTP 方法：不仅仅是 `GET/POST`​，也可以是 `PATCH`​，`DELETE`​ 及其他。

之前，没有人能够设想网页能发出这样的请求。因此，可能仍然存在有些 Web 服务将非标准方法视为一个信号：“这不是浏览器”。它们可以在检查访问权限时将其考虑在内。

因此，为了避免误解，任何“非安全”请求 —— 在过去无法完成的，浏览器不会立即发出此类请求。首先，它会先发送一个初步的、所谓的“预检（preflight）”请求，来请求许可。

预检请求使用 `OPTIONS`​ 方法，它没有 body，但是有三个 header：

* ​`Access-Control-Request-Method`​ header 带有非安全请求的方法。
* ​`Access-Control-Request-Headers`​ header 提供一个以逗号分隔的非安全 HTTP-header 列表。

如果服务器同意处理请求，那么它会进行响应，此响应的状态码应该为 200，没有 body，具有 header：

* ​`Access-Control-Allow-Origin`​ 必须为 `*`​ 或进行请求的源（例如 `https://javascript.info`​）才能允许此请求。
* ​`Access-Control-Allow-Methods`​ 必须具有允许的方法。
* ​`Access-Control-Allow-Headers`​ 必须具有一个允许的 header 列表。
* 另外，header `Access-Control-Max-Age`​ 可以指定缓存此权限的秒数。因此，浏览器不是必须为满足给定权限的后续请求发送预检。

‍

​![image](assets/image-20240310115628-okwyqo6.png)​

让我们在一个跨源 `PATCH`​ 请求的例子中一步一步地看它是如何工作的（此方法经常被用于更新数据）：

```javascript
let response = await fetch('https://site.com/service.json', {
  method: 'PATCH',
  headers: {
    'Content-Type': 'application/json',
    'API-Key': 'secret'
  }
});
```

这里有三个理由解释为什么它不是一个安全请求（其实一个就够了）：

* 方法 `PATCH`​
* ​`Content-Type`​ 不是这三个中之一：`application/x-www-form-urlencoded`​，`multipart/form-data`​，`text/plain`​。
* “非安全” `API-Key`​ header。

---

[ ](https://zh.javascript.info/fetch-crossorigin#step-1-yu-jian-qing-qiu-preflightrequest)​**[预检请求（preflight request）](https://zh.javascript.info/fetch-crossorigin#step-1-yu-jian-qing-qiu-preflightrequest)**

在发送我们的请求前，浏览器会自己发送如下所示的预检请求：

```http
OPTIONS /service.json
Host: site.com
Origin: https://javascript.info
Access-Control-Request-Method: PATCH
Access-Control-Request-Headers: Content-Type,API-Key
```

* 方法：`OPTIONS`​。
* 路径 —— 与主请求完全相同：`/service.json`​。
* 特殊跨源头：

  * ​`Origin`​ —— 来源。
  * ​`Access-Control-Request-Method`​ —— 请求方法。
  * ​`Access-Control-Request-Headers`​ —— 以逗号分隔的“非安全” header 列表。

---

[ ](https://zh.javascript.info/fetch-crossorigin#step-2-yu-jian-xiang-ying-preflightresponse)​**[预检响应（preflight response）](https://zh.javascript.info/fetch-crossorigin#step-2-yu-jian-xiang-ying-preflightresponse)**

服务应响应状态 200 和 header：

* ​`Access-Control-Allow-Origin: https://javascript.info`​
* ​`Access-Control-Allow-Methods: PATCH`​
* ​`Access-Control-Allow-Headers: Content-Type,API-Key`​。

这将允许后续通信，否则会触发错误。

如果服务器将来需要其他方法和 header，则可以通过将这些方法和 header 添加到列表中来预先允许它们。

例如，此响应还允许 `PUT`​、`DELETE`​ 以及其他 header：

```http
200 OK
Access-Control-Allow-Origin: https://javascript.info
Access-Control-Allow-Methods: PUT,PATCH,DELETE
Access-Control-Allow-Headers: API-Key,Content-Type,If-Modified-Since,Cache-Control
Access-Control-Max-Age: 86400
```

现在，浏览器可以看到 `PATCH`​ 在 `Access-Control-Allow-Methods`​ 中，`Content-Type,API-Key`​ 在列表 `Access-Control-Allow-Headers`​ 中，因此它将发送主请求。

如果 `Access-Control-Max-Age`​ 带有一个表示秒的数字，则在给定的时间内，预检权限会被缓存。上面的响应将被缓存 86400 秒，也就是一天。在此时间范围内，后续请求将不会触发预检。假设它们符合缓存的配额，则将直接发送它们。

---

**[实际请求（actual request）](https://zh.javascript.info/fetch-crossorigin#step-3-shi-ji-qing-qiu-actualrequest)**

预检成功后，浏览器现在发出主请求。这里的过程与安全请求的过程相同。

主请求具有 `Origin`​ header（因为它是跨源的）：

```http
PATCH /service.json
Host: site.com
Content-Type: application/json
API-Key: secret
Origin: https://javascript.info
```

**[实际响应（actual response）](https://zh.javascript.info/fetch-crossorigin#step-4-shi-ji-xiang-ying-actualresponse)**

服务器不应该忘记在主响应中添加 `Access-Control-Allow-Origin`​。成功的预检并不能免除此要求：

```http
Access-Control-Allow-Origin: https://javascript.info
```

然后，JavaScript 可以读取主服务器响应了。

**请注意：**

**预检请求发生在“幕后”，它对 JavaScript 不可见。**

JavaScript 仅获取对主请求的响应，如果没有服务器许可，则获得一个 error。、

### [Response header](https://zh.javascript.info/fetch-crossorigin#responseheader)

对于跨源请求，默认情况下，JavaScript 只能访问“安全的” response header：

* ​`Cache-Control`​
* ​`Content-Language`​
* ​`Content-Type`​
* ​`Expires`​
* ​`Last-Modified`​
* ​`Pragma`​

> 请注意：列表中没有 `Content-Length`​ header！
>
> 该 header 包含完整的响应长度。因此，如果我们正在下载某些内容，并希望跟踪进度百分比，则需要额外的权限才能访问该 header。

要授予 JavaScript 对任何其他 response header 的访问权限，服务器必须发送 `Access-Control-Expose-Headers`​ header。它包含一个以逗号分隔的应该被设置为可访问的非安全 header 名称列表。

例如：

```http
200 OK
Content-Type:text/html; charset=UTF-8
Content-Length: 12345
API-Key: 2c9de507f2c54aa1
Access-Control-Allow-Origin: https://javascript.info
Access-Control-Expose-Headers: Content-Length,API-Key
```

有了这种 `Access-Control-Expose-Headers`​ header，此脚本就被允许读取响应的 `Content-Length`​ 和 `API-Key`​ header。

### [凭据（Credentials）](https://zh.javascript.info/fetch-crossorigin#ping-ju-credentials)

默认情况下，由 JavaScript 代码发起的跨源请求不会带来任何凭据（cookies 或者 HTTP 认证（HTTP authentication））。

这对于 HTTP 请求来说并不常见。通常，对 `http://site.com`​ 的请求附带有该域的所有 cookie。但是由 JavaScript 方法发出的跨源请求是个例外。

例如，`fetch('http://another.com')`​ 不会发送任何 cookie，即使那些 (!) 属于 `another.com`​ 域的 cookie。

这是因为具有凭据的请求比没有凭据的请求要强大得多。如果被允许，它会使用它们的凭据授予 JavaScript 代表用户行为和访问敏感信息的全部权力。

服务器真的这么信任这种脚本吗？是的，它必须显式地带有允许请求的凭据和附加 header。

要在 `fetch`​ 中发送凭据，我们需要添加 `credentials: "include"`​ 选项，像这样：

```javascript
fetch('http://another.com', {
  credentials: "include"
});
```

现在，`fetch`​ 将把源自 `another.com`​ 的 cookie 和我们的请求发送到该网站。

**注意：**

如果服务器同意接受 **带有凭据** 的请求，则除了 `Access-Control-Allow-Origin`​ 外，服务器还应该在响应中添加 header `Access-Control-Allow-Credentials: true`​。

例如：

```http
200 OK
Access-Control-Allow-Origin: https://javascript.info
Access-Control-Allow-Credentials: true
```

**对于具有凭据的请求，禁止** **​`Access-Control-Allow-Origin`​** **使用星号**  **​`*`​**​ **。如上所示，它必须有一个确切的源。这是另一项安全措施，以确保服务器真的知道它信任的发出此请求的是谁。否则浏览器不会发送实际请求，从而导致请求失败。**

### JSONP

​`script`​ 可以具有任何域的 `src`​，例如 `<script src="http://another.com/…">`​。也可以执行来自任何网站的 `script`​。

如果一个网站，例如 `another.com`​ 试图公开这种访问方式的数据，则会使用所谓的 “JSONP (JSON with padding)” 协议。

这是它的工作方式。

假设在我们的网站，需要以这种方式从 `http://another.com`​ 网站获取数据，例如天气：

1. 首先，我们先声明一个全局函数来接收数据，例如 `gotWeather`​。

    ```javascript
    // 1. 声明处理天气数据的函数
    function gotWeather({ temperature, humidity }) {
      alert(`temperature: ${temperature}, humidity: ${humidity}`);
    }
    ```
2. 然后我们创建一个特性（attribute）为 `src="http://another.com/weather.json?callback=gotWeather"`​ 的 `<script>`​ 标签，使用我们的函数名作为它的 `callback`​ URL-参数。

    ```javascript
    let script = document.createElement('script');
    script.src = `http://another.com/weather.json?callback=gotWeather`;
    document.body.append(script);
    ```
3. 远程服务器 `another.com`​ 动态生成一个脚本，该脚本调用 `gotWeather(...)`​，发送它想让我们接收的数据。

    ```javascript
    // 我们期望来自服务器的回答看起来像这样：
    gotWeather({
      temperature: 25,
      humidity: 78
    });
    ```
4. 当远程脚本加载并执行时，`gotWeather`​ 函数将运行，并且因为它是我们的函数，我们就有了需要的数据。

这是可行的，并且不违反安全规定，因为双方都同意以这种方式传递数据。而且，既然双方都同意这种行为，那这肯定不是黑客攻击了。现在仍然有提供这种访问的服务，因为即使是非常旧的浏览器它依然适用。

不久之后，网络方法出现在了浏览器 JavaScript 中。

起初，跨源请求是被禁止的。但是，经过长时间的讨论，跨源请求被允许了，但是任何新功能都需要服务器明确允许，以特殊的 header 表述。

### CORS

* 概念

  ```js
  CORS是解决跨域数据请求的终极解决方案，全称是 Cross-origin resource sharing。
  CORS技术需要浏览器和服务器同时支持，二者缺一不可：
  浏览器要支持 CORS 功能（主流的浏览器全部支持，IE 不能低于 IE10）
  服务器要开启 CORS 功能（需要后端开发者为接口开启 CORS 功能）
  ```
* 原理：服务器端通过 Access-Control-Allow-Origin 响应头，来告诉浏览器当前的 API 接口是否允许跨域请求。

  ```js
   Access-Control-Allow-Origin:*
  ```
* 优势

  ```js
  CORS 是真正的 Ajax 请求，支持 GET、POST、DELETE、PUT、PATCH 等这些常见的 Ajax 请求方式
  只需要后端开启 CORS 功能即可，前端的代码无须做任何改动

  Node.js中可以通过设置如下的请求头 允许跨域
  response.setHeader('Access-Control-Allow-Origin', '*')
  ```

### postMessage

```js
postMessage是HTML5 XMLHttpRequest Level 2中的API，且是为数不多可以跨域操作的window属性之一，它可用于解决以下方面的问题：
1.页面和其打开的新窗口的数据传递
2.多窗口之间消息传递
3.页面与嵌套的iframe消息传递
4.上面三个场景的跨域数据传递
postMessage()方法允许来自不同源的脚本采用异步方式进行有限的通信，可以实现跨文本档、多窗口、跨域消息传递 。
```

### websocket

```js
Websocket是HTML5的一个持久化的协议，它实现了浏览器与服务器的全双工通信，同时也是跨域的一种解决方案。
WebSocket和HTTP都是应用层协议，都基于 TCP 协议。
但是WebSocket 是一种双向通信协议，在建立连接之后，WebSocket 的 server 与 client 都能主动向对方发送或接收数据。
同时，WebSocket 在建立连接时需要借助 HTTP 协议，连接建立好了之后 client 与 server 之间的双向通信就与 HTTP 无关了。
原生WebSocket API使用起来不太方便，我们使用Socket.io，它很好地封装了webSocket接口，提供了更简单、灵活的接口，也对不支持webSocket的浏览器提供了向下兼容。
```

### Node中间件代理(两次跨域)

同源策略是浏览器需要遵循的标准，而如果是服务器向服务器请求就无需遵循同源策略。  
代理服务器，需要做以下几个步骤：

* 接受客户端请求 。
* 将请求转发给服务器。
* 拿到服务器响应数据。
* 将响应转发给客户端。

我们先来看个例子：本地文件index.html文件，通过代理服务器`http://localhost:3000`​向目标服务器`http://localhost:4000`​请求数据。

```xml
// index.html(http://127.0.0.1:5500)
<script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
<script>
   $.ajax({
     url: 'http://localhost:3000',
     type: 'post',
     data: { name: 'xiamen', password: '123456' },
     contentType: 'application/json;charset=utf-8',
     success: function(result) {
       console.log(result) // {"title":"fontend","password":"123456"}
     },
     error: function(msg) {
       console.log(msg)
     }
   })
</script>
```

```javascript
// server1.js 代理服务器(http://localhost:3000)
const http = require('http')
// 第一步：接受客户端请求
const server = http.createServer((request, response) => {
  // 代理服务器，直接和浏览器直接交互，需要设置CORS 的首部字段
  response.writeHead(200, {
    'Access-Control-Allow-Origin': '*',
    'Access-Control-Allow-Methods': '*',
    'Access-Control-Allow-Headers': 'Content-Type'
  })
  // 第二步：将请求转发给服务器
  const proxyRequest = http
    .request(
      {
        host: '127.0.0.1',
        port: 4000,
        url: '/',
        method: request.method,
        headers: request.headers
      },
      serverResponse => {
        // 第三步：收到服务器的响应
        var body = ''
        serverResponse.on('data', chunk => {
          body += chunk
        })
        serverResponse.on('end', () => {
          console.log('The data is ' + body)
          // 第四步：将响应结果转发给浏览器
          response.end(body)
        })
      }
    )
    .end()
})
server.listen(3000, () => {
  console.log('The proxyServer is running at http://localhost:3000')
})
复制代码
```

```javascript
// server2.js(http://localhost:4000)
const http = require('http')
const data = { title: 'fontend', password: '123456' }
const server = http.createServer((request, response) => {
  if (request.url === '/') {
    response.end(JSON.stringify(data))
  }
})
server.listen(4000, () => {
  console.log('The server is running at http://localhost:4000')
})
```

上述代码经过两次跨域，值得注意的是浏览器向代理服务器发送请求，也遵循同源策略，最后在index.html文件打印出`{"title":"fontend","password":"123456"}`​

### nginx

实现原理类似于Node中间件代理，需要你搭建一个中转nginx服务器，用于转发请求。

使用nginx反向代理实现跨域，是最简单的跨域方式。只需要修改nginx的配置即可解决跨域问题，支持所有浏览器，支持session，不需要修改任何代码，并且不会影响服务器性能。

实现思路：通过nginx配置一个代理服务器（域名与domain1相同，端口不同）做跳板机，反向代理访问domain2接口，并且可以顺便修改cookie中domain信息，方便当前域cookie写入，实现跨域登录。

先下载[nginx](http://nginx.org/en/download.html)，然后将nginx目录下的nginx.conf修改如下:

```ini
// proxy服务器
server {
  listen       81;
  server_name  www.domain1.com;
  location / {
    proxy_pass   http://www.domain2.com:8080;  #反向代理
    proxy_cookie_domain www.domain2.com www.domain1.com; #修改cookie里域名
    index  index.html index.htm;

    # 当用webpack-dev-server等中间件代理接口访问nignx时，此时无浏览器参与，故没有同源限制，下面的跨域配置可不启用
    add_header Access-Control-Allow-Origin http://www.domain1.com;  #当前端只跨域不带cookie时，可为*
    add_header Access-Control-Allow-Credentials true;
  }
}
```

最后通过命令行`nginx -s reload`​启动nginx

```csharp
// index.html
var xhr = new XMLHttpRequest();
// 前端开关：浏览器是否读写cookie
xhr.withCredentials = true;
// 访问nginx中的代理服务器
xhr.open('get', 'http://www.domain1.com:81/?user=admin', true);
xhr.send();
```

```ini
// server.js
var http = require('http');
var server = http.createServer();
var qs = require('querystring');
server.on('request', function(req, res) {
    var params = qs.parse(req.url.substring(2));
    // 向前台写cookie
    res.writeHead(200, {
        'Set-Cookie': 'l=a123456;Path=/;Domain=www.domain2.com;HttpOnly'   // HttpOnly:脚本无法读取
    });
    res.write(JSON.stringify(params));
    res.end();
});
server.listen('8080');
console.log('Server is running at port 8080...');
```

## 报文

* 请求报文：在使用`HTTP`​和服务器进行通讯的时候,发送的叫做请求报文，请求报文中的绝大多数内容是自动生成。

  ```js
  1. 请求行（request line）：有请求的方法和地址
  2. 请求头部（ header ）
  3. 空行
  4. 请求体 4 个部分组成。
  ```

  ​![image-20220613110032277](assets/image-20220613110032277-20220801165426-zp21wvr.png)​
* 响应报文：在使用HTTP和服务器进行通讯的时候,服务器响应叫做请求报文， 响应报文中的主体内容在响应(主)体

  ```js
  1. 状态行：有响应状态码
  2. 响应头部
  3. 空行
  4. 响应体 4 个部分组成。
  ```

  ​![image](assets/image-20240310142021-xj9jilq.png)​

## 状态码

* HTTP状态码：`http`​ 响应状态码（Status Code）由三位数字组成，用来标识响应成功与否的状态

  ```js
  100//需要后续请求
  200//请求成功。                           
  201//资源在服务器端已成功创建。
  302//重定向
  304//资源在客户端被缓存，响应体中不包含任何资源内容！   
  400//客户端的请求方式、或请求参数有误导致的请求失败！   
  401//客户端的用户身份认证未通过，导致的此次请求失败！ 
  403//服务器拒绝请求  
  404 // 客户端请求的资源地址错误，导致服务器无法找到资源！ 
  500//服务器内部错误，导致的本次请求失败！
  ```
* 业务状态码：业务状态码是服务端给出的关于业务描述的码，用于客户端明确得知本次请求的资源的状态情况。

  ```js
  1.位置不同
  在状态行中所包含的状态码，叫做响应状态码
  在响应体的数据中所包含的状态码，叫做业务状态码
  2.表示的结果不同
  响应状态码只能表示这次请求的成功与否（成功或失败）
  业务状态码用来表示这次业务处理的成功与否
  3.通用性不同
  响应状态码是由http协议规定的，具有通用性。每个不同的状态码都有其标准的含义，不能乱用。
  业务状态码是**后端程序员**自定义的，不具有通用性。
  ```

‍

## [fetch](https://zh.javascript.info/fetch-api)

```js
let promise = fetch(url, [options])

let promise = fetch(url, {
  method: "GET", // POST，PUT，DELETE，等。
  headers: {
    // 内容类型 header 值通常是自动设置的
    // 取决于 request body
    "Content-Type": "text/plain;charset=UTF-8"
  },
  body: undefined // string，FormData，Blob，BufferSource，或 URLSearchParams
  referrer: "about:client", // 或 "" 以不发送 Referer header，
  // 或者是当前源的 url
  referrerPolicy: "no-referrer-when-downgrade", // no-referrer，origin，same-origin...
  mode: "cors", // same-origin，no-cors
  credentials: "same-origin", // omit，include
  cache: "default", // no-store，reload，no-cache，force-cache，或 only-if-cached
  redirect: "follow", // manual，error
  integrity: "", // 一个 hash，像 "sha256-abcdef1234567890"
  keepalive: false, // true
  signal: undefined, // AbortController 来中止请求
  window: window // null
})
  .then((respones)=>{response.json()})
  .then(esult => /* process result */)
```

### 请求过程

```js
let promise = fetch(url, [options])
```

* ​**​`url`​**​ —— 要访问的 URL。
* ​**​`options`​**​ —— 可选参数：method，header 等。

没有 `options`​，这就是一个简单的 GET 请求，下载 `url`​ 的内容。

浏览器立即启动请求，并返回一个该调用代码应该用来获取结果的 `promise`​。

获取响应通常需要经过两个阶段。

➡️**第一阶段，当服务器发送了响应头（response header），**​**​`fetch`​**​ **返回的** **​`promise`​**​ **就使用内建的** **[Response](https://fetch.spec.whatwg.org/#response-class)** **class 对象来对响应头进行解析。**

在这个阶段，我们可以通过检查响应头，来检查 HTTP 状态以确定请求是否成功，当前还没有响应体（response body）。

如果 `fetch`​ 无法建立一个 HTTP 请求，例如网络问题，亦或是请求的网址不存在，那么 promise 就会 reject。异常的 HTTP 状态，例如 404 或 500，不会导致出现 error。

我们可以在 response 的属性中看到 HTTP 状态：

* ​**​`status`​**​ —— HTTP 状态码，例如 200。
* ​**​`ok`​**​ —— 布尔值，如果 HTTP 状态码为 200-299，则为 `true`​。

例如：

```javascript
let response = await fetch(url);

if (response.ok) { // 如果 HTTP 状态码为 200-299
  // 获取 response body（此方法会在下面解释）
  let json = await response.json();
} else {
  alert("HTTP-Error: " + response.status);
}
```

 **➡️第二阶段，为了获取 response body，我们需要使用一个其他的方法调用。**

​`Response`​ 提供了多种基于 promise 的方法，来以不同的格式访问 body：

```js
let response = await fetch('url')
let text = awati response.text() //读取成文本格式
let json = await response.json(); // 读取 response body，并将其解析为 JSON 格式
let blob = await response.blob(); // 下载为 Blob 对象
let formData= await response.formData(); // 下载为 FormData 对象
let body= await response.body(); // 下载为 ReadableStream 对象，流式读取
```

---

* ​`response.text()`​ —— 读取 response，并以文本形式返回 response，

  ```js
  let response = await fetch('https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits');

  let text = await response.text(); // 将 response body 读取为文本

  alert(text.slice(0, 80) + '...');
  ```
* ​`response.json()`​ —— 将 response 解析为 JSON 格式，

  ```js
  let url = 'https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits';
  let response = await fetch(url);

  let commits = await response.json(); // 读取 response body，并将其解析为 JSON 格式

  alert(commits[0].author.login);
  ```
* ​`response.formData()`​ —— 以 `FormData`​ 对象（在 [下一章](https://zh.javascript.info/formdata) 有解释）的形式返回 response，
* ​`response.blob()`​ —— 以 [Blob](https://zh.javascript.info/blob)（具有类型的二进制数据）形式返回 response，

  ```js
  let response = await fetch('/article/fetch/logo-fetch.svg');

  let blob = await response.blob(); // 下载为 Blob 对象

  // 为其创建一个 <img>
  let img = document.createElement('img');
  img.style = 'position:fixed;top:10px;left:10px;width:100px';
  document.body.append(img);

  // 显示它
  img.src = URL.createObjectURL(blob);

  setTimeout(() => { // 3 秒后将其隐藏
    img.remove();
    URL.revokeObjectURL(img.src);
  }, 3000);
  ```
* ​`response.arrayBuffer()`​ —— 以 [ArrayBuffer](https://zh.javascript.info/arraybuffer-binary-arrays)（低级别的二进制数据）形式返回 response，
* 另外，`response.body`​ 是 [ReadableStream](https://streams.spec.whatwg.org/#rs-class) 对象，它允许你逐块读取 body，我们稍后会用一个例子解释它。

> **重要：**
>
> 我们只能选择一种读取 body 的方法。
>
> 如果我们已经使用了 `response.text()`​ 方法来获取 response，那么如果再用 `response.json()`​，则不会生效，因为 body 内容已经被处理过了。
>
> ```javascript
> let text = await response.text(); // response body 被处理了
> let parsed = await response.json(); // 失败（已经被处理过了）
> ```

### Request header

要在 `fetch`​ 中设置 request header，我们可以使用 `headers`​ 选项。它有一个带有输出 header 的对象，如下所示：

```javascript
let response = fetch(protectedUrl, {
  headers: {
    Authentication: 'secret'
  }
});
```

……但是有一些我们无法设置的 header（详见 [forbidden HTTP headers](https://fetch.spec.whatwg.org/#forbidden-header-name)）：

* ​`Accept-Charset`​, `Accept-Encoding`​
* ​`Access-Control-Request-Headers`​
* ​`Access-Control-Request-Method`​
* ​`Connection`​
* ​`Content-Length`​
* ​`Cookie`​, `Cookie2`​
* ​`Date`​
* ​`DNT`​
* ​`Expect`​
* ​`Host`​
* ​`Keep-Alive`​
* ​`Origin`​
* ​`Referer`​
* ​`TE`​
* ​`Trailer`​
* ​`Transfer-Encoding`​
* ​`Upgrade`​
* ​`Via`​
* ​`Proxy-*`​
* ​`Sec-*`​

这些 header 保证了 HTTP 的正确性和安全性，所以它们仅由浏览器控制。

### Response header

Response header 位于 `response.headers`​ 中的一个类似于 Map 的 header 对象。

它不是真正的 Map，但是它具有类似的方法，我们可以按名称（name）获取各个 header，或迭代它们：

```javascript
let response = await fetch('https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits');

// 获取一个 header
alert(response.headers.get('Content-Type')); // application/json; charset=utf-8

// 迭代所有 header
for (let [key, value] of response.headers) {
  alert(`${key} = ${value}`);
}
```

### 下载进度

​`fetch`​ 方法允许去跟踪 **下载** 进度。

请注意：到目前为止，`fetch`​ 方法无法跟踪 **上传** 进度。对于这个目的，请使用 [XMLHttpRequest](https://zh.javascript.info/xmlhttprequest)

要跟踪下载进度，我们可以使用 `response.body`​ 属性。它是 `ReadableStream`​ —— 一个特殊的对象，它可以逐块（chunk）提供 body。在 [Streams API](https://streams.spec.whatwg.org/#rs-class) 规范中有对 `ReadableStream`​ 的详细描述。

与 `response.text()`​，`response.json()`​ 和其他方法不同，`response.body`​ 给予了对进度读取的完全控制，我们可以随时计算下载了多少。

这是从 `response.body`​ 读取 response 的示例代码：

```javascript
// 代替 response.json() 以及其他方法
const reader = response.body.getReader();
// 在 body 下载时，一直为无限循环
while(true) {
  // 当最后一块下载完成时，done 值为 true
  // value 是块字节的 Uint8Array
  const {done, value} = await reader.read();

  if (done) {
    break;
  }

  console.log(`Received ${value.length} bytes`)
}
```

​`await reader.read()`​ 调用的结果是一个具有两个属性的对象：

* ​**​`done`​**​ —— 当读取完成时为 `true`​，否则为 `false`​。
* ​**​`value`​**​ —— 字节的类型化数组：`Uint8Array`​。

> **请注意：**
>
> Streams API 还描述了如果使用 `for await..of`​ 循环异步迭代 `ReadableStream`​，但是目前为止，它还未得到很好的支持（参见 [浏览器问题](https://github.com/whatwg/streams/issues/778#issuecomment-461341033)），所以我们使用了 `while`​ 循环。
>
> 我们在循环中接收响应块（response chunk），直到加载完成，也就是：直到 `done`​ 为 `true`​。
>
> 要将进度打印出来，我们只需要将每个接收到的片段 `value`​ 的长度（length）加到 counter 即可。
>
> 这是获取响应，并在控制台中记录进度的完整工作示例，下面有更多说明：

---

```javascript
// Step 1：启动 fetch，并获得一个 reader
let response = await fetch('https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits?per_page=100');
const reader = response.body.getReader();

// Step 2：获得总长度（length）
const contentLength = +response.headers.get('Content-Length');

// Step 3：读取数据
let receivedLength = 0; // 当前接收到了这么多字节
let chunks = []; // 接收到的二进制块的数组（包括 body）
while(true) {
  const {done, value} = await reader.read();
  if (done) {
    break;
  }
  chunks.push(value);
  receivedLength += value.length;
  console.log(`Received ${receivedLength} of ${contentLength}`)
}

// Step 4：将块连接到单个 Uint8Array
let chunksAll = new Uint8Array(receivedLength); // (4.1)
let position = 0;
for(let chunk of chunks) {
  chunksAll.set(chunk, position); // (4.2)
  position += chunk.length;
}

// Step 5：解码成字符串
let result = new TextDecoder("utf-8").decode(chunksAll);

// 我们完成啦！
let commits = JSON.parse(result);
alert(commits[0].author.login);
```

1. 我们像往常一样执行 `fetch`​，但不是调用 `response.json()`​，而是获得了一个流读取器（stream reader）`response.body.getReader()`​。  
    请注意，我们不能同时使用这两种方法来读取相同的响应。要么使用流读取器，要么使用 reponse 方法来获取结果。
2. 在读取数据之前，我们可以从 `Content-Length`​ header 中得到完整的响应长度。  
    跨源请求中可能不存在这个 header（请参见 [Fetch：跨源请求](https://zh.javascript.info/fetch-crossorigin)），并且从技术上讲，服务器可以不设置它。但是通常情况下它都会在那里。
3. 调用 `await reader.read()`​，直到它完成。  
    我们将响应块收集到数组 `chunks`​ 中。这很重要，因为在使用完（consumed）响应后，我们将无法使用 `response.json()`​ 或者其他方式（你可以试试，将会出现 error）去“重新读取”它。
4. 最后，我们有了一个 `chunks`​ —— 一个 `Uint8Array`​ 字节块数组。我们需要将这些块合并成一个结果。但不幸的是，没有单个方法可以将它们串联起来，所以这里需要一些代码来实现：

    1. 我们创建 `chunksAll = new Uint8Array(receivedLength)`​ —— 一个具有所有数据块合并后的长度的同类型数组。
    2. 然后使用 `.set(chunk, position)`​ 方法，从数组中一个个地复制这些 `chunk`​。
5. 我们的结果现在储存在 `chunksAll`​ 中。但它是一个字节数组，不是字符串。  
    要创建一个字符串，我们需要解析这些字节。可以使用内建的 [TextDecoder](https://zh.javascript.info/text-decoder) 对象完成。然后，我们可以 `JSON.parse`​ 它，如果有必要的话。  
    如果我们需要的是二进制内容而不是字符串呢？这更简单。用下面这行代码替换掉第 4 和第 5 步，这行代码从所有块创建一个 `Blob`​：

    ```javascript
    let blob = new Blob(chunks);
    ```

最后，我们得到了结果（以字符串或 blob 的形式表示，什么方便就用什么），并在过程中对进度进行了跟踪。

再强调一遍，这不能用于 **上传** 过程（现在无法通过 `fetch`​ 获取），仅用于 **下载** 过程。

另外，如果大小未知，我们应该检查循环中的 `receivedLength`​，一旦达到一定的限制就将其中断。这样 `chunks`​ 就不会溢出内存了。

### AbortController

正如我们所知道的，`fetch`​ 返回一个 promise。JavaScript 通常并没有“中止” promise 的概念。那么我们怎样才能取消一个正在执行的 `fetch`​ 呢？例如，如果用户在我们网站上的操作表明不再需要某个执行中的 `fetch`​。

为此有一个特殊的内建对象：`AbortController`​。它不仅可以中止 `fetch`​，还可以中止其他异步任务。

创建一个控制器（controller）：

```javascript
let controller = new AbortController();
```

控制器是一个极其简单的对象。

* 它具有单个方法 `abort()`​，
* 和单个属性 `signal:{aborted,reason,onabort}`​对象，我们可以在这个属性上设置事件监听器。

当 `abort()`​ 被调用时：

* ​`controller.signal`​ 就会触发 `abort`​ 事件。
* ​`controller.signal.aborted`​ 属性变为 `true`​。

通常，我们需要处理两部分：

1. 一部分是通过在 `controller.signal`​ 上添加一个监听器，来执行可取消操作。
2. 另一部分是触发取消：在需要的时候调用 `controller.abort()`​。

这是完整的示例（目前还没有 `fetch`​）：

```javascript
let controller = new AbortController();
let signal = controller.signal;

// 执行可取消操作部分
// 获取 "signal" 对象，
// 并将监听器设置为在 controller.abort() 被调用时触发
signal.addEventListener('abort', () => alert("abort!"));

// 另一部分，取消（在之后的任何时候）：
controller.abort(); // 中止！

// 事件触发，signal.aborted 变为 true
alert(signal.aborted); // true
```

正如我们所看到的，`AbortController`​ 只是在 `abort()`​ 被调用时传递 `abort`​ 事件的一种方式。

我们可以自己在代码中实现相同类型的事件监听，而不需要 `AbortController`​ 对象。

但是有价值的是，`fetch`​ 知道如何与 `AbortController`​ 对象一起工作。它们是集成在一起的。

---

为了能够取消 `fetch`​，请将 `AbortController`​ 的 `signal`​ 属性作为 `fetch`​ 的一个可选参数（option）进行传递：

```javascript
let controller = new AbortController();
fetch(url, {
  signal: controller.signal
});
```

​`fetch`​ 方法知道如何与 `AbortController`​ 一起工作。它会监听 `signal`​ 上的 `abort`​ 事件。

现在，想要中止 `fetch`​，调用 `controller.abort()`​ 即可：

```javascript
controller.abort();
```

我们完成啦：`fetch`​ 从 `signal`​ 获取了事件并中止了请求。

当一个 fetch 被中止，它的 promise 就会以一个 error `AbortError`​ reject，因此我们应该对其进行处理，例如在 `try..catch`​ 中。

这是完整的示例，其中 `fetch`​ 在 1 秒后中止：

```javascript
// 1 秒后中止
let controller = new AbortController();
setTimeout(() => controller.abort(), 1000);

try {
  let response = await fetch('/article/fetch-abort/demo/hang', {
    signal: controller.signal
  });
} catch(err) {
  if (err.name == 'AbortError') { // handle abort()
    alert("Aborted!");
  } else {
    throw err;
  }
}
```

## XMLHttpRequest

​`XMLHttpRequest`​ 是一个内建的浏览器对象，它允许使用 JavaScript 发送 HTTP 请求。

虽然它的名字里面有 “XML” 一词，但它可以操作任何数据，而不仅仅是 XML 格式。我们可以用它来上传/下载文件，跟踪进度等。

现如今，我们有一个更为现代的方法叫做 `fetch`​，它的出现使得 `XMLHttpRequest`​ 在某种程度上被弃用。

在现代 Web 开发中，出于以下三种原因，我们还在使用 `XMLHttpRequest`​：

1. 历史原因：我们需要支持现有的使用了 `XMLHttpRequest`​​ 的脚本。
2. 我们需要兼容旧浏览器，并且不想用 polyfill（例如为了使脚本更小）。
3. 我们需要做一些 `fetch`​​ 目前无法做到的事情，例如跟踪上传进度。

### 创建请求

​`XMLHttpRequest `​有两种执行模式：**同步（synchronous）和异步（asynchronous）。**

要发送请求，需要 3 个步骤：

1. 创建 `XMLHttpRequest`​：此构造器没有参数。

    ```javascript
    let xhr = new XMLHttpRequest();
    ```
2. 初始化它，通常就在 `new XMLHttpRequest`​ 之后：

    ```javascript
    xhr.open(method, URL, [async, user, password])
    ```

    此方法指定请求的主要参数：

    * ​`method`​ —— HTTP 方法。通常是 `"GET"`​ 或 `"POST"`​。
    * ​`URL`​ —— 要请求的 URL，通常是一个字符串，也可以是 [URL](https://zh.javascript.info/url) 对象。
    * ​`async`​ —— 如果显式地设置为 `false`​，那么请求将会以同步的方式处理，我们稍后会讲到它。
    * ​`user`​，`password`​ —— HTTP 基本身份验证（如果需要的话）的登录名和密码。

    请注意，`open`​ 调用与其名称相反，不会建立连接。它仅配置请求，而网络活动仅以 `send`​ 调用开启。
3. 发送请求。

    ```javascript
    xhr.send([body])
    ```

    这个方法会建立连接，并将请求发送到服务器。可选参数 `body`​ 包含了 request body。  
    一些请求方法，像 `GET`​ 没有 request body。还有一些请求方法，像 `POST`​ 使用 `body`​ 将数据发送到服务器。我们稍后会看到相应示例。
4. 监听 `xhr`​ 事件以获取响应。  
    这三个事件是最常用的：

    * ​`load`​ —— 当请求完成（即使 HTTP 状态为 400 或 500 等），并且响应已完全下载。
    * ​`error`​ —— 当无法发出请求，例如网络中断或者无效的 URL。
    * ​`progress`​ —— 在下载响应期间定期触发，报告已经下载了多少。

    ```javascript
    xhr.onload = function() {
      alert(`Loaded: ${xhr.status} ${xhr.response}`);
    };

    xhr.onerror = function() { // 仅在根本无法发出请求时触发
      alert(`Network Error`);
    };

    xhr.onprogress = function(event) { // 定期触发
      // event.loaded —— 已经下载了多少字节
      // event.lengthComputable = true，当服务器发送了 Content-Length header 时
      // event.total —— 总字节数（如果 lengthComputable 为 true）
      alert(`Received ${event.loaded} of ${event.total}`);
    };
    ```

下面是一个完整的示例。它从服务器加载 `/article/xmlhttprequest/example/load`​，并打印加载进度：

```javascript
// 1. 创建一个 new XMLHttpRequest 对象
let xhr = new XMLHttpRequest();

// 2. 配置它：从 URL /article/.../load GET-request
xhr.open('GET', '/article/xmlhttprequest/example/load');

// 3. 通过网络发送请求
xhr.send();

// 4. 当接收到响应后，将调用此函数
xhr.onload = function() {
  if (xhr.status != 200) { // 分析响应的 HTTP 状态
    alert(`Error ${xhr.status}: ${xhr.statusText}`); // 例如 404: Not Found
  } else { // 显示结果
    alert(`Done, got ${xhr.response.length} bytes`); // response 是服务器响应
  }
};

xhr.onprogress = function(event) {
  if (event.lengthComputable) {
    alert(`Received ${event.loaded} of ${event.total} bytes`);
  } else {
    alert(`Received ${event.loaded} bytes`); // 没有 Content-Length
  }

};

xhr.onerror = function() {
  alert("Request failed");
};
```

一旦服务器有了响应，我们可以在以下 `xhr`​ 属性中接收结果：

​`status`​HTTP 状态码（一个数字）：`200`​，`404`​，`403`​ 等，如果出现非 HTTP 错误，则为 `0`​。`statusText`​HTTP 状态消息（一个字符串）：状态码为 `200`​ 对应于 `OK`​，`404`​ 对应于 `Not Found`​，`403`​ 对应于 `Forbidden`​。`response`​（旧脚本可能用的是 `responseText`​）服务器 response body。我们还可以使用相应的属性指定超时（timeout）：

```javascript
xhr.timeout = 10000; // timeout 单位是 ms，此处即 10 秒
```

如果在给定时间内请求没有成功执行，请求就会被取消，并且触发 `timeout`​ 事件。

> **URL 搜索参数（URL search parameters）**
>
> 为了向 URL 添加像 `?name=value`​ 这样的参数，并确保正确的编码，我们可以使用 [URL](https://zh.javascript.info/url) 对象：
>
> ```javascript
> let url = new URL('https://google.com/search');
> url.searchParams.set('q', 'test me!');
>
> // 参数 'q' 被编码
> xhr.open('GET', url); // https://google.com/search?q=test+me%21
> ```

### [同步请求](https://zh.javascript.info/xmlhttprequest#tong-bu-qing-qiu)

如果在 `open`​ 方法中将第三个参数 `async`​ 设置为 `false`​，那么请求就会以同步的方式进行。

换句话说，JavaScript 执行在 `send()`​ 处暂停，并在收到响应后恢复执行。这有点儿像 `alert`​ 或 `prompt`​ 命令。

下面是重写的示例，`open`​ 的第三个参数为 `false`​：

```javascript
let xhr = new XMLHttpRequest();

xhr.open('GET', '/article/xmlhttprequest/hello.txt', false);

try {
  xhr.send();
  if (xhr.status != 200) {
    alert(`Error ${xhr.status}: ${xhr.statusText}`);
  } else {
    alert(xhr.response);
  }
} catch(err) { // 代替 onerror
  alert("Request failed");
}
```

这看起来好像不错，但是很少使用同步调用，因为它们会阻塞页面内的 JavaScript，直到加载完成。在某些浏览器中，滚动可能无法正常进行。如果一个同步调用执行时间过长，浏览器可能会建议关闭“挂起（hanging）”的网页。

​`XMLHttpRequest`​ 的很多高级功能在同步请求中都不可用，例如向其他域发起请求或者设置超时。并且，正如你所看到的，没有进度指示。

基于这些原因，同步请求使用的非常少，几乎从不使用。在这我们就不再讨论它了。

### [跨源请求](https://zh.javascript.info/xmlhttprequest#kua-yuan-qing-qiu)

​`XMLHttpRequest`​ 可以使用和 [fetch](https://zh.javascript.info/fetch-crossorigin) 相同的 CORS 策略进行跨源请求。

就像 `fetch`​ 一样，默认情况下不会将 cookie 和 HTTP 授权发送到其他域。要启用它们，可以将 `xhr.withCredentials`​ 设置为 `true`​：

```javascript
let xhr = new XMLHttpRequest();
xhr.withCredentials = true;
xhr.open('POST', 'http://anywhere.com/request');
...
```

### ResponseType

我们可以使用 `xhr.responseType`​​ 属性来设置**响应格式**：

* ​`""`​（默认）—— 响应格式为字符串，
* ​`"text"`​ —— 响应格式为字符串，
* ​`"arraybuffer"`​ —— 响应格式为 `ArrayBuffer`​（对于二进制数据，请参见 [ArrayBuffer，二进制数组](https://zh.javascript.info/arraybuffer-binary-arrays)），
* ​`"blob"`​ —— 响应格式为 `Blob`​（对于二进制数据，请参见 [Blob](https://zh.javascript.info/blob)），
* ​`"document"`​ —— 响应格式为 XML document（可以使用 XPath 和其他 XML 方法）或 HTML document（基于接收数据的 MIME 类型）
* ​`"json"`​ —— 响应格式为 JSON（自动解析）。

例如，我们以 JSON 格式获取响应：

```javascript
let xhr = new XMLHttpRequest();

xhr.open('GET', '/article/xmlhttprequest/example/json');

xhr.responseType = 'json';

xhr.send();

// 响应为 {"message": "Hello, world!"}
xhr.onload = function() {
  let responseObj = xhr.response;
  alert(responseObj.message); // Hello, world!
};
```

> **请注意：**
>
> 在旧的脚本中，你可能会看到 `xhr.responseText`​，甚至会看到 `xhr.responseXML`​ 属性。
>
> 它们是由于历史原因而存在的，以获取字符串或 XML 文档。如今，我们应该在 `xhr.responseType`​ 中设置格式，然后就能获取如上所示的 `xhr.response`​ 了。

### [readyState](https://zh.javascript.info/xmlhttprequest#readystate)

​`XMLHttpRequest`​ 的状态（state）会随着它的处理进度变化而变化。可以通过 `xhr.readyState`​ 来了解当前状态。

[规范](https://xhr.spec.whatwg.org/#states) 中提到的所有状态如下：

```javascript
UNSENT = 0; // 初始状态
OPENED = 1; // open 被调用
HEADERS_RECEIVED = 2; // 接收到 response header
LOADING = 3; // 响应正在被加载（接收到一个数据包）
DONE = 4; // 请求完成
```

​`XMLHttpRequest`​ 对象以 `0`​ → `1`​ → `2`​ → `3`​ → … → `3`​ → `4`​ 的顺序在它们之间转变。每当通过网络接收到一个数据包，就会重复一次状态 `3`​。

我们可以使用 `readystatechange`​ 事件来跟踪它们：

```javascript
xhr.onreadystatechange = function() {
  if (xhr.readyState == 3) {
    // 加载中
  }
  if (xhr.readyState == 4) {
    // 请求完成
  }
};
```

你可能在非常老的代码中找到 `readystatechange`​ 这样的事件监听器，它的存在是有历史原因的，因为曾经有很长一段时间都没有 `load`​ 以及其他事件。如今，它已被 `load/error/progress`​ 事件处理程序所替代。

### [Aborting](https://zh.javascript.info/xmlhttprequest#zhong-zhi-qing-qiu-aborting)

我们可以随时终止请求。调用 `xhr.abort()`​ 即可：

```javascript
xhr.abort(); // 终止请求
```

它会触发 `abort`​ 事件，且 `xhr.status`​ 变为 `0`​。

### [HTTP-header](https://zh.javascript.info/xmlhttprequest#httpheader)

​`XMLHttpRequest`​ 允许发送自定义 header，并且可以从响应中读取 header。

HTTP-header 有三种方法：

​`setRequestHeader(name, value)`​使用给定的 `name`​ 和 `value`​ 设置 request header。

例如：

```javascript
xhr.setRequestHeader('Content-Type', 'application/json');
```

> **Header 的限制**
>
> 一些 header 是由浏览器专门管理的，例如 `Referer`​ 和 `Host`​。 完整列表请见 [规范](https://xhr.spec.whatwg.org/#the-setrequestheader()-method)。
>
> 为了用户安全和请求的正确性，`XMLHttpRequest`​ 不允许更改它们。

---

> **不能移除 header**
>
> ​`XMLHttpRequest`​ 的另一个特点是不能撤销 `setRequestHeader`​。
>
> 一旦设置了 header，就无法撤销了。其他调用会向 header 中添加信息，但不会覆盖它。
>
> 例如：
>
> ```javascript
> xhr.setRequestHeader('X-Auth', '123');
> xhr.setRequestHeader('X-Auth', '456');
>
> // header 将是：
> // X-Auth: 123, 456
> ```

​`getResponseHeader(name)`​获取具有给定 `name`​ 的 header（`Set-Cookie`​ 和 `Set-Cookie2`​ 除外）。

例如：

```javascript
xhr.getResponseHeader('Content-Type')
```

​`getAllResponseHeaders()`​返回除 `Set-Cookie`​ 和 `Set-Cookie2`​ 外的所有 response header。

header 以单行形式返回，例如：

```http
Cache-Control: max-age=31536000
Content-Length: 4260
Content-Type: image/png
Date: Sat, 08 Sep 2012 16:53:16 GMT
```

**header 之间的换行符始终为**  **​`"\r\n"`​**​ **（不依赖于操作系统）** ，所以我们可以很容易地将其拆分为单独的 header。

name 和 value 之间总是以冒号后跟一个空格 `": "`​ 分隔。这是标准格式。

因此，如果我们想要获取具有 name/value 对的对象，则需要用一点 JavaScript 代码来处理它们。

像这样（假设如果两个 header 具有相同的名称，那么后者就会覆盖前者）：

```javascript
let headers = xhr
  .getAllResponseHeaders()
  .split('\r\n')
  .reduce((result, current) => {
    let [name, value] = current.split(': ');
    result[name] = value;
    return result;
  }, {});

// headers['Content-Type'] = 'image/png'
```

### [上传进度](https://zh.javascript.info/xmlhttprequest#shang-chuan-jin-du)

​`progress`​ 事件仅在下载阶段触发。

也就是说：如果我们 `POST`​ 一些内容，`XMLHttpRequest`​ 首先上传我们的数据（request body），然后下载响应。

如果我们要上传的东西很大，那么我们肯定会对跟踪上传进度感兴趣。但是 `xhr.onprogress`​ 在这里并不起作用。

这里有另一个对象，它没有方法，它专门用于跟踪上传事件：`xhr.upload`​。

它会生成事件，类似于 `xhr`​，但是 `xhr.upload`​ 仅在上传时触发它们：

* ​`loadstart`​ —— 上传开始。
* ​`progress`​ —— 上传期间定期触发。
* ​`abort`​ —— 上传中止。
* ​`error`​ —— 非 HTTP 错误。
* ​`load`​ —— 上传成功完成。
* ​`timeout`​ —— 上传超时（如果设置了 `timeout`​ 属性）。
* ​`loadend`​ —— 上传完成，无论成功还是 error。

handler 示例：

```javascript
xhr.upload.onprogress = function(event) {
  alert(`Uploaded ${event.loaded} of ${event.total} bytes`);
};

xhr.upload.onload = function() {
  alert(`Upload finished successfully.`);
};

xhr.upload.onerror = function() {
  alert(`Error during the upload: ${xhr.status}`);
};
```

这是一个真实示例：带有进度指示的文件上传：

```js
<input type="file" onchange="upload(this.files[0])">

<script>
function upload(file) {
  let xhr = new XMLHttpRequest();

  // 跟踪上传进度
  xhr.upload.onprogress = function(event) {
    console.log(`Uploaded ${event.loaded} of ${event.total}`);
  };

  // 跟踪完成：无论成功与否
  xhr.onloadend = function() {
    if (xhr.status == 200) {
      console.log("success");
    } else {
      console.log("error " + this.status);
    }
  };

  xhr.open("POST", "/article/xmlhttprequest/post/upload");
  xhr.send(file);
}
</script>
```

### 生命周期

使用 `XMLHttpRequest`​ 的 GET 请求的典型代码：

实际上还有很多事件，在 [现代规范](https://xhr.spec.whatwg.org/#events) 中有详细列表（按生命周期排序）：

* ​`loadstart`​ —— 请求开始。
* ​`progress`​ —— 一个响应数据包到达，此时整个 response body 都在 `response`​ 中。
* ​`abort`​ —— 调用 `xhr.abort()`​ 取消了请求。
* ​`error`​ —— 发生连接错误，例如，域错误。不会发生诸如 404 这类的 HTTP 错误。
* ​`load`​ —— 请求成功完成。
* ​`timeout`​ —— 由于请求超时而取消了该请求（仅发生在设置了 timeout 的情况下）。
* ​`loadend`​ —— 在 `load`​，`error`​，`timeout`​ 或 `abort`​ 之后触发。

​`error`​，`abort`​，`timeout`​ 和 `load`​ 事件是互斥的。其中只有一种可能发生。

最常用的事件是加载完成（`load`​），加载失败（`error`​），或者我们可以使用单个 `loadend`​ 处理程序并检查请求对象 `xhr`​ 的属性，以查看发生了什么。

我们还了解了另一个事件：`readystatechange`​。由于历史原因，它早在规范制定之前就出现了。如今我们已经无需使用它了，我们可以用新的事件代替它，但通常可以在旧的代码中找到它。

如果我们需要专门跟踪上传，那么我们应该在 `xhr.upload`​ 对象上监听相同的事件。

### 恢复文件上传

使用 `fetch`​ 方法来上传文件相当容易。

连接断开后如何恢复上传？这里没有对此的内建选项，但是我们有实现它的一些方式。

对于大文件（如果我们可能需要恢复），可恢复的上传应该带有上传进度提示。由于 `fetch`​ 不允许跟踪上传进度，我们将会使用 [XMLHttpRequest](https://zh.javascript.info/xmlhttprequest)。

---

要恢复上传，我们需要知道在连接断开前已经上传了多少。

我们有 `xhr.upload.onprogress`​ 来跟踪上传进度。

不幸的是，它不会帮助我们在此处恢复上传，因为它会在数据 **被发送** 时触发，**但是服务器是否接收到了？浏览器并不知道。**

或许它是由本地网络代理缓冲的（buffered），或者可能是远程服务器进程刚刚终止而无法处理它们，亦或是它在中间丢失了，并没有到达服务器。

这就是为什么此事件仅适用于显示一个好看的进度条。

要恢复上传，我们需要 **确切地** 知道服务器接收的字节数。而且只有服务器能告诉我们，因此，我们将发出一个额外的请求。

---

[一个示例算法](https://zh.javascript.info/resume-upload#suan-fa)

1. 首先，创建一个文件 id，以唯一地标识我们要上传的文件：

    ```javascript
    let fileId = file.name + '-' + file.size + '-' + file.lastModified;
    ```

    在恢复上传时需要用到它，以告诉服务器我们要恢复的内容。  
    如果名称，或大小，或最后一次修改时间发生了更改，则将有另一个 `fileId`​。
2. 向服务器发送一个请求，询问它已经有了多少字节，像这样：

    ```javascript
    let response = await fetch('status', {
      headers: {
        'X-File-Id': fileId
      }
    });

    // 服务器已有的字节数
    let startByte = +await response.text();
    ```

    这假设服务器通过 `X-File-Id`​ header 跟踪文件上传。应该在服务端实现。  
    如果服务器上尚不存在该文件，则服务器响应应为 `0`​。
3. 然后，我们可以使用 `Blob`​ 和 `slice`​ 方法来发送从 `startByte`​ 开始的文件：

    ```javascript
    xhr.open("POST", "upload", true);

    // 文件 id，以便服务器知道我们要恢复的是哪个文件
    xhr.setRequestHeader('X-File-Id', fileId);

    // 发送我们要从哪个字节开始恢复，因此服务器知道我们正在恢复
    xhr.setRequestHeader('X-Start-Byte', startByte);

    xhr.upload.onprogress = (e) => {
      console.log(`Uploaded ${startByte + e.loaded} of ${startByte + e.total}`);
    };

    // 文件可以是来自 input.files[0]，或者另一个源
    xhr.send(file.slice(startByte));
    ```

    这里我们将文件 id 作为 `X-File-Id`​ 发送给服务器，所以服务器知道我们正在上传哪个文件，并且，我们还将起始字节作为 `X-Start-Byte`​ 发送给服务器，所以服务器知道我们不是重新上传它，而是恢复其上传。  
    服务器应该检查其记录，如果有一个上传的该文件，并且当前已上传的文件大小恰好是 `X-Start-Byte`​，那么就将数据附加到该文件。

这是用 Node.js 写的包含客户端和服务端代码的示例。

在本网站上，它只有部分能工作，因为 Node.js 位于另一个服务 Nginx 后面，该服务器缓冲（buffer）上传的内容，当完全上传后才将其传递给 Node.js。

## [axios](https://www.axios-http.cn/docs/intro)

* 概念：`axios`​是目前最为流行的请求库，是基于`Promise`​封装网络请求库。可以用于浏览器和`node.js`​
* 基本使用：**多看看官方文档，写的很详细的**

  ```javascript
  axios本来就是基于promise封装好的http网络请求库
  一般是安装好axios插件后
  在src/utils/创建request.js文件
  在里面导入axios，并且创建axios实例对象
  并且配置好网络基地址，请求拦截器、响应拦截器等相关配置
  然后设置暴露导出,在需要使用网络请求的时候，导入request文件使用

  axios('url', {
    // 配置代码
    method: 'GET',//请求方法
    timeout: 3000,//超时时间
    withCredentials: true,//设置跨域请求是否携带凭借，即cookie
    headers: {//设置请求头
      'Content-Type': 'application/json'
    },
    // 其他请求配置...
  })
  .then((data) => {
    // todo: 真正业务逻辑代码
    console.log(data);
  }, (err) => {
    // 错误处理代码  
    if (err.response.status === 401) {
    // handle authorization error
    }
    if (err.response.status === 403) {
    // handle server forbidden error
    }
    // 其他错误处理.....
    console.log(err);
  });
  ```
* 创建实例并且设置基地址

  ```js
  import axios from 'axios'
  const request  = axios.create({
    baseURL='',//配置基地址
    timeout:3000//设置超时时间
  })
  export default request
  ```
* 拦截器

  ```js
  // 设置请求拦截器
  request.interceptors.request.use(
    confirm=>{
      return confirm 
    },
    err=>{   
      return Promise.reject(err)
    }
  )
  // 设置响应拦截器
  request.interceptors.response.use(
    confirm=>{
      return confirm 
    },
    err=>{
      return Promise.reject(err)
    }
  )
  ```
* 配置携带token：请求拦截器中配置

  ```js
  request.interceptors.request.use(
    config => {
      // 自动携带token
      config.headers.Authorization = token;//config是请求报文？
      return config;
  })
  ```
* 取消请求：`XMLHttpRequest.abort()`​用来取消已发送的请求，浏览器不再解析返回来的数据

  ```js
  //版本v0.22.0之后
  const controller = new AbortController();
  axios.get('url', data).then(function(response) {
     //...
  });
  // 取消请求
  controller.abort()
  ```
* 带参请求

  ```js
  request({
    url:'xxx/id=123&name=xixi' //和params是一样的
  })

  request({
    params:{//和请求一起发出的url参数，即会拼接在url上面，必须是一个简单对象  
      id:123
    }
  })

  //data传参数，是作为 请求体 被发送的数据
  //仅仅适用于put，post，delete和patch
  request({
    data:{
      firstName:'Fred'
    }
  })
  ```
