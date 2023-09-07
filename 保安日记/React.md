![image](assets/wallpaper_5228834-20230207221228-s3qagsd.jpg)

# React

# [React](assets/React全家桶-20230902185416-dfw0shn.pdf)相关概念

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

### 视图更新

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

### react样式

　　关于`react`​中写样式的问题，目前来说没有比较统一的方案，react比较推荐的是直接在组件中写行内样式。因为一个组件就是一个整体，直接在组件内部写行内样式即可。

　　也可以单独的拿一个文件夹来存放 `css ​`​样式文件，在组件需要的地方， 使用 `import ​`​导入使用即可

### 函数组件和类组件

* 类组件：ES6的加入让`JavaScript`​直接支持使用`class`​来定义一个类，`react`​创建组件的方式就是使用的类的继承， `ES6 class ​`​是目前官方推荐的使用方式，它使用了ES6标准语法来构建

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

* 事件绑定

  ```js
  react中的事件绑定，使用 on + 事件名方式来绑定事件
  react的事件绑定和原生的事件绑定是不一样的
  原生事件都是小写  onclick 
  react 是驼峰写法 onClick
  react的事件不是原生事件，并不是绑定在dom上的，而是合成事件，有自己的事件处理机制，事件好像是在根节点统一处理，通过事件冒泡来的
  ```
* ​`event`​对象

  ```js
  event对象
  和普通浏览器一样，事件handler会被自动传入一个 event 对象
  这个对象和普通的浏览器 event 对象所包含的方法和属性都基本一致。
  不同的是 React中的 event 对象并不是浏览器提供的，而是它自己内部所构建的。
  它同样具有 event.stopPropagation、event.preventDefault 这种常用的方法
  ```

### ref

```js
推荐的获取ref的方法
1.定义需要的ref
const myRef = React.createRef()
2.挂载ref
<div ref={this.myRef}>hello</div>
3.获取ref
this.myRef.current

老方法
1.挂载ref
<div ref=myRef>hello</div>
2.获取ref
this.refs.myRef.current

ref使用箭头函数返回组件实例
设置变量接受
let myRef = null
再组件中获取
<div ref={ el => this.myRef = el }>hello</div>

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

# react基础

## 组件通信

### 状态提升

　　尽可能使用受控组件，组件不使用内部的 `状态`​， 使用 父组件传递过来的 `属性`​ 来控制组件的显示。

　　对于多个子组件之间的通讯，使用`​ 状态提升 ​`​，子组件内的数据展示都使用父组件的状态，并把改变父组件内状态的函数传递给子组件使用，从而达到子组件通讯，共享属性的效果

### 订阅发布

　　类似于vue中的eventBus。

　　核心思想是，创建个对象，里面一个数组，用于收集 组件订阅触发事件的函数， 一个`subscribe`​函数用于接受订阅函数，一个 `publish`​函数用于触发事件，把事件对于的函数执行。

　　达到一个集中手机事件，并提供触发的一个平台对象。

* 事件平台中心创建

  ```js
  //调度中心
  var bus = {
      list:[],
      //订阅
      subscribe(callback){
          this.list.push(callback)//组件订阅的触发函数进行收集
      },
      //发布，遍历所有的list， 将回调函数执行
      publish(text){//text，用于接收 publish 的描述字段
          this.list.forEach(callback=>{
              callback && callback(text)
          })
      }
  }
  ```
* 订阅者

  ```js
  订阅者把触发函数给push到list中，可以携带参数
  bus.subscribe((value)=>{
      console.log("2222",value)
  })
  ```
* 发布者

  ```js
  bus.publish('des')//传入事件的描述字段，用于触发事件
  ```
* 思考：这种是很简单的一种实现方法，可以大概理解类似原理的实现，目前来看，只要执行了publish 之后，所有的订阅事件函数都会触发一遍，并没有精确执行事件对于的事件函数

  ```js
  简单参考了网上的例子，写另外一个简单的实现
  var bus = {
    list:new Map([]),可以使用其他的方法，去实现事件名和对于的事件函数的对于关系
    //订阅
    subscribe(des,callback){
      if(this.list.has(dex))return
      this.list.set(des,callback)//映射关系
    },
    publish(des){
      this.list.has(des) && this.list.get(dex)()//找到对应的事件函数，并触发
    }
  }
  ```

　　‍

### context

* 概念：`context`​ 上下文对象，react可以使用 `context`​对子组件提供依赖。这个方法和vue中的provide、inject蛮像的
* 基本使用，需要`​ React.createContext() ​`​

  ```js
  import React, { Component } from 'react'
  const GlobalContext  = React.createContext() //创建context对象

  //父组件提供依赖
  export default class App extends Component {
      constructor() {
          super()
          this.state = {
              filmList: [],
              info: ""
          }
      }
      render() {
          return (//  需要使用 xxx.Provider 来提供依赖，需要在组件的最外层进行包裹。 value 来提供需要传递的参数
              <GlobalContext.Provider value={{
                  info: this.state.info,
                  changeInfo: (value) => {
                      this.setState({
                          info: value
                      })
                  }
              }}>
                  <div>
                     <FilmItem ></FilmItem>
                  </div>
              </GlobalContext.Provider>
          )
      }
  }
  //使用  xx.Consumer 包裹组件，来对父组件提供的数据进行消费。
  //同时要使用回调函数 （val）=> { return 组件 } 形式，通过value形参来拿到value
  class FilmItem extends Component {
      render() {
          let { name, poster, grade, synopsis } = this.props
          return (
              <GlobalContext.Consumer>
                  {
                      (value) => {
                          console.log(value)
                          return <div className="filmitem" onClick={() => {
                              value.changeInfo(synopsis)
                          }}>
                              <div>观众评分：{grade}</div>
                          </div>
                      }
                  }
              </GlobalContext.Consumer>
          )
      }
  }
  ```

　　‍

　　‍

　　‍

## 生命周期

　　​![image](assets/image-20230907231035-bfpvs4r.png)​![image](assets/image-20230907231045-bd0k3wk.png)​​

### [概述](https://juejin.cn/post/7248890696819277885?searchId=202309072235420B1497972C1DF3676587)

* react中自带的生命周期函数好像都是类组件中的，函数时组件需要使用生命周期的功能，需要使用hooks来实现。
* 生命周期一览

  ```js
  1.挂载
  constructor
  getDerivedStateFromProps
  render
  componentDidMount

  2.更新：setState、useState、props变化、forceUpdate 会触发组件的更新
  static  getDerivedStateFromProps （新） //类变量
  shouldComponentUpdate
  render
  getSnapshotBeforeUpdate （新）
  componentDidUpdate

  3.卸载
  componentWillUnmount
  ```
* 将被废弃的钩子：从 v16.3 开始废弃了 componentWillMount、componentWillReceiveProps、componentWillUpdate 三个钩子，并为这几个钩子提供了别名。以别名的方式就是来恶心你的，让你不要使用🤣。

  ```js
  UNSAFE_componentWillMount：当组件被渲染出来之前
  UNSAFE_componentWillReceiveProps：当组件收到新的props之前
  UNSAFE_componentWillUpdate：组件更新前
  ```
* 废弃原因

  ```js
  在v16之前的版本中，react的更新过程是同步的，react 在加载或者更新组件的过程是同步的，期间不允许其他的操作，往往一个主线程长时间被占用，会导致页面性能问题。
  而 React Fiber 的机制: 把更新过程碎片化，利用浏览器 requestIdleCallback 将可中断的任务进行分片处理，维护每一个分片的数据结构，就是Fiber，每一个小片的运行时间很短，这样唯一的线程就不会被独占。
  ```
* Fiber

  ```js
  1.概念
  在计算机中，除了 进程（Process）和线程（Thread）的概念外还有一个名为 Fiber (纤维)的概念，意指比Thread更细的线，也就是比线程控制得更精密的并发处理机制。
  Fiber 是一个执行单元也是一种数据结构，支撑 Fiber 构建任务的运转。
  在渲染过程中，react 将渲染任务拆分成了一个个的小任务进行处理，每一个小任务指的就是 Fiber 节点的构建。
  拆分的小任务会在浏览器的空闲时间被执行，每个任务单元执行完成后，React 都会检查是否还有空余时间，如果有就交换主线程的控制权。
  Fiber 的总体思路就是利用循环和链表代替递归去优化性能，Fiber 架构有两个阶段，

  render 阶段负责构架 Fiber 对象和链表，找出需要更新的 DOM
  commit 阶段负责去构建 DOM

  2.Fiber 对生命周期的影响
  在React Fiber中，一次更新过程会分成多个分片完成，所以完全有可能一个更新任务还没有完成，就被另一个更高优先级的更新过程打断，这时，优先级高的更新任务会优先处理完，而低优先级更新任务所做的工作则会完全作废，然后等待机会重头再来。
  React Fiber一个更新过程被分为两个阶段(Phase)：第一个阶段Reconciliation Phase和第二阶段Commit Phase。

  第一阶段：构建 Fiber 对象，构建链表，在链表中标记要执行的 DOM 操作 ，这个阶段是可以被打断的
  render： render 是纯函数能够被多次调用
  componentWillMount
  componentWillReceiveProps： 即使当前组件不更新，只要父组件更新也会引发这个函数被调用，允许被多次调用
  shouldComponentUpdate：这函数的作用就是返回一个true或者false，不会产生任何副作用，多调几次也没关系
  componentWillUpdate

  第二阶段：根据构建好的链表DOM 的，更新DOM的过程中不会被打断
  componentDidMount
  componentDidUpdate
  componentWillUnmount

  在React Fiber中，第一阶段中的生命周期函数在一次加载和更新过程中可能会被多次调用！ 
  React Fiber Reconciliation 这个过程有可能暂停然后继续执行，所以挂载和更新之前的生命周期钩子就有可能不执行或者多次执行。
  ```

### 挂载阶段

* constructor：只会在组件初始化的时候触发一次

  ```js
  1.设置初始化状态函数内部 state：因为组件的生命周期中任何函数都可能要访问 State，在生命周期中第一个被调用的构造函数便是初始化 state 最理想的地方。
  2.绑定成员函数上下文引用
  因为在 ES6 语法下，类的每个成员函数在执行时的 this 并不是和类实例自动绑定的；而在构造函数中 this 就是当前组件实例，所以在构造函数中将 this 为当前类实例；
  建议定义函数方法时直接使用箭头函数，就无须在构造函数中进行函数的 bind 操作。
  class Sample extends React.Component {
    constructor(props, context, updater) {
    //props：继承 React.Component 的属性方法，它是不可变的 read-only
    //context：全局上下文。
    //updater：包含一些更新方法的对象
    //this.setState 最终调用的是 this.updater.enqueueSetState
    //this.forceUpdate 最终调用的是 this.updater.enqueueForceUpdate 方法，所以这些 API 更多是 React 调用使用，暴露出来以备开发者不时之需。
      super(props);
      this.state = {
        foo: 'InitailValue',
      };
    }
  }

  Sample.defaultProps = {
    bar: 'InitialValue',
  };
  ```
* static getDerivedStateFromProps：该函数会在组件化实例化后和重新渲染前调用（生成 VirtualDOM 之后，实际 DOM 挂载之前）

  ```js
  1.无论是父组件的更新、props 的变化或通过 setState 更新组件内部的 State，它都会被调用。
  2.返回值：该生命周期函数必须有返回值，返回一个对象来更新 State，若返回 null 表明新 props 不更新 state。
  3.新特性：当组件实例化时，该方法替代了 componentWillMount，而当接收新的 props 时，该方法替代了 componentWillReceiveProps 和 componentWillUpdate。
  适用场景：表单获取默认值
  4.注意：getDerivedStateFromProps 是一个静态函数，不能使用this, 也就是只能作一些无副作用的操作，如果父组件导致了组件的重新渲染，即使属性没有更新，这一方法也会被触发；
  该生命周期函数被设计成静态方法的目的是为了保持该方法的纯粹。能够起到限制开发者无法访问 this 也就是实例的作用，这样就不能在里面调用实例方法或者 setState 以破坏该生命周期函数的功能。
  通过更具父组件输入的 props 按需更新 state，这种 state 叫做衍生 state，返回的对象就是要增量更新的 state，除此之外不应该在里面执行任何操作。

  static getDerivedStateFromProps(nextProps, prevState) {
    if (nextProps.translateX !== prevState.translateX) {
      return {
        translateX: nextProps.translateX
      }
    }
    if (nextProps.data !== prevState.data){
      return {
        data: nextProps.data
      }
    }
    return null;
  }
  ```
* UNSAFE_componentWillMount：预装载函数 componentWillMount 将在 React v17 正式废弃，用 UNSAFE_componentWillMount 别名代替。

  ```js
  1.触发时机：在构造函数和装载组件（将 DOM 树渲染到浏览器中）之间触发。装载组件后将执行 render 渲染函数。因此在此生命周期函数里使用 setState 同步设置组件内部状态 state 将不会触发重新渲染。
  2.注意：避免在该方法中引入任何的副作用（Effects）或订阅（Subscription）。对于这些使用场景，建议提前到构造函数中
  ```
* render：第一次以及之后组件发生更新，都会执行render，一个仅仅用于渲染的**纯函数**，返回值完全取决于 state 和 props，而生成虚拟 DOM 树进行对比是在 render 函数执行了之后进行

  ```js
  1.class组件中唯一必须实现的方法，用于渲染 DOM, render()方法必须有返回值，返回 null 或 false 表示不渲染任何 DOM 元素。
  2.一个仅仅用于渲染的**纯函数**。此渲染函数并不做实际的渲染动作（渲染到 DOM 树），它返回一个 JSX 的描述对象（及组件实例），何时进行真正的渲染是有 React 库决定。
  3.在组件 初始化 和 props或者state发生变化时候触发
  ```
* componentDidMount：在组件挂载完成之后 (插入DOM树后) 立即调用

  ```js
  1.触发时机：组件完全挂载到网页上后触发 
  使用场景：
  发送网络请求；
  任何依赖于 DOM 的初始化操作；
  添加事件监听；如果使用了 Redux 之类的数据管理工具，也能触发 action 处理数据变化逻辑。
  此钩子函数中允许使用 setState 改变组件内 State。
  注意：该生命周期函数在进行服务器端渲染时不会触发（仅客户端有效）。
  ```

### 更新阶段

　　属性（Props）或状态（State）的改变会触发一次组件的更新，但是组件是否会重新渲染，这取决于 shouldComponentUpdate

* shouldComponentUpdate：在组件更新之前调用，可以控制组件是否进行更新， 返回true时组件更新， 返回false则不更新 shouldComponentUpdate(nextProps, nextState)

  ```js
  1.当props或state发生变化，在重新渲染执行之前，第一个是即将更新的 props 值，第二个是即将跟新后的 state 值，可以根据更新前后的 props 或 state 来比较加一些限制条件，决定是否更新，进行性能优化
  2.当该方法返回的布尔值 false 告知 React 无须重新渲染时，render、UNSAFE_componentWillUpdate 和 componentDidUpdate 等生命周期钩子函数都不会被触发。
  3.此钩子函数在初始化渲染和使用了 forceUpdate 方法的情况下不会被触发，使用 forceUpdate 会强制更新
  4.请勿在此函数中使用 setState 方法，会导致循环调用。
  ```
* getSnapshotBeforeUpdate：保存状态快照。该生命周期函数会在组件即将挂载时触发，它的触发在 render 渲染函数之后。

  ```js
  由此可见，render 函数并没有完成挂载操作，而是进行构建抽象 UI（也就是 Virtual DOM）的工作。该生命周期函数执行完毕后就会立即触发 componentDidUpdate 生命周期钩子。
  该生命周期函数能让你捕获某些从 DOM 中才能获取的（可能会变更的例如滚动位置），此生命周期返回的任何值都会作为第三个参数传递给 componentDidUpdate()。如不需要传递任何值，那么请返回 null。
  getSnapshotBeforeUpdate(prevProps, prevState) {
      console.log('#enter getSnapshotBeforeUpdate');
      return 'foo';
  }
  componentDidUpdate(prevProps, prevState, snapshot) {
      console.log('#enter componentDidUpdate snapshot = ', snapshot);
  }

  ```
* componentDidUpdate：组件每次重新渲染后会触发 componentDidUpdate ； 但是只有在组件首次渲染（即初始化）时触发的是 componentDidMount 钩子函数，初始化渲染后componentDidMount 不会再被触发。

  ```js
  1.在 componentDidUpdate 生命周期函数中调用 setState 方法时，确实需要加上条件判断，以避免死循环的发生。
  2.通过判断 prevProps、prevState 和 this.state 之间的数据变化，来判断是否执行相关的 state 变更逻辑，这使得尽管在 componentDidUpdate 中调用了 setState 进行再更新，如果条件不成立state就不会进行更新，从而避免了死循环的发生。
  相比装载阶段的生命周期函数，更新阶段的生命周期函数使用的相对来说要少一些。
  常用的是getDerivedStateFromProps、shouldComponentUpdate， 前者经常用于根据新 props 的数据去设置组件的 State，而后者则是常用于优化，避免不必要的渲染。
  componentDidUpdate(prevProps){
      if (this.props.id !== prevProps.id){
          this.fetchData(this.props.id);
      }
  }

  ```

### 卸载阶段

* componentWillUnmount：在组件卸载和销毁之前触发。可以利用这个生命周期方法进行事件的解绑。

  ```js
  用于移除事件监听器；取消网络请求；取消定时器；解绑 DOM 事件。 在该方法中调用 setState 不会触发 render，因为所有的更新队列，更新状态都被重置为 null。
  ```

### 组件间的周期顺序

　　当子组件自身状态改变时，不会对父组件产生副作用的情况下，父组件不会进行更新，即不会触发父组件的生命周期

　　当父组件中状态发生变化（包括子组件的挂载以及卸载）时，会触发自身对应的生命周期以及子组件的更新

* 父子组件初始化

  ```js
  Parent 组件： constructor()
  Parent 组件： getDerivedStateFromProps()
  Parent 组件： render()
  Child 组件： constructor()
  Child 组件： getDerivedStateFromProps()
  Child 组件： render()
  Child 组件： componentDidMount()
  Parent 组件： componentDidMount()

  当执行render子组件的时候，才会进入子组件的生命周期，子组件的周期结束后，再回到上级的周期。
  ```
* 改变子组件自身状态

  ```js
  Child 组件： getDerivedStateFromProps()
  Child 组件： shouldComponentUpdate()
  Child 组件： render()
  Child 组件： getSnapshotBeforeUpdate()
  Child 组件： componentDidUpdate()

  ```
* 修改父组件传入给子组件的 props 当父组件修改传递的 props 值时，会触发父子组件的更新

  ```js
  Parent 组件： getDerivedStateFromProps()
  Parent 组件： shouldComponentUpdate()
  Parent 组件： render()
  Child 组件： getDerivedStateFromProps()
  Child 组件： shouldComponentUpdate()
  Child 组件： render()
  Child 组件： getSnapshotBeforeUpdate()
  Parent 组件： getSnapshotBeforeUpdate()
  Child 组件： componentDidUpdate()
  Parent 组件： componentDidUpdate()
  ```
* 只修改父组件的值 当父组件的值被修改时，会触发父组件的 render() 方法重新渲染，并递归地 r-render 子组件。这个过程中，会依次触发所有子组件的生命周期方法：

  ```js
  Parent 组件： getDerivedStateFromProps()
  Parent 组件： shouldComponentUpdate()
  Parent 组件： render()
  Child 组件： getDerivedStateFromProps()
  Child 组件： shouldComponentUpdate()
  Child 组件： render()
  Child 组件： getSnapshotBeforeUpdate()
  Parent 组件： getSnapshotBeforeUpdate()
  Child 组件： componentDidUpdate()
  Parent 组件： componentDidUpdate()

  ```
* 子组件修改自身state

  ```js
  子组件 getDerivedStateFromProps
  子组件 shouldComponentUpdate
  子组件 render
  子组件 getSnapShotBeforeUpdate
  子组件 componentDidUpdate
  ```
* 卸载子组件

  ```js
  Parent 组件： getDerivedStateFromProps()
  Parent 组件： shouldComponentUpdate()
  Parent 组件： render()
  Parent 组件： getSnapshotBeforeUpdate()
  Child 组件： componentWillUnmount()
  Parent 组件： componentDidUpdate()
  ```

　　‍

　　‍

　　‍

　　‍

## 插槽

### children

　　父组件可以直接向子组件中插入内容，此时在子组件中可以在**this.props.children**获取到插入到其中传递的元素。

1. 如果插入了多个元素，那么this.props.children是一个数组，此时可通过索引访问到对应位置的元素

2. 如果插入了单个元素，那么this.props.children即该元素（**就不是一个数组了**）

* 子组件

  ```js
  子组件通过this.props.children 获取父组件传递过来的标签
  import React, { Component } from 'react'
  import PropTypes from "prop-types"

  export class NavBar extends Component {
    render() {
      const { children } = this.props
      return (
        <div>
          <div>{ children[0] }</div>
          <div>{ children[1] }</div>
          <div>{ children[2] }</div>
        </div>
      )
    }
  }

  NavBar.propTypes = {
    children: PropTypes.array//表示children是数组形式，需要父组件传递多个内容进来
    children: PropTypes.element//表示children是一个元素，父组件只能传递一个内容进来，这样的话，this.props.children 就是指该原生本身了
  }

  export default NavBar
  ```
* 父组件

  ```js
  import React, { Component } from 'react'
  import NavBar from './nav-bar'

  export class App extends Component {
    render() {
      return (
        <div>
          <NavBar>
            <button>按钮</button>
            <h2>哈哈哈</h2>
            <i>斜体文本</i>
          </NavBar>
        </div>
      )
    }
  }

  export default App

  ```

### props

　　使用children实现插槽或许是比较方便，但是使用索引获取传入进来的元素容易出错，需要明确顺序，涉及到索引变动就比较麻烦

　　**在父组件中不仅可以使用props来向子组件传递数据、回调函数外，还可以传递元素**

* 父组件

  ```js
  import React, { Component } from 'react'
  import NavBar from './nav-bar'

  export class App extends Component {
    render() {
      const btn = <button>按钮</button>
      return (
        <div>
          <NavBar
            leftSlot={btn}
            centerSlot={<h2>哈哈哈</h2>}
            rightSlot={<i>123</i>}
          />
        </div>
      )
    }
  }
  export default App
  ```
* 子组件

  ```js
  import React, { Component } from 'react'
  export class NavBarTwo extends Component {
    render() {
      const { leftSlot, centerSlot, rightSlot } = this.props

      return (
        <div className='nav-bar'>
          <div className="left">{leftSlot}</div>
          <div className="center">{centerSlot}</div>
          <div className="right">{rightSlot}</div>
        </div>
      )
    }
  }
  export default NavBarTwo
  ```

## hooks
