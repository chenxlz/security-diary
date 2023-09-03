![image](assets/wallpaper_5228834-20230207221228-s3qagsd.jpg)

# React

# [React](assets/React全家桶-20230902185416-dfw0shn.pdf)基础

### 关于JSX

　　**JSX** 是 JavaScript 语法扩展，可以让你在 JavaScript 文件中书

　　[JSX and React 是相互独立的](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html#whats-a-jsx-transform) 东西。但它们经常一起使用，但你 **可以** 单独使用它们中的任意一个，JSX 是一种语法扩展，而 React 则是一个 JavaScript 的库。写类似 HTML 的标签。

### JSX规则

1. 只能返回一个根元素  
    如果想要在一个组件中包含多个元素，**需要用一个父标签把它们包裹起来**。如`<div></div>`​ 或者说`​ <></>`​  
    JSX的底层是把内容都转化成对象，一个函数中只能返回一个对象，除非是返回的一个数组内容。
2. 标签必须闭合  
    即原来html中的可以不闭合标签在jsx中必须闭合，不然会出现报错信息
3. 使用驼峰命名 ~~所有~~ 大部分属性命名  
    jsx底层是转化成对象，所以要遵守js对 对象 命名时候的规则，如不能出现 中划线 - ， 不能使用 class 这种内置关键字等，所以使用 `class`​ 可以写成 `className`​ 这种方式
4. 使用花括号`{}`​在jsx中写 js 逻辑部分，如果需要在里面使用对象的话，那么就使用双花括号`{{}}`​，外面一次表示js区域，里面一成表示值为对象

### 纯函数和副作用

1. 纯函数：简单理解就是输入什么，就永远输出什么不会发生改变

    * **只负责自己的任务**。它不会更改在该函数调用前就已存在的对象或变量
    * **输入相同，则输出相同**。给定相同的输入，纯函数应总是返回相同的结果。
2. 副作用：会出现不符合预期的结果，会影响到不属于自己变量之外的值的改变，

    * 如：更新屏幕、启动动画、更改数据等，它们是 **“额外”** 发生的事情，与渲染过程无关。
    * 在 React 中，**副作用通常属于 ​**​****。
    * 事件处理程序是 React 在你执行某些操作（如单击按钮）时运行的函数
    * 即使事件处理程序是在你的组件 **内部** 定义的，它们也不会在渲染期间运行，**因此事件处理程序无需是纯函数**。
3. react使用纯函数编写的原因

    * 可以在不同的环境下运行，总是返回相同的结果
    * 可以为那些输入未更改的组件来 [跳过渲染](https://zh-hans.react.dev/reference/react/memo)，以提高性能。这是安全的做法，因为纯函数总是返回相同的结果，所以可以安全地缓存它们。
    * 如果在渲染深层组件树的过程中，某些数据发生了变化，React 可以重新开始渲染，而不会浪费时间完成过时的渲染。纯粹性使得它随时可以安全地停止计算。

### hook

　　在react中，所有以 `use`​ 开头的函数都被叫做 hook

　　hook是特殊函数，只在react渲染时有效，只能在组件或自定义hook的最顶层调用

### 数据快照

　　react中，初始化的state数据其实是一直保留的，更改数据不能直接更改state的值，需要在setState函数中传入需要改变的值，之后会保存一个数据快照，然后通知react进行渲染。

　　也就是说，要把需要的值作为参数传入到setState函数中，原来的state是不会改变的

　　**一个 state 变量的值永远不会在一次渲染的内部发生变化**

### 视图的更新

　　**React 会等到事件处理函数中的** 所有 **代码都运行完毕再处理你的 state 更新。**

　　重新渲染只会发生在所有 `setState()`​ 调用 **之后** 的原因。

### [更新state](https://zh-hans.react.dev/learn/queueing-a-series-of-state-updates)

　　使用`setState`​函数的时候，传入值是形成一个数据快照，原来的state其实是没有改变的

　　如果传入的是一个函数，如：`setState((n)=>n+1)`​ ，该回调函数称为 `更新函数`​  。`n`​是上一个state返回的值

1. React 会将此函数加入队列，以便在事件处理函数中的所有其他代码运行后进行处理。
2. 在下一次渲染期间，React 会遍历队列并给你更新之后的最终 state。

```js
一个更新函数（例如：n => n + 1）会被添加到队列中。
任何其他的值（例如：数字 5）会导致“替换为 5”被添加到队列中，已经在队列中的内容会被忽略。
```

### 函数组件和类组件

* 类组件：ES6的加入让JavaScript直接支持使用class来定义一个类，react创建组件的方式就是使用的类的继承， ES6 class 是目前官方推荐的使用方式，它使用了ES6标准语法来构建

  ```js
  import React from 'react'
  import ReactDOM from 'react-dom'

  class App extends React.Component {
  render () {
    return (<h1>欢迎进入React的世界</h1>)
    }
  }
  ReactDOM.render(
  <App/>,
  document.getElementById('root')
  )
  ```
* 函数式组件

  ```js
  import React from 'react'
  import ReactDOM from 'react-dom'

  const App = (props) => <h1>欢迎进入React的世界</h1>
  ReactDOM.render(
    // React组件的调用方式
    <App/>,
    document.getElementById('root')
  )
  ```

### 事件处理

```js
react中的事件绑定，使用 on + 事件名方式来绑定事件
react的事件绑定和原生的事件绑定是不一样的
原生事件都是小写  onclick 
react 是驼峰写法 onClick
react的事件不是原生事件，并不是绑定在dom上的，而是合成事件，有自己的事件处理机制，事件好像是在根节点统一处理，通过事件冒泡来的

event对象
和普通浏览器一样，事件handler会被自动传入一个 event 对象
这个对象和普通的浏览器 event 对象所包含的方法和属性都基本一致。
不同的是 React中的 event 对象并不是浏览器提供的，而是它自己内部所构建的。
它同样具有 event.stopPropagation、event.preventDefault 这种常用的方法
```

### ref

```js
1.定义需要的ref
const myRef = React.createRef()
2.挂载ref
<div ref={this.myRef}>hello</div>
3.获取ref
this.myRef.current
```

### state

* 类组件中直接定义

  ```js
  class App extends Component {
    state = {
      ...
    }
  }
  ```
* 使用类的 `constructor ​`​函数

  ```js
  class App extends Component {
    constructor(){
      super()//执行父组件中的constructor
      this.state = {
        ...
      }
    }
  }
  ```
* setState：用于改变组件状态的方法

  ```js
  第一个参数可以是对象，也可以是方法return一个对象，我们把这个参数叫做 updater
  handleChange(){
    this.setState({
      ...state中需要被替换的内容
    })
  }
  handleChange(){
    this.setState((prevState,props)=>{
      return {
        ...
      }
    })
  }

  该函数有第二个参数，参数的形式是回调函数，会在state改变之后，dom更新完成之后调用
  handleChange(){
    this.setState({
      ...state中需要被替换的内容
    },()=>{})
  }

  ps:
  setState 方法是 异步调用 的，react为了避免频繁的修改状态，会统一的把state变化排列到队伍中
  1.上面这个 异步 是react的优化手段， this.setState 在同步中，是异步的
  2.在异步中，this.setState 是同步的
  setTime(()=>{
    this.setState({})
    this.setState({})
  })
  ```

### react中的v-html

```js
react中使用 dangerouslySetInnerHTML 来实现v-html的功能

<div dangerouslySetInnerHTML={ { __html:this.state.htmlContent } }></div>
```
