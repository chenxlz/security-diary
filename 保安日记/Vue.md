![image](assets/HACG_22-20230204152545-63onq54.jpg)

# 基本概念

### 响应式原理

响应式的基本原理：响应式数据和函数之间的关联关系

函数：`render`​、`computed`​、`watch`​、`watchEffect`​

数据：[响应式数据](https://cn.vuejs.org/guide/extras/reactivity-in-depth.html#how-reactivity-works-in-vue)

只有这样，数据和函数产生关系之后，才会在数据发生变化的时候，去通知视图层去更新

本质是通过数据劫持和发布订阅者模式实现的双向数据绑定

### DOM 更新

当你修改了响应式状态时，`DOM`​会被自动更新。但是需要注意的是，**DOM 更新不是同步的**。

Vue 会在`“next tick”`​更新周期中缓冲所有状态的修改，以确保不管进行了多少次状态修改，每个组件都只会被更新一次。

### VNode

* 概念：`VNode`​是一个类，通过属性来描述`dom`​对象

  ```js
  原生js中的各种节点概念
  元素节点:div
  属性节点:class属性
  文本节点:标签内的文字
  注释节点:html中的注释
  ```
* 基本结构

  ```js
  export default class VNode {
    tag: string | void; //标签
    data: VNodeData | void;//数据
    children: ?Array<VNode>;//子组件
    text: string | void;//是否是文本节点
    elm: Node | void;//对应的实际dom节点，引用地址
    ns: string | void;
    context: Component | void; // rendered in this component's scope 
    functionalContext: Component | void; // only for functional component root nodes
    key: string | number | void;
    componentOptions: VNodeComponentOptions | void;
    componentInstance: Component | void; // component instance
    parent: VNode | void; // component placeholder node
    raw: boolean; // contains raw HTML? (server only)
    isStatic: boolean; // hoisted static node
    isRootInsert: boolean; // necessary for enter transition check
    isComment: boolean; // empty comment placeholder?
    isCloned: boolean; // is a cloned node?
    isOnce: boolean; // is a v-once node?

    constructor (
      tag?: string,
      data?: VNodeData,
      children?: ?Array<VNode>,
      text?: string,
      elm?: Node,
      context?: Component,
      componentOptions?: VNodeComponentOptions
    ) {
      /*当前节点的标签名*/
      this.tag = tag
      /*当前节点对应的对象，包含了具体的一些数据信息，是一个VNodeData类型，可以参考VNodeData类型中的数据信息*/
      this.data = data
      /*当前节点的子节点，是一个数组*/
      this.children = children
      /*当前节点的文本*/
      this.text = text
      /*当前虚拟节点对应的真实dom节点*/
      this.elm = elm
      /*当前节点的名字空间*/
      this.ns = undefined
      /*编译作用域*/
      this.context = context
      /*函数化组件作用域*/
      this.functionalContext = undefined
      /*节点的key属性，被当作节点的标志，用以优化*/
      this.key = data && data.key
      /*组件的option选项*/
      this.componentOptions = componentOptions
      /*当前节点对应的组件的实例*/
      this.componentInstance = undefined
      /*当前节点的父节点*/
      this.parent = undefined
      /*简而言之就是是否为原生HTML或只是普通文本，innerHTML的时候为true，textContent的时候为false*/
      this.raw = false
      /*静态节点标志*/
      this.isStatic = false
      /*是否作为跟节点插入*/
      this.isRootInsert = true
      /*是否为注释节点*/
      this.isComment = false
      /*是否为克隆节点*/
      this.isCloned = false
      /*是否有v-once指令*/
      this.isOnce = false
    }

    // DEPRECATED: alias for componentInstance for backwards compat.
    /* istanbul ignore next https://github.com/answershuto/learnVue*/
    get child (): Component | void {
      return this.componentInstance
    }
  }
  ```
* 重要的属性

  ```js
  TextVNode 文本节点。
  ElementVNode 普通元素节点。
  ComponentVNode 组件节点。
  EmptyVNode 没有内容的注释节点。
  CloneVNode 克隆节点，可以是以上任意类型的节点，唯一的区别在于isCloned属性为true 
  ```

### CSS scoped

* 作用：样式作用空间，不造成样式污染

  ```javascript
  <style scoped>
  .example {
    color: red;
  }
  </style>
  <template>
    <div class="example">hi</div>
  </template>

  转化为
  <style>
  .example[data-v-f3f3eg9] {
    color: red;
  }
  </style>

  <template>
    <div class="example" data-v-f3f3eg9>hi</div>
  </template>
  ```
* 作用原理

  ```css
  1.
  如果某个组件的样式加了scoped，那么它会帮这个组件内所有的标签都加一个行内属性：data-v-hash
  并且把这个组件里加了 scoped 的 style 里的样式，变成 属性选择器 [data-v-hash]，
  通过这个操作，就意味着让这些样式只能给自己用了，因为只有自己才有 [data-v-自己的hash] 的行内属性

  2.
  也就是说，在设置了 scoped 中写的样式，这个样式只会作用到该标签上面，如果这个标签里面也有标签，那里面的标签是无法接受到该样式的 scoped

  3.
  深度作用选择器，就是把这个选择器的 `data-v-hash` 的属性选择器提升到前面，变成后代选择器，既然是后代选择器，所有当前组件里的后代就能匹配的上了
  p[data-v-hash]{} //交集选择器
  变成了
  [data-v-hash] p{} //后代选择器
  ```
* vue2写法：

  ```css
  >>>       less不支持 
  /deep/    less 支持，但是vscode报错
  ::v-deep  推荐，不会报错，less 和 scss 都支持

  <style scoped>
  ::v-deep p {
    color: red;//现在p标签作用的更深：能影响到子组件里的标签
  }
  </style>
  ```
* vue3写法：

  ```css
  1. 深度作用选择器
  <style scoped>
    .a :deep(div) {}
  </style>

  2. 插槽选择器
  默认情况下，作用域样式不会影响到 <slot/> 渲染出来的内容，因为它们被认为是父组件所持有并传递进来的。使用 :slotted 伪类以明确地将插槽内容作为选择器的目标
  <style scoped>
  :slotted(div) {}
  </style>

  3.全局选择器
  <style scoped>
  :global(.red) {
    color: red;
  }
  </style>
  ```

### style-module

* 概念：一个 `<style module>`​ 标签会被编译为 [CSS Modules](https://github.com/css-modules/css-modules) 并且将生成的 `CSS class`​ 作为 `$style`​ 对象暴露给组件
* ​`<style module></style> === $style`​

  ```css
  <template>
    <p :class="$style.red">This should be red</p>
  </template>

  <style module>
  .red {
    color: red;
  }
  </style>
  ```
* ​`<style module="moduleName"></style> === $moduleName`​

  ```css
  <template>
    <p :class="$classesName.red">red</p>
  </template>

  <style module="classesName">
  .red {
    color: red;
  }
  </style>
  ```
* ​`vue3:useCssModule(name?):$style|$classes`​

  ```css
  import { useCssModule } from 'vue'
  useCssModule()// 在 setup() 作用域中,默认情况下, 返回 <style module> 的 class
  useCssModule('classes')// 具名情况下, 返回 <style module="classes"> 的 class
  ```

### hook

* 概念：和`react`​中的`hook`​一样。`hook`​是`vue3`​中有的，在功能上代替了`vue2`​中的((20220926111147-8ccang1 'mixins'))。
* 官方解释：“组合式函数”(`Composables`​) 是一个利用 `Vue`​的组合式 API 来封装和复用 **有状态逻辑** 的函数
* 混入的缺点

  ```js
  1.不清晰的数据来源：
  使用多个mixin时候，数据的来源就会不清晰。混入是导入注册就可以直接当属性或方法使用，一旦mixins多了，就不容易搞清楚那个属性是那个文件里面的
  2.命名空间会产生冲突：
  因为混入的时候不可以改名，一旦混入进来的文件多了，里面中的属性或方法名重复，那么就会发生命名冲突
  3.隐式的跨 mixin 交流：
  多个 mixin 需要依赖共享的属性名来进行相互作用，这使得它们隐性地耦合在一起
  ```
* 基本使用

  ```js
  1.创建相应的js文件
  2.里面写方法，也就是.vue文件中，js部分的内容，可以正常地导入vue中的生命周期等内容
  3.再暴露出去使用
  4.每个调用函数的组件实例都会创建独有的状态属性（即vue2中的mixin的使用，组件的使用不会影响到别的组件使用相同的mixin）
  5.组合式函数约定用驼峰命名法命名，并以“use”作为开头。
  ```

### 渲染函数

​`h()`​ 是 **hyperscript** 的简称——意思是“能生成 HTML (超文本标记语言) 的 JavaScript”。

这个名字来源于许多虚拟 DOM 实现默认形成的约定。

一个更准确的名称应该是 `createVnode()`​，但当你需要多次使用渲染函数时，一个简短的名字会更省力。

参数：

第一个参数既可以是一个字符串 (用于原生元素) 也可以是一个 Vue 组件定义。

第二个参数是要传递的 prop

第三个参数是子节点信息

* 函数签名

  ```js
  function h(
    type: string | Component,
    props?: object | null,
    children?: Children | Slot | Slots
  ): VNode

  // 省略 props
  function h(type: string | Component, children?: Children | Slot): VNode

  type Children = string | number | boolean | VNode | null | Children[]

  type Slot = () => Children

  type Slots = { [name: string]: Slot }
  ```
* 创建元素

  ```js
  import { h } from 'vue'
  // 除了 type 外，其他参数都是可选的
  h('div')
  h('div', { id: 'foo' })
  // attribute 和 property 都可以用于 prop
  // Vue 会自动选择正确的方式来分配它
  h('div', { class: 'bar', innerHTML: 'hello' })
  // class 与 style 可以像在模板中一样
  // 用数组或对象的形式书写
  h('div', { class: [foo, { bar }], style: { color: 'red' } })
  // 事件监听器应以 onXxx 的形式书写
  h('div', { onClick: () => {} })
  // children 可以是一个字符串
  h('div', { id: 'foo' }, 'hello')
  // 没有 prop 时可以省略不写
  h('div', 'hello')
  h('div', [h('span', 'hello')])
  // children 数组可以同时包含 vnode 和字符串
  h('div', ['hello', h('span', 'hello')])

  //使用组件
  import Foo from './Foo.vue'
  // 传递 prop
  h(Foo, {
    // 等价于 some-prop="hello"
    someProp: 'hello',
    // 等价于 @update="() => {}"
    onUpdate: () => {}
  })

  // 传递单个默认插槽
  h(Foo, null, () => 'default slot')

  // 传递具名插槽
  // 注意，需要使用 `null` 来避免
  // 插槽对象被当作是 prop
  h(MyComponent, null, {
    default: () => 'default slot',
    foo: () => h('div', 'foo'),
    bar: () => [h('span', 'one'), h('span', 'two')]
  })
  ```

### 类型标注

* props

  ```js
  1.
  const props = defineProps({
    foo: { type: String, required: true },
    bar: Number
  })
  2.
  const props = defineProps<{
    foo: string
    bar?: number
  }>()
  3.
  interface Props {
    foo: string
    bar?: number
  }
  const props = defineProps<Props>()


  Props 解构默认值：当使用基于类型的声明时，我们失去了为 props 声明默认值的能力，这可以通过 withDefaults 编译器宏解决。
  export interface Props {
    msg?: string
    labels?: string[]
  }

  const props = withDefaults(defineProps<Props>(), {
    msg: 'hello',
    labels: () => ['one', 'two']
  })

  ```
* emits

  ```js
  <script setup lang="ts">
  // 运行时
  const emit = defineEmits(['change', 'update'])

  // 基于类型
  const emit = defineEmits<{
    (e: 'change', id: number): void
    (e: 'update', value: string): void
  }>()

  // 3.3+: alternative, more succinct syntax
  const emit = defineEmits<{
    change: [id: number]
    update: [value: string]
  }>()
  </script>
  ```
* ref：会隐式地从它的参数中推导类型

  ```js
  const year = ref(2020)//自动推导
  const year: Ref<string | number> = ref('2020')
  const n = ref<number>()// 推导得到的类型：Ref<number | undefined>
  ```
* reactive：会隐式地从它的参数中推导类型

  ```js
  import { reactive } from 'vue'
  const book = reactive({ title: 'Vue 3 指引' })// 推导得到的类型：{ title: string }

  interface Book {
    title: string
    year?: number
  }
  const book: Book = reactive({ title: 'Vue 3 指引' })
  ```
* computed：会自动从其计算函数的返回值上推导出类型

  ```js
  import { ref, computed } from 'vue'
  const count = ref(0)
  const double = computed(() => count.value * 2)// 推导得到的类型：ComputedRef<number>

  const result = double.value.split('')// => TS Error: Property 'split' does not exist on type 'number'
  ```
* provide、inject：这两通常是用在不同组件之间（祖孙），类型标注需要使用`InjectionKey`​接口，它是一个继承自 `Symbol`​ 的泛型类型

  ```js
  import { provide, inject } from 'vue'
  import type { InjectionKey } from 'vue'
  标注是给所提供的值进行标注的
  const key = Symbol() as InjectionKey<string>
  provide(key, 'foo') // 若提供的是非字符串值会导致错误
  const foo = inject(key) // foo 的类型：string | undefined


  const foo = inject<string>('foo') // 类型：string | undefined
  const foo = inject<string>('foo', 'bar') // 类型：string //提供了一个默认值

  ```
* 模板引用的类型标注：给组件设置类型：需要通过一个显式指定的泛型参数和一个初始值 `null`​ 来创建。`InstanceType<typeof>`​

  ```js
  //原生的组件
  <template>
    <input ref="el" />
  </template>
  <script setup lang="ts">
  import { ref, onMounted } from 'vue'
  const el = ref<HTMLInputElement | null>(null)
  onMounted(() => {
    el.value?.focus()
  })
  </script>


  //获取子组件
  <script setup lang="ts">
  import MyModal from './MyModal.vue'
  const modal = ref<InstanceType<typeof MyModal> | null>(null)

  const openModal = () => {
    modal.value?.open()
  }
  </script>
  ```

# API

### setup

```js
export default {
  //两个参数，props，context，第二个是上下文对象，里面有 emit、attrs、slot、expose 等
  //返回返回值会暴露给模板和其他的选项式 API 钩子
  //exporse 暴露出的东西才能给模板引用中使用
  setup(props, context) {
    // 透传 Attributes（非响应式的对象，等价于 $attrs）
    console.log(context.attrs)
    // 插槽（非响应式的对象，等价于 $slots）
    console.log(context.slots)
    // 触发事件（函数，等价于 $emit）
    console.log(context.emit)
    // 暴露公共属性（函数）
    console.log(context.expose)
  }
}
```

### reactive

​`reactive()`​ 将深层地转换对象，当访问嵌套对象时，它们也会被 `reactive()`​ 包装。当 `ref`​ 的值是一个对象时，`ref()`​ 也会在内部调用它。

​`shallowReactive() `​可以选择退出深层响应性，就是只包装最外面这层。

* 概念：`reactive`​是使用`Proxy`​的，返回的是代理对象，里面的对象也是，所以和原对象是不一致的，不是全等关系，**只有代理对象是响应式的，更改原始对象不会触发更新。**

  ```js
  const raw = {}
  const proxy = reactive(raw)
  console.log(proxy === raw) // false// 代理对象和原始对象不是全等的

  为保证访问代理的一致性，对同一个原始对象调用 reactive() 会总是返回同样的代理对象
  而对一个已存在的代理对象调用 reactive() 会返回其本身
  意思就是说，不是代理我给你一个代理，这个代理对象是不会变得，对代理对象进行代理，会把本身这个代理对象返回
  console.log(reactive(raw) === proxy) // true// 在同一个对象上调用 reactive() 会返回相同的代理
  console.log(reactive(proxy) === proxy) // true// 在一个代理上调用 reactive() 会返回它自己

  const proxy = reactive({})
  const raw = {}
  proxy.nested = raw
  console.log(proxy.nested === raw) // false
  ```
* 局限性：`proxy`​只能代理对象的基本操作，简单数据类型需要通过包装成对象才能创建响应式

  ```js
  1.它只能用于对象类型 (对象、数组和如 Map、Set 这样的集合类型)。它不能持有如 string、number 或 boolean 这样的原始类型。
  2.不能轻易地“替换”响应式对象，因为这样的话与第一个引用的响应性连接将丢失
  3.对解构操作不友好：当我们将响应式对象的原始类型属性解构为本地变量时，或者将该属性传递给函数时，我们将丢失响应性连接
  5.所以建议使用 ref() 作为声明响应式状态的主要 API
  ```
* 基本使用

  ```js
  定义
  import { reactive } from 'vue'
  const state = reactive({ count: 0 })

  js中使用
  state.count++ //可以不用.value

  模板中使用
  <button @click="state.count++">
    {{ state.count }}
  </button>
  ```

### shallReactive

​`reactive()`​ 的浅层作用形式。**一个浅层响应式对象里只有根级别的属性是响应式的。**

```js
const state = shallowReactive({
  foo: 1,
  nested: {
    bar: 2
  }
})
// 更改状态自身的属性是响应式的
state.foo++
// ...但下层嵌套对象不会被转为响应式
isReactive(state.nested) // false
// 不是响应式的
state.nested.bar++
```

### ref

​`ref()`​ 接收参数，创建一个对象，对象的value属性指向用户传递的数据，该对象使用`reactive`​进行创建响应式数据

**可以是简单或复杂数据类型**，返回一个响应式的、可更改的`ref对象`​，此对象只有一个指向其内部值的属性 `.value`​。

​`ref`​可以持有任何类型的值，包括深层嵌套的对象、数组或者 JavaScript 内置的数据结构，比如 `Map`​、`Set`​

也通过 `shallowRef()`​这个api来**传建浅层的响应式对象**，只会对`.value `​的访问是响应式的，在某种情况下可以减少性能的浪费

* 基本使用

  ```js
  import { ref }  from 'vue'
  let count =  ref(0)
  let obj =  ref({count: 0})
  const year: Ref<string | number> = ref('2020')//传入一个泛型参数，来覆盖默认的推导行为

  function fn(){
    count++
  }

  <template>
    <div>{{ obj.count }}</div>//在模板中会自动解包，不在需要.value
  </template>
  ```
* 解包：简单来说就是在模板中使用`ref`​变量的时候，不需要 `.value`​ 解包拿到最新的值

  ```js
  在模板中ref会自动解包（最顶级），作为响应式数据的对象属性会自动解包，作为 Array、Map 的值的时候，需要手动解包
  1.作为响应式对象的属性的时候会自动解包
  const count = ref(0)
  const state = reactive({
    count
  })
  console.log(state.count) // 0  赋值没有用 .value

  state.count = 1
  console.log(count.value) // 1

  2.作为响应式数组或原生集合类型(如 Map) 中的元素被访问时，它不会被解包
  const books = reactive([ref('Vue 3 Guide')])
  console.log(books[0].value)// 这里需要 .value

  const map = reactive(new Map([['count', ref(0)]]))
  console.log(map.get('count').value)// 这里需要 .value
  ```
* 在模板中使用，最顶级属性才会自动解包

  ```js
  const count = ref(0)//顶级
  const object = { id: ref(0) }//非顶级，在普通对象中作为一个value

  模板中使用
  {{ count + 1 }}会触发响应式
  {{ object.id + 1 }}不会触发响应式，渲染结果是： [object Object]1

  如果 ref 是文本插值的最终计算值 (即 {{ }} 标签)，那么它将被解包，因此以下内容将渲染为 1：
  {{ object.id }} //该特性仅仅是文本插值的一个便利特性，等价于 {{ object.id.value }}

  可以解构出来获得响应式
  const { id } = object
  {{ id + 1 }}
  ```

### shallowRef

​`ref()`​的浅层作用形式。创建并返回 `浅层响应式`​ 数据。

和 `ref()`​ 不同，浅层 ref 的内部值将会原样存储和暴露，并且不会被深层递归地转为响应式。只有对 `.value`​ 的访问是响应式的。

```js
const state = shallowRef({ count: 1 })
state.value.count = 2// 不会触发更改
state.value = { count: 2 }// 会触发更改
```

### triggerRef

强制触发依赖于一个`浅层 ref `​的副作用，这通常在对浅引用的内部值进行深度变更后使用。

```js
const shallow = shallowRef({
  greet: 'Hello, world'
})
// 触发该副作用第一次应该会打印 "Hello, world"
watchEffect(() => {
  console.log(shallow.value.greet)
})
// 这次变更不应触发副作用，因为这个 ref 是浅层的
shallow.value.greet = 'Hello, universe'
// 打印 "Hello, universe"
triggerRef(shallow)
```

### customRef

创建一个自定义的 `ref`​，显式声明对其依赖追踪和更新触发的控制方式。

```js
import { customRef } from 'vue'
//track 是收集依赖信息
//trigger 触发和该响应式有关的effect函数
export function useDebouncedRef(value, delay = 200) {
  let timeout
  return customRef((track, trigger) => {
    return {
      get() {
        track()
        return value
      },
      set(newValue) {
        clearTimeout(timeout)
        timeout = setTimeout(() => {
          value = newValue
          trigger()
        }, delay)
      }
    }
  })
}
```

### readonly

* 接受一个对象 (不论是响应式还是普通的) 或是一个 `ref`​，返回一个原值的只读代理。是深层只读。

  ```js
  const original = reactive({ count: 0 })
  const copy = readonly(original)
  watchEffect(() => {
    // 用来做响应性追踪
    console.log(copy.count)
  })
  original.count++ // 更改源属性会触发其依赖的侦听器
  copy.count++ // warning! // 更改该只读副本将会失败，并会得到一个警告
  ```

### toRef

可以将 值、`refs`​或 `getters`​规范化为 `refs`​(3.3+)。

也可以基于响应式对象上的一个属性，创建一个对应的 ref，这样创建的 ref 与其源属性保持同步（改变源属性的值将更新 ref 的值，反之亦然。）

```js
toRef(existingRef)// 按原样返回现有的 ref
toRef(1) // 从非函数的值中创建普通的 ref，等同于 ref(1)
toRef(reactive,key) // 返回响应式数据中的key对应的value值，同时两者保持响应式的相互联系
toRef(() => props.foo)// 创建一个只读的 ref，当访问 .value 时会调用此 getter 函数


const state = reactive({
  foo: 1,
  bar: 2
})

const fooRef = toRef(state, 'foo')// 双向 ref，会与源属性同步
// 更改该 ref 会更新源属性
fooRef.value++
console.log(state.foo) // 2
// 更改源属性也会更新该 ref
state.foo++
console.log(fooRef.value) // 3
```

### toRefs

**将一个响应式对象转换为一个普通对象，这个普通对象的每个属性都是指向源对象相应属性的 ref。**

每个单独的 ref 都是使用 `toRef()`​ 创建的。

主要用于实现`reactive`​创建的响应式数据解构

```js
const state = reactive({
  foo: 1,
  bar: 2
})

const stateAsRefs = toRefs(state)

// 这个 ref 和源属性已经“链接上了”
state.foo++
console.log(stateAsRefs.foo.value) // 2

stateAsRefs.foo.value++
console.log(state.foo) // 3
```

‍

### computed

​`computed()`​ 方法期望接收一个 `getter`​ 函数，返回值为一个**计算属性 ref。** 或者传入对象，对象包含set、get方法

**默认是在组件更新完成之前调用。**

计算属性值会基于**其响应式依赖**被**缓存**：无论多少次**访问计算属性**，都会**立即返回先前的计算结果**，**而不用重复执行 getter 函数**。

* 基本使用

  ```js
  <script setup>
  import { reactive, computed } from 'vue'

  const author = reactive([
      'Vue 2 - Advanced Guide',
      'Vue 3 - Basic Guide',
      'Vue 4 - The Mystery'
  ])

  // 一个计算属性 ref
  const publishedBooksMessage = computed(() => {
    return author.length > 0 ? 'Yes' : 'No'
  })
  </script>

  <template>
    <span>{{ publishedBooksMessage }}</span>
  </template>
  ```
* 计算属性的`setter`​

  ```js
  <script setup>
  import { ref, computed } from 'vue'
  const firstName = ref('John')
  const lastName = ref('Doe')
  const fullName = computed({
    // getter
    get() {
      return firstName.value + ' ' + lastName.value
    },
    // setter
    set(newValue) {
      [firstName.value, lastName.value] = newValue.split(' ')
    }
  })
  </script>
  ```
* 注意

  ```js
  1. Getter 不应有副作用
  计算属性的 getter 应只做计算而没有任何其他的副作用

  2.避免直接修改计算属性值
  从计算属性返回的值是派生状态。
  可以把它看作是一个“临时快照”，每当源状态发生变化时，就会创建一个新的快照。
  更改快照是没有意义的，因此计算属性的返回值应该被视为只读的，并且永远不应该被更改——应该更新它所依赖的源状态以触发新的计算。
  ```

### watch

* 概念：侦听器，在响应式侦听发生的时候，触发回调函数，可以去执行一些函数的副作用，返回一个函数，执行该函数则取消数据侦听。

  ```js
  默认是懒侦听的，即仅在侦听源发生变化时才执行回调函数。
  <script setup>
  import { ref, watch } from 'vue'
  const conut = ref(0)
  watch(conut , ()=>{},{deep：true，immediate:true})// 可以直接侦听一个 ref
  </script>
  ```
* ​`watch`​的三个参数

  ```js
  1.第一个参数：
  可以是不同形式的“数据源” 
  ref (包括计算属性)、一个响应式对象、一个getter函数、或多个数据源组成的数组
  不能直接侦听响应式对象的属性，需要侦听的话，使用一个getter函数

  2.第二个参数：
  执行的回调函数

  3.第三个参数：
  配置对象
  {
    deep?：boolean ,//default: false//是否深度侦听
    immediate?:boolean ,//default: false//是否初始化时立即执行一次
    flush?:'pre' | 'post' | 'sync' //default: pre。 post 组件更新后调用，获取最新组件实例，sync 组件更新前触发
    once：boolean,//是否只调用一次
  }，

  const x = ref(0)
  const y = ref(0)
  // 单个 ref
  watch(x, (newX) => {
    console.log(`x is ${newX}`)
  })

  // getter 函数
  watch(
    () => x.value + y.value,
    (sum) => {
      console.log(`sum of x + y is: ${sum}`)
    }
  )

  // 多个来源组成的数组
  watch([x, () => y.value], ([newX, newY]) => {
    console.log(`x is ${newX} and y is ${newY}`)
  })

  //不能直接侦听响应式对象的属性
  const obj = reactive({ count: 0 })
  // 错误，因为 watch() 得到的参数是一个 number
  watch(obj.count, (count) => {
    console.log(`count is: ${count}`)
  })

  // 提供一个 getter 函数
  watch(() => obj.count, (count) => {
      console.log(`count is: ${count}`)
    }
  )
  ```
* 返回值：返回一个函数，执行该函数可以停止侦听

  ```js
  1.同步语句创建的侦听器，会自动绑定到宿主组件实例上，并且会在宿主组件卸载时自动停止。
  2.如果用异步回调创建一个侦听器，那么它不会绑定到当前组件上，你必须手动停止它，以防内存泄漏。
  <script setup>
  import { watch,ref } from 'vue'
  const count = ref(0)
  watch(count,() => {})// 它会自动停止,会随组件实例自动卸载

  setTimeout(() => {
    watch(count,() => {})// 它会自动停止
  }, 100)
  </script>

  const unwatch = watch(count,() => {})//返回一个函数，执行该函数，就会卸载该侦听器

  // ...当该侦听器不再需要时
  unwatch()
  ```
* 注意点：默认情况下，**侦听器回调都会在 Vue 组件更新之前被调用**。这意味着**回调中访问的 DOM 将是被 Vue 更新之前的状态**。

  ```js
  watchOption配置flush: 'post'，可以在组件更新完成之后调用注册的回调函数，此时可以拿到最新的组件dom
  watch(source, callback, {
    flush: 'post'
  })

  watchEffect(callback, {
    flush: 'post'
  })

  //watchEffect的别名函数
  import { watchPostEffect } from 'vue'
  watchPostEffect(() => {
    /* 在 Vue 更新后执行 */
  })
  ```

### watchEffect

* 概念：`watchEffect()`​ 允许我们自动跟踪回调的响应式依赖。并先执行一次

  ```js
  watch 、 watchEffect 的区别
  1.watch 
  只追踪明确侦听的数据源。它不会追踪任何在回调中访问到的东西。
  仅在数据源确实改变时才会触发回调。
  watch 会避免在发生副作用时追踪依赖，因此，我们能更加精确地控制回调函数的触发时机。

  2.watchEffect
  则会在副作用发生期间追踪依赖。它会在同步执行过程中，自动追踪所有能访问到的响应式属性。
  这更方便，而且代码往往更简洁，但有时其响应性依赖关系会不那么明确
  ```
* 使用

  ```js
  import { ref,watch,watchEffect } from "vue"
  const todoId = ref(1)
  const data = ref(null)

  //可以起到和下面类似的效果
  watchEffect(async () => {
    const response = await fetch(
      `xxxxx/${todoId.value}`//会自动跟踪todoId的变化，只是语义上看起来没watch明确
    )
    data.value = await response.json()
  })

  watch(todoId, async () => {
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
    )
    data.value = await response.json()
  }, { immediate: true })

  ```

### nextTick

当你更改响应式状态后，DOM 会自动更新。注意 DOM 的更新并不是同步的。相反，Vue 将缓冲它们直到更新周期的 “下个时机” 以确保无论你进行了多少次状态更改，每个组件都只需要更新一次

```js
import { nextTick } from 'vue'
function increment() {
  state.count++
  nextTick(() => { }) // 访问更新后的 DOM
}
```

### provide、inject

* 概念

  ```js
  provide：依赖，提供，父组件提供一个依赖给子组件使用
  provide() 
  1.接受两个参数：第一个参数是要注入的 key，可以是一个字符串或者一个 symbol，第二个参数是要注入的值。

  inject：注入，把父组件提供的依赖注入到组件之中使用
  inject() 
  1.第一个参数是注入的 key。
  2.第二个参数是可选的，即在没有匹配到 key 时使用的默认值。
  3.第二个参数也可以是一个工厂函数，用来返回某些创建起来比较复杂的值。在这种情况下，你必须将 true 作为第三个参数传入，表明这个函数将作为工厂函数使用，而非值本身。
  4.Vue 会遍历父组件链，通过匹配 key 来确定所提供的值。如果父组件链上多个组件对同一个 key 提供了值，那么离得更近的组件将会“覆盖”链上更远的组件所提供的值。如果没有能通过 key 匹配到值，inject() 将返回 undefined，除非提供了一个默认值。
  ```
* 基本使用：建议注入的值由供给方来修改，即谁提供值，谁来提供修改的方法，可以把值和修改的方法一起提供，接收方获得方法来修改值

  ```js
  provide
  <script setup>
  import { ref, provide } from 'vue'

  provide('foo', 'bar')// 提供静态值

  const count1 = ref(0)
  provide('count', count1)// 提供响应式的值

  const count2 = ref(0)
  provide('read-only-count', readonly(count2))//使用readonly来包裹数据，设置数据为只读属性，避免后代使用时被修改
  </script>

  inject
  <script setup>
  import { inject } from 'vue'
  import { fooSymbol } from './injectionSymbols'
  const foo = inject('foo')// 注入不含默认值的静态值
  const count = inject('count')// 注入响应式的值
  const bar = inject('foo', 'default value')// 注入一个值，若为空则使用提供的默认值
  const fn = inject('function', () => {})// 注入一个值，若为空则使用提供的函数类型的默认值
  const baz = inject('factory', () => new ExpensiveObject(), true)// 注入一个值，若为空则使用提供的工厂函数
  </script>
  ```

### defineExpose

```js
//把方法和属性暴露出去，外界获得了这个组件dom，就可以使用这个组件里面的方法或属性
//不能像vue2中那样使用ref直接操作子组件的属性和方法了，因为你设置了setup，不设置的话，你是可以和以前一样使用的
<script setup>
let data = ref({data:data})
let fn = ()=>{}
defineExpose({
  data,
  fn
})
</script>
```

### defineProps

* 概念：只能在`<script setup>`​ 中使用的**编译器宏**

  ```js
  1.无需引入，可以直接使用
  2.可以使用字符串、数组、对象等形式作为参数
  3.会返回一个响应式对象，使用的时候可以使用点语法 props.name。在模板中也可以直接使用，如：name（name在defineProps中声明过了）
  4.如果返回的对象props进行解构，那解构后的数据将不具有相应式
  let props = defineProps('propA')
  let props = defineProps(['propA','propB'])
  let props = defineProps({
    propA: Number,// 基础类型检查，设置为null 和 undefined 则会跳过任何类型检查
    propB: [String, Number],// 多种可能的类型
    propC: {
      type: String,
      required: true// 必传，且为 String 类型
    },
    propD: {
      type: Number, 
      default: 100  //默认值
    }
  })
  ```
* 父组件传值

  ```html
  //单个传值
  <son :propA="xxx"></son>

  //传递对象
  const post = ref({name:'zhangsan',age:20})
  <son v-bind="post" />
  <son :="post" />
  <son :name="post.name" :age="post.age" /> //这三个是等效的,这种写法相当于 解构后传递对象了
  ```
* 子组件接收

  ```js
  let props = defineProps(['name','age'])
  let { name} = defineProps(['name','age'])//这种解构写法会失去响应式

  <template>
    <div>{{ props.name }}</div>
  </template>

  //或者可以直接使用
  <template>
    <div>{{ propA }}</div>
  </template>
  ```
* 数据校验：可以声明对`props`​的校验，当传入值不符合时，会在控制台抛出警告。validator:(value)=>{return :boolean}

  ```js
  defineProps({
    // 对象类型的默认值
    propA: {
      type: Object,
      // 对象或数组的默认值
      // 必须从一个工厂函数返回。
      // 该函数接收组件所接收到的原始 prop 作为参数。
      default(rawProps) {
        return { message: 'hello' }
      }
    },
    // 自定义类型校验函数
    propB: {
      validator(value) {
        return ['success', 'warning', 'danger'].includes(value)
      }
    },
    // 函数类型的默认值
    propC: {
      type: Function,
      // 不像对象或数组的默认，这不是一个工厂函数。这会是一个用来作为默认值的函数
      default() {
        return 'Default function'
      }
    }
  })

  ```

### defineEmits

* 定义：只能在`<script setup>`​ 中使用的**编译器宏**。组件要触发的事件可以显式地通过 `defineEmits()`​来声明

  ```js
  <script setup>
    defineEmits()不能在子函数中使用。必须直接放置在<script setup>的顶级作用域下。
    const emit = defineEmits(['inFocus', 'submit'])
    function buttonClick() {
      emit('submit',value)//可以在该时间传入默认参数
    }
  </script>

  //另外一种方式
  <template>
    <div class="blog-post">
      <h4>{{ title }}</h4>
      <button @click="$emit('change')">Enlarge text</button>
      <button @click="$emit('enlarge-text',value)">Enlarge text</button>
    </div>
  </template>
  ```
* 使用：在父组件中触发该函数

  ```html
  <son @submit='fn'></son>
  fn(value){
    console.log(value)//value就是子组件传入的值
  }
  ```
* ts写法

  ```js
  <script setup lang="ts">
  const emit = defineEmits<{
    (e: 'change', id: number): void
    (e: 'update', value: string): void
  }>()
  </script>
  ```
* 事件校验：事件名称可以被赋值为一个函数，接受的参数就是抛出事件时传入 `emit`​ 的内容，即传入事件的参数，返回一个布尔值来表明事件是否合法

  ```js
  <script setup>
  const emit = defineEmits({
    // 没有校验
    click: null,
    // 校验 submit 事件
    submit: ({ email, password }) => {
      if (email && password) {
        return true
      } else {
        console.warn('Invalid submit event payload!')
        return false
      }
    }
  })

  function submitForm(email, password) {
    emit('submit', { email, password })
  }
  </script>

  ```

### shallReadonly

* 只有根层级的属性变为了只读。属性的值都会被原样存储和暴露，这也意味着值为 ref 的属性**不会**被自动解包了。

  ```js
  const state = shallowReadonly({
    foo: 1,
    nested: {
      bar: 2
    }
  })

  // 更改状态自身的属性会失败
  state.foo++

  // ...但可以更改下层嵌套对象
  isReadonly(state.nested) // false

  // 这是可以通过的
  state.nested.bar++
  ```

### effectScope

返回一个对象，创建一个 `effect `​作用域，可以捕获其中所创建的响应式副作用 (即计算属性和侦听器)，这样捕获到的副作用可以一起处理

```js
const scope = effectScope()
scope.run(() => {
  const doubled = computed(() => counter.value * 2)
  watch(doubled, () => console.log(doubled.value))
  watchEffect(() => console.log('Count: ', doubled.value))
})
// 处理掉当前作用域内的所有 effect
scope.stop()
```

‍

# 组件

### 组件注册

* 全局组件注册

  ```ts
  import { createApp } from 'vue'
  const app = createApp({})
  //.component('name'，组件)//该api有两个参数，第一个是组件的名称，第二个是组件的实例文件
  import MyComponent from './App.vue'
  app.component('MyComponent', MyComponent).component('ComponentB', ComponentB)//也可以链式调用
  ```
* 局部组件注册

  ```ts
  vue3中，在setup中导入的组件，可以直接在模板中使用，不需要再去显式的去声明
  <script setup>
  import ComponentA from './ComponentA.vue'
  </script>

  <template>
    <ComponentA />
  </template>
  ```

* ​`defineAsyncComponent`​：远程获取

  ```js
  import { defineAsyncComponent } from 'vue'
  // ... 像使用其他一般组件一样使用 AsyncComp

  1.远程获取
  const AsyncComp = defineAsyncComponent(() => {
    return new Promise((resolve, reject) => {
      // ...从服务器获取组件
      resolve(/* 获取到的组件 */)
    })
  })

  2.懒加载
  import { defineAsyncComponent } from 'vue'
  //使用还是如上
  const AsyncComp = defineAsyncComponent(() =>
    import('./components/MyComponent.vue')
  )
  ```
* 错误处理：使用其他参数，来实现不同的效果

  ```js
  const AsyncComp = defineAsyncComponent({
    // 加载函数
    loader: () => import('./Foo.vue'),
    // 加载异步组件时使用的组件
    loadingComponent: LoadingComponent,
    // 展示加载组件前的延迟时间，默认为 200ms
    delay: 200,
    // 加载失败后展示的组件
    errorComponent: ErrorComponent,
    // 如果提供了一个 timeout 时间限制，并超时了
    // 也会显示这里配置的报错组件，默认值是：Infinity
    timeout: 3000
  })
  ```

### 生命周期

生命周期这个钩子函数，是随着组件的实例话过程被调用执行

```js
//这些带on的都需要按需导入，参数都是一个可以执行的回调函数
import { ...on生命周期钩子函数 } from "vue"
setup()  //开始创建组件之前，在beforeCreate和created之前执行。创建的是data和method
组件挂载
onBeforeMount()  //组件挂载到节点上之前执行的函数。
onMounted()  //组件挂载完成后执行的函数。
组件数据更新
onBeforeUpdate()  //组件更新之前执行的函数。
onUpdated()  //组件更新完成之后执行的函数。
//组件销毁
onBeforeUnmount()  //组件卸载之前执行的函数。 //销毁的和vue2中差距较大
onUnmounted()  //组件卸载完成后执行的函数
//组件显示和隐藏
onActivated()  //被包含在中的组件，会多出两个生命周期钩子函数。被激活时执行。
onDeactivated()  //比如从 A 组件，切换到 B 组件，A 组件消失时执行。
onErrorCaptured()  //当捕获一个来自子孙组件的异常时激活钩子函数

//使用
onMounted(()=>{})
```

​![vue3生命周期](assets/vue3生命周期-20230810215527-5yx7vd8.png)​

### 动态组件

使用内部组件`compoent`​，通过传入组件名称动态渲染不同的组件

```html
:is可输入的值  组件的名称，导入的组件对象
可以使用<KeepAlive></KeepAlive>再组件切换之后，保留在内存中，减少切换的损耗
<component :is="componentName"></component>
```

### useAttrs

* 注意：使用`useAttrs`​方法返回的`attrs`​对象不是**响应式的**，不能通过`watch`​去侦听它的变化。

* 概念：“透传 attribute”指的是传递给一个组件，却没有被该组件声明为` props`​ 或 `emits`​的`attribute`​或者 `v-on`​ 事件监听器。

  这意味着当我们有一个单根节点的子组件时，这些绑定会被作为一个常规的`HTML attribute`​应用在该子组件的根节点元素上。

  ```html
  1.简单来说，就是没有被props、emit显示声明的，由父组件对在子组件身上传递过来的属性和触发方法
  2.通过返回的对象，可以拿到父组件传递来的属性，可以在子组件内，绑定dom，通过某种方法触发父组件的事件。
  3.vue2中是拿不到class和style的，同时事件监听器需用另外一个参数来接收：$lisenter
  4.$attrs 不是响应式对象，要获取它的最新值，可以在 updata 生命周期中重新获取

  <son class='red' name="name" @click="fn"></son>//在子组件上传递了class、name属性，和click事件
  click通过v-on绑定到了<son/>的根元素上，点击会触发父组件的fn，如果<son/>的click也绑定了事件，那么会一起被触发。

  import { useAttrs } from 'vue'
  const attrs = useAttrs()
  ```
* 深层组件继承（自动继承到根节点上）

  ```js
  组件的根节点使用另一个组件，该组件接收的透传attribute会直接继续传给在根节点组件
  如:在<son/>组件中直接使用另一个组件
  <template>
    <GrandSon/> //<Son>组件接收的透传attributes给传给<GrandSon>,除非在<Son>中props和emits消耗了传来的attributes
  </template>

  如果<son/>的根节点下有多个多根节点模板，由于 Vue 不知道要将 attribute 透传到哪，会报一个警告
  <template>
    <Title/>  
    <Footer v-bind="$attrs"/>//需要显示的一个警告，把$attrs绑定报节点上，警告就会消失
  </template>
  ```
* 关闭透传：透传是默认是开启的。利用**​`inheritAttrs:false`​**​，使用 **​`$attrs`​**​来获得传入的属性和事件

  ```js
  1.关闭透传后，属性和事件就不会再作用到组件的根节点上了
  2.使用 useAttrs 拿到对应的透传对象

  <script>
  // 使用普通的 <script> 来声明选项
  export default {
    inheritAttrs: false//关闭透传
  }
  </script>

  从 vue 3.3 开始你也可以直接在 <script setup> 中使用 defineOptions
  <script setup>
  defineOptions({
    inheritAttrs: false//关闭透传递
  })
  </script>

  获取透传对象
  <span>{{ $attrs }}</span>
  props:  $attrs['foo-bar']  //会保留传入时候的大小写的
  如@click这样的一个 v-on 事件监听器将在此对象下被暴露为一个函数 $attrs.onClick
  ```

### $parent

* 概念：组件是由层级嵌套的关系的，使用`ref`​的模板引用的时候，只能在父组件中使用，去拿到子组件的实例，子组件中无法通过`ref`​去拿到父组件（子组件中都看不到父组件的结构怎么去拿）
* 获取

  ```html
  //在子组件中
  <template>
    <div>
      <button @click="fn($parent)"></button>
    </div
  </template>

  <script setup>
    fn($parent){
      console.log($parent.value)//拿到的是引用该子组件的最近的父组件，同样的，要拿到父组件的值和方法，要在父组件中声明暴露
    }
  </script> 
  ```

### v-slot

​![插槽](assets/插槽-20230812150635-h738vi5.png)​

* 概念：把父组件传入子组件中的内容放到子组件中进行渲染

  ```js
  在子组件中
  <button class="fancy-btn">
    <slot></slot> //设置了一个默认插槽的出口，父组件在子组件中放置的内容默认放在这个出口，默认的名字其实就是default
    <diev>分割线分割线<div>
    <slot name="footer"></slot> //设置了一个具名插槽叫做footer，只有v-solt="footer" 的内容才会放到这个位置
  </button>

  父亲组件中
  <BaseLayout>
    默认插槽和具名插槽同时使用，默认插槽的内容要使用 template 包裹
    <template #default> //也可以不写这个default
      <span>给子组件的内容</span>//给子组件的默认插槽放这个内容
    </template>

    <template v-slot:footer> 给子组件的footer具名插槽放这个内容 v-slot:footer 缩写是 #footer
      <!-- footer插槽的内容放这里 -->
    </template>
  </BaseLayout>
  ```
* 作用域插槽：父组件使用子组件的插槽时，是不能访问子组件的状态，需要子组件把需要的数据主动的放到插槽中让父组件使用

  ​![作用域插槽](assets/作用域插槽-20230812153438-0pyi0wo.svg)​

  ```html
  //子组件
  <div>
    <slot :text="greetingMessage" :count="1"></slot> //子组件在默认插槽中传入两个数据
  </div>

  //父组件中
  <MyComponent v-slot="slotProps">
    {{ slotProps.text }} {{ slotProps.count }} //父组件中，使用 v-slot="slotProps" 或者 #default=“obj” ，设置一个变量名称，来接收子组件传递过来的数据，这些数据会变成一个对象
  </MyComponent>
  ```
* 注意：**同时使用了具名插槽与默认插槽，则需要为默认插槽使用显式的 **​ **​`<template>`​** ​ ** 标签**

  ```html
  <template>
    <MyComponent>
      <!-- 使用显式的默认插槽 -->
      <template #default="{ message }">
        <p>{{ message }}</p>
      </template>

      <template #footer>
        <p>Here's some contact info</p>
      </template>
    </MyComponent>
  </template>
  ```

### 模板引用

* 概念：可以使用 `ref`​  用于注册元素或子组件的引用。**注意：如果实在v-for里面获取的组件，对应的ref是个数组**

  ```js
  1.如果用于普通 DOM 元素，引用将是元素本身；如果用于子组件，引用将是子组件的实例
  2.因为 ref 本身是作为渲染函数的结果来创建的，必须等待组件挂载后才能对它进行访问
  3.vue3中拿到子组件vc的时候，默认是获取不到子组件的数据和方法，组件的数据和方法默认是关闭的，需要在组件中主动暴露才可以通过组件实例来获取到组件内部暴露出来的方法和数据  
  ```
* 获取模板

  ```js
  <template>
    <son ref="p"></son>
  </template>

  <script setup>
  import { ref } from 'vue'
  const p = ref()
  console.log(p.value.name)//p是的代理对象，需要 .value  这样是获取组件p中的name属性，这个属性要暴露出来才能获取得到
  </script>


  //---------------
  <son>
  <script setup>
  lei name = ref('子组件名字')
  difineExpose({name})//在子组件中需要暴露出来才能通过模板引用拿到该值
  </script>
  </son>
  ```
* 获得数据

  ```js
  <script setup>
  import { ref, onMounted } from 'vue'
  // 声明一个 ref 来存放该元素的引用,必须和模板里的 ref 同名
  const input = ref(null)
  const son= ref(null)
  const listRef = ref([])//同时使用v-for的情况下，里面包含的是整个dom的数组

  //对dom操作要在实例化完成之后才行，不然会无法完成
  onMounted(() => {
    input.value.focus()
  })
  </script>
  <template>
    <input ref="input" />
     <son ref="son"></son>
    <div v-for="item in 999" :key="index" ref="listRef"></div>
  </template>
  ```
* 模板引用标注类型：模板引用需要通过一个**显式指定的泛型参数**和一个初始值 `null`​ 来创建

  ```html
  <script setup lang="ts">
  import { ref, onMounted } from 'vue'
  const el = ref<HTMLInputElement | null>(null)
  onMounted(() => {
    el.value?.focus()//可选链
  })
  </script>

  <template>
    <input ref="el" />
  </template>

  //方法二
  <!-- App.vue -->
  <script setup lang="ts">
  import MyModal from './MyModal.vue'

  const modal = ref<InstanceType<typeof MyModal> | null>(null)

  const openModal = () => {
    modal.value?.open()
  }
  </script>

  ```

### KeepAlive

* 概念：可以用来缓存组件，保持组件不用时不被销毁
* 基本使用

  ```js
  <!-- 非活跃的组件将会被缓存！ -->
  <KeepAlive>
    <component :is="activeComponent" />
  </KeepAlive>
  ```
* 其他属性

  ```js
  include:包含那些可以缓存
  exclude:排除那些可以缓存
  include\exclude，会根据组件的name选项来进行匹配，需要缓存的话，就需要显式的声明一个name选项
  在 3.2.34 或以上的版本中，使用 <script setup> 的单文件组件会自动根据文件名生成对应的 name 选项，无需再手动声明。
  mix:最大缓存组件数量
  新增生命周期

  <script setup>
  import { onActivated, onDeactivated } from 'vue'
    //onActivated 在组件挂载时也会调用，并且 onDeactivated 在组件卸载时也会调用
    onActivated(() => {})
    onDeactivated(() => {})
  </script>

  <!-- 以英文逗号分隔的字符串 -->
  <KeepAlive include="a,b">
    <component :is="view" />
  </KeepAlive>

  <!-- 正则表达式 (需使用 `v-bind`) -->
  <KeepAlive :include="/a|b/">
    <component :is="view" />
  </KeepAlive>

  <!-- 数组 (需使用 `v-bind`) -->
  <KeepAlive :include="['a', 'b']">
    <component :is="view" />
  </KeepAlive>

  <KeepAlive :max="10">
    <component :is="activeComponent" />
  </KeepAlive>
  ```

### Teleport

* 概念：它可以将一个组件内部的一部分模板“传送”到该组件的 DOM 结构外层的位置去。
* 基本使用：把你这个单文件的某一部分dom`传送`​到别去dom之中

  ```js
  <Teleport> 接收一个 to prop 来指定传送的目标。
  to 的值可以是一个 CSS 选择器字符串，也可以是一个 DOM 元素对象。
  这段代码的作用就是告诉 Vue“把以下模板片段传送到 body 标签下”

  <button @click="open = true">Open Modal</button>
  <Teleport to="body">
    <div v-if="open" class="modal">
      <p>Hello from the modal!</p>
      <button @click="open = false">Close</button>
    </div>
  </Teleport>
  ```

### Suspense

* 概念：可以等待组件的异步操作，都成功之后再统一显示
* 基本使用

  ```js
  //Dashboard异步组件未成功获取，就先显示下面的 #fallback 插槽的内容。
  //异步成功之后， #fallback 状态结束，显示正常组件
  <Suspense>
    <!-- 具有深层异步依赖的组件 -->
    <Dashboard />

    <!-- 在 #fallback 插槽中显示 “正在加载中” -->
    <template #fallback>
      Loading...
    </template>
  </Suspense>
  ```
* 异步组件

  ```js
  1.在setup顶层中使用await
  <script setup>
  const res = await fetch(...)
  const posts = await res.json()
  </script>
  <template>
    {{ posts }}
  </template>

  2.使用defineAsyncComponent获取的组件
  import { defineAsyncComponent } from 'vue'
  const AsyncComp = defineAsyncComponent(() => {
    return new Promise((resolve, reject) => {
      // ...从服务器获取组件
      resolve(/* 获取到的组件 */)
    })
  })
  // ... 像使用其他一般组件一样使用 `AsyncComp`
  ```

# 指令

### v-model

* 概念：在表单输入元素或组件上创建双向绑定。**表单元素：input、select、textarea、components，会根据绑定的原生标签使用不同的$emit('input')事件**
* 修饰符

  ```js
  .number  输入的字符转为数字类型（整数形式）。如果该值无法被 parseFloat() 处理，那么将返回原始值
  .lazy  侦听change事件而不是input事件
  .trim  移除输入内容两端空格
  .capitalize  输入的字符第一个大写
  ```
* 自定义修饰符：给`v-model`​添加的自定义修饰符，可以通过 `prop.modelModifiers`​ 拿到

  ```js
  <MyComponent v-model.capitalize="myText" /> 自定义加了一个capitalize修饰符

  const props = defineProps({
    modelValue: String,
    modelModifiers: {default:()=>({})}//通过 defineProps 中的 modelModifiers 来获取 自定义修饰符的参数，默认是空对象
  })

  function emitValue(e) {
    let value = e.target.value 
    if (props.modelModifiers.capitalize) { //通过props.modelMofifier.xxx 来获取自己设置的修饰符
      value = value.charAt(0).toUpperCase() + value.slice(1)
    }
    emit('update:modelValue', value)
  }

  <MyComponent v-model:title.capitalize="myText">//这种就是又带参数，又带修饰符的  defineProp(['title'])
  ```
* 语法糖：和`vue2`​中的基本一样，vue2使用的`:value`​，`@input`​，vue3使用的是`:modelValue`​，`@updata:modelValue`​

  ```html
  v-model会根据绑定的不同标签使用对应的dom属性和事件组合
  1.文本类型的 <input> 和 <textarea> 元素会绑定 value property 并侦听 input 事件
  2.<input type="checkbox"> 和 <input type="radio"> 会绑定 checked property 并侦听 change 事件；
  3.<select> 会绑定 value property 并侦听 change 事件

  //vue2在表单元素中使用的时候
  <input :value="text"  @input="event => text = event.target.value">
  <input v-model="text">

  //vue2在自定义组件中的使用
  <input :value="text"  @input="newValue=> text = newValue">
  <input v-model="text">

  vue3在原生表单原生中的使用
  将内部原生 input 元素的 value attribute 绑定到 modelValue prop
  输入新的值时在 input 元素上触发 update:modelValue 事件
  <template>
    <input :value="modelValue" @input="$emit('update:modelValue', $event.target.value)" />
  </template>

  vue3在自定义组件中的使用
  <CustomInput v-model='searchText' />
  <CustomInput :modelValue="searchText"  @update:modelValue="newValue => searchText = newValue"/>

  <script setup>
  defineProps(['modelValue'])
  defineEmits(['update:modelValue'])
  </script>

  vue3可以绑定其他值
  默认情况下，v-model 在组件上都是使用 modelValue 作为 prop，并以 update:modelValue 作为对应的事件。
  可以通过给 v-model:xxx="" 指定一个参数来更改这些名字
  modelValue => xxx    updata:madelValue => updata:xxx
  <CustomInput v-model:name='faterName' />
  <CustomInput :name="faterName"  @update:name="newValue=> name= newValue"/>

  <script setup>
  defineProps(['name'])
  defineEmits(['update:name'])
  </script>
  ```

### v-text

```html
<span v-text="msg"></span>
<!-- 等同于 -->
<span>{{msg}}</span>
```

### v-html

```html
在单文件组件中，scoped样式是不会作用到v-html里面的内容
动态渲染任意的 HTML 是非常危险的，因为它很容易导致 XSS 攻击。
<div v-html="html"></div>
```

### v-show

​`v-show`​ 会在 DOM 渲染中保留该元素；`v-show`​ 仅切换了该元素上名为 `display`​ 的 CSS 属性，即` display : none`​

​`v-show`​ 不支持在 `<template>`​ 元素上使用，也不能和 `v-else`​ 搭配使用

```html
<h1 v-show="ok">Hello!</h1>
```

### v-if

**vue3里面，v-if比v-for的优先级要更高(vue2中则相反)，一起使用的话，会导致v-if没法使用到v-for作用域的变量别名。**

​`v-if`​ 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回真值时才被渲染。

​`v-if`​ 有更高的切换开销，而 `v-show`​ 有更高的初始渲染开销。

```html
const type = ref('B')
<div v-if="type === 'A'">A</div>
<div v-else-if="type === 'B'">B</div>
<div v-else-if="type === 'C'">C</div>
<div v-else>Not A/B/C</div>

<template></template> 包装器容器，实际是不会渲染的
```

### v-for

​`v-for`​ 指令的值需要使用 `item in items`​ 形式的特殊语法，其中 `items`​ 是源数据的数组，而 `item`​ 是迭代项的**别名**

​`key属性`​：Vue 默认按照“就地更新”的策略来更新通过 `v-for`​ 渲染的元素列表。当数据项的顺序改变时，Vue 不会随之移动 DOM 元素的顺序，而是就地更新每个元素，确保它们在原本指定的索引位置上渲染。

```js
const items = ref([{ message: 'Foo' }, { message: 'Bar' }])

item是迭代项的别名，index是数组的下标
<li v-for="(item, index) in items" :key="item.messgae">
  {{ parentMessage }} - {{ index }} - {{ item.message }}
</li>


可以使用 of 作为分隔符来替代 in，这更接近 JavaScript 的迭代器语法
<div v-for="item of items" :key="item.messgae"></div>

可以使用 v-for 来遍历一个对象的所有属性。遍历的顺序会基于对该对象调用 Object.keys() 的返回值来决定。 // vaule , key , index 
const myObject = reactive({
  title: 'How to do lists in Vue',
  author: 'Jane Doe',
  publishedAt: '2016-04-10'
})
<li v-for="(value, key, index) in myObject" :key="item.title">  
  {{ index }}. {{ key }}: {{ value }}
</li>

使用范围值：n是从1开始的
<span v-for="(n,index) in 10" :key="index">{{ n }}</span>

```

### v-on

​`v-on`​：监听事件，`v-on`​ 指令 (简写为 `@`​) 来监听 DOM 事件，并在事件触发时执行对应的 JavaScript。

用法：`v-on:click="handler"`​ 或 `@click="handler"`​。（**内联事件处理器、方法事件处理器**）

​`foo`​、`foo.bar`​ 和 `foo['bar']`​ 会被视为方法事件处理器，而 `foo()`​ 和 `count++`​ 会被视为内联事件处理器。

* 基本使用

  ```js
  内联事件处理器
  const count = ref(0)
  <button @click="count++">Add 1</button>

  方法事件处理器
  <button @click="greet">Greet</button>

  const name = ref('Vue.js')
  function greet(event) {
    // event 是 DOM 原生事件
    if (event) {
      alert(event.target.tagName)
    }
  }
  ```
* 使用原生的事件对象 event

  ```js
  <!-- 使用特殊的 $event 变量 -->
  <button @click="warn('Form cannot be submitted yet.', $event)">
    Submit
  </button>

  <!-- 使用内联箭头函数 -->
  <button @click="($event) => warn('Form cannot be submitted yet.', $event)">
    Submit
  </button>
  ```
* 事件修饰符：`event.preventDefault()`​ 取消标签的默认事件或 `event.stopPropagation()`​阻止事件冒泡

  ```js
  修饰符可以链式调用，就是说可以一次写多个，功能是按照顺序来的
  .stop  防止事件冒泡
  .prevent  禁用标签的默认事件
  .self  事件只能由其本身触发（防止冒泡导致的触发）
  .capture  开启事件捕捉
  .once  只触发一次
  .passive   修饰符一般用于触摸事件的监听器，可以用来改善移动端设备的滚屏性能，相当于给onscroll事件整了一个.lazy修饰符

  按键修饰符
  @keyup.enter 按键修饰符，回车
  .tab .delete .esc .space .up .down .left .right .ctrl .alt .shift .meta
  <!-- Alt + Enter -->
  <input @keyup.alt.enter="clear" />
  <!-- Ctrl + 点击 -->
  <div @click.ctrl="doSomething">Do something</div>
  ```

### v-bind

```js
1.动态绑定attribute（特性），也可以是组件的prop（属性---所传入组件所需要的组件属性）
2.vue会优先对传入的值对prop进行匹配，然后才是attr，需要强制匹配prop或者attr可以使用修饰符
3.缩写 v-bind:src=''   ===   :src=''

修饰符：
.camel ——将短横线命名的 attribute 转变为驼峰式命名。
.prop ——强制绑定为 DOM property。3.2+
.attr ——强制绑定为 DOM attribute。3.2+

<!-- 绑定 attribute -->
<img v-bind:src="imageSrc" />
<!-- 动态 attribute 名 -->
<button v-bind:[key]="value"></button>
<!-- 缩写 -->
<img :src="imageSrc" />
<!-- 缩写形式的动态 attribute 名 -->
<button :[key]="value"></button>
<!-- 内联字符串拼接 -->
<img :src="'/path/to/images/' + fileName" />+

<!-- prop 绑定。“prop” 必须在子组件中已声明。 -->
<MyComponent :prop="someThing" />
<!-- 传递子父组件共有的 prop -->
<MyComponent v-bind="$props" />
<!-- XLink -->
<svg><a :xlink:special="foo"></a></svg>


<!-- class 绑定 -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]"></div>

<!-- style 绑定 -->
<div :style="{ fontSize: size + 'px' }"></div>
<div :style="[styleObjectA, styleObjectB]"></div>
<!-- 绑定对象形式的 attribute -->
<div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

<!-- 一般 style写法 即style内联写法 -->
<div style="color:red"></div>
```

### v-slot

​![插槽](assets/插槽-20230812150635-h738vi5.png)​

* 概念：把父组件传入子组件中的内容放到子组件中进行渲染

  ```js
  在子组件中
  <button class="fancy-btn">
    <slot></slot> //设置了一个默认插槽的出口，父组件在子组件中放置的内容默认放在这个出口，默认的名字其实就是default
    <diev>分割线分割线<div>
    <slot name="footer"></slot> //设置了一个具名插槽叫做footer，只有v-solt="footer" 的内容才会放到这个位置
  </button>

  父亲组件中
  <BaseLayout>
    默认插槽和具名插槽同时使用，默认插槽的内容要使用 template 包裹
    <template #default> //也可以不写这个default
      <span>给子组件的内容</span>//给子组件的默认插槽放这个内容
    </template>

    <template v-slot:footer> 给子组件的footer具名插槽放这个内容 v-slot:footer 缩写是 #footer
      <!-- footer插槽的内容放这里 -->
    </template>
  </BaseLayout>
  ```
* 作用域插槽：父组件使用子组件的插槽时，是不能访问子组件的状态，需要子组件把需要的数据主动的放到插槽中让父组件使用

  ​![作用域插槽](assets/作用域插槽-20230812153438-0pyi0wo.svg)​

  ```html
  //子组件
  <div>
    <slot :text="greetingMessage" :count="1"></slot> //子组件在默认插槽中传入两个数据
  </div>

  //父组件中
  <MyComponent v-slot="slotProps">
    {{ slotProps.text }} {{ slotProps.count }} //父组件中，使用 v-slot="slotProps" 或者 #default=“obj” ，设置一个变量名称，来接收子组件传递过来的数据，这些数据会变成一个对象
  </MyComponent>
  ```
* 注意：**同时使用了具名插槽与默认插槽，则需要为默认插槽使用显式的**  **​`<template>`​** ​ **标签**

  ```html
  <template>
    <MyComponent>
      <!-- 使用显式的默认插槽 -->
      <template #default="{ message }">
        <p>{{ message }}</p>
      </template>

      <template #footer>
        <p>Here's some contact info</p>
      </template>
    </MyComponent>
  </template>
  ```

### v-pre

跳过编译

```html
跳过该元素及其所有子元素的编译。
元素内具有 v-pre，所有 Vue 模板语法都会被保留并按原样渲染。最常见的用例就是显示原始双大括号标签及内容
<span v-pre>{{ this will not be compiled }}</span>
```

### v-once

```js
仅渲染元素和组件一次，并跳过之后的更新。
在随后的重新渲染，元素/组件及其所有子项将被当作静态内容并跳过渲染。这可以用来优化更新时的性能
<!-- 单个元素 -->
<span v-once>This will never change: {{msg}}</span>
<!-- 带有子元素的元素 -->
<div v-once>
  <h1>comment</h1>
  <p>{{msg}}</p>
</div>
<!-- 组件 -->
<MyComponent v-once :comment="msg" />
<!-- `v-for` 指令 -->
<ul>
  <li v-for="i in list" v-once>{{i}}</li>
</ul>
```

### v-memo

​`v-memo=any[]`​:缓存组件

```js
缓存一个模板的子树。在元素和组件上都可以使用。为了实现缓存，该指令需要传入一个固定长度的依赖值数组进行比较。
如果数组里的每个值都与最后一次的渲染相同，那么整个子树的更新将被跳过。

<div v-memo="[valueA, valueB]">
  ...
</div>
当组件重新渲染，如果 valueA 和 valueB 都保持不变，这个 <div> 及其子项的所有更新都将被跳过。
实际上，甚至虚拟 DOM 的 vnode 创建也将被跳过，因为缓存的子树副本可以被重新使用。
v-memo 传入空依赖数组 (v-memo="[]") 将与 v-once 效果相同。
```

### v-cloak

```js
用于隐藏尚未完成编译的 DOM 模板。
当使用直接在 DOM 中书写的模板时，可能会出现一种叫做“未编译模板闪现”的情况：用户可能先看到的是还没编译完成的双大括号标签，直到挂载的组件将它们替换为实际渲染的内容
v-cloak 会保留在所绑定的元素上，直到相关组件实例被挂载后才移除。
配合像 [v-cloak] { display: none } 这样的 CSS 规则，它可以在组件编译完毕前隐藏原始模板。


<div v-cloak>
  {{ message }}
</div>
[v-cloak] {
  display: none;
}
```

### 事件修饰符

事件对象在 `vue`​ 中相对来说用的少。在vue中可以使用修饰符来阻止事件默认行为、阻止冒泡等

* 基本语法

  ```html
  <标签 @事件名.修饰符="方法名"></标签>
  //修饰符可以添加多个
  <标签 @事件名.修饰符1.修饰符2="方法名"></标签>
  ```
* 常见修饰符

  ```js
  .lazy //改变输入框的值时value不会改变，当光标离开输入框时，v-model绑定的值value才会改变
  .trim  //把v-model绑定的值的首尾空格给过滤掉
  .number  //将值转成数字，先输入字符后数值，本次输入无效，先数值后字母，会截取前面数值部分
  .stop  //阻止冒泡
  .capture  //开始事件捕捉
  .self  //只有点击事件绑定的本身才会触发事件
  .once  //事件只执行一次
  .prevent  //阻止默认事件（例如a标签的跳转）
  .native  //加在自定义组件的事件上，可以是自定义组件使用原生click事件等，不用到自定义组件中再去设置
  .left .right .middle  //鼠标的左中右按键触发的事件
  .passive  //给onscroll事件整了一个.lazy修饰符，优化移动端的性能
  .sync  //语法糖，子组件传值方便
  .keyCode(.enter .tab .esc)  //修饰keyup等事件，确定那个键
  ```
* ​`.sync修饰符`​：要想修饰符生效，子组件内部做子传父时，发起数据请求时应该这样写：`this.$emit('updata:数值'，所需数据）`​

  ```html
  //传统父子双向传递时，在子组件标签中的写法
  <子组件 :show="editShow" @update:show="editShow = $event"/>

  //使用.sync修饰符,就相当是上面的写法了，替你写了 @update:show="editShow = $event"
  <子组件 :show.sync="editShow" />

  :show.sync="editShow"   ===   @update:show="editShow = $event"
  ```

### 自定义指令

* 创建vue3自定义指令：在 `<script setup>`​ 中，任何以`v`​开头的驼峰式命名的变量都可以被用作一个自定义指令。

  ```js
  <script setup>
  // 对象式写法，全局可以写成函数形式，这个我不知道行不行写成函数
  // 全局的函数式写法，是mounted和updated生命周期一起的。
  const vFocus = {
    mounted: (el) => el.focus()
  }
  </script>

  <template>
    <input v-focus />
  </template>

  ```

* 自定义指令的钩子

  ```js
  const vMyDirective = {
    // 在绑定元素的 attribute 前或事件监听器应用前调用
    created(el, binding, vnode, prevVnode) {},
    // 在元素被插入到 DOM 前调用
    beforeMount(el, binding, vnode, prevVnode) {},
    // 在绑定元素的父组件及他自己的所有子节点都挂载完成后调用
    mounted(el, binding, vnode, prevVnode) {},
    // 绑定元素的父组件更新前调用
    beforeUpdate(el, binding, vnode, prevVnode) {},
    // 在绑定元素的父组件及他自己的所有子节点都更新后调用
    updated(el, binding, vnode, prevVnode) {},
    // 绑定元素的父组件卸载前调用
    beforeUnmount(el, binding, vnode, prevVnode) {},
    // 绑定元素的父组件卸载后调用
    unmounted(el, binding, vnode, prevVnode) {}
  }
  ```
* 参数：除了**​`el`​**​​外，**其他属性都是只读的**

  ```js
  el：指令绑定到的元素。这可以用于直接操作 DOM。

  binding：一个对象，包含以下属性。
    value：传递给指令的值。例如在 v-my-directive="1 + 1" 中，值是 2。
    oldValue：之前的值，仅在 beforeUpdate 和 updated 中可用。无论值是否更改，它都可用。
    arg：传递给指令的参数 (如果有的话)。例如在 v-my-directive:foo 中，参数是 "foo"。
    modifiers：一个包含修饰符的对象 (如果有的话)。如 v-my-directive.foo.bar 中，修饰符对象是 { foo: true, bar: true }。
    instance：使用该指令的组件实例。
    dir：指令的定义对象。
  vnode：代表绑定元素的底层 VNode。
  prevNode：之前的渲染中代表指令所绑定元素的 VNode。仅在 beforeUpdate 和 updated 钩子中可用。


  <div v-example:foo.bar="baz">
  binding 参数会是一个这样的对象：
  {
    arg: 'foo',
    modifiers: { bar: true },
    value: /* `baz` 的值 */,
    oldValue: /* 上一次更新时 `baz` 的值 */
  }
  ```
* 注意：不建议在组件上直接使用自定义指令，和((20230205165505-bmllvnx "透传Attributes"))一样会作用在组件的根节点的  
  ​`vue3`​的`template`​下可以有多个根节点，`vue2`​只能有一个，不过还是建议`vue3`​就写一个根节点

  ```js
  1.自定义指令作用到根节点的时候，会抛出警告。
  2.和 attribute 不同，指令不能通过 v-bind="$attrs" 来传递给一个不同的元素
  ```

‍

# [router-v4.x](https://router.vuejs.org/zh/api/)

### 基本配置

1. **导航是异步的，返回值一般是**​**​`false`​**​**或**​**​`undefined`​**​ **，有返回值说明路由跳转失败**

    ```js
    可通过await等待路由跳转后再执行
    await router.push('/my-profile')
    this.isMenuOpen = false
    ```
2. 路由默认不区分大小写和最后的` /`​​

    ```js
    const router = createRouter({
      history: createWebHistory(),
      routes: [
        // 将匹配 /users/posva 而非：
        // - /users/posva/ 当 strict: true
        // - /Users/posva 当 sensitive: true
        { path: '/users/:id', sensitive: true },
        // 将匹配 /users, /Users, 以及 /users/42 而非 /users/ 或 /users/42/
        { path: '/users/:id?' },
      ],
      strict: true, // applies to all routes
    })
    ```
3. 路由默认` / `​​是根路径
4. 路由参数：`params`​、`query`​。`query 查询参数`​，拼接在`url`​上面的，`params`​在内存中，刷新就无了
5. ​`routes`​配置时候的一些属性

    ```js
    const routes = Array<RouteItem>

    interface RouteItem {
      name:string;//路由名称
      path:string;//路由路径
      query:object;//查询参数、放在url上
      params:object;//参数
      redirect:string; | {name:string};//重定向
      alias:string | string[];//别名
      meta:object; //路由元信息
      props:string | boolean | object //路由传值
      replace:boolean;//是否刷新记录？这个忘记了
      hash:string; //"#123" 哈希
    }
    ```

### 配置路由

* 安装路由

  ```js
  npm i vue-router
  yarn add vue-router@4
  ```
* 创建路由：`src/router/index.js`​

  ```js
  // 创建路由文件,导入创建路由对象的函数
  import { createRouter, createWebHashHistory } from 'vue-router'
  // vue3 中创建路由时，一定要指定路由使用的模式
  const router = createRouter({// 创建路由对象
    // 设置路由对象的模式：
    history: createWebHashHistory(), // hash 模块：createWebHashHistory // history 模式：createWebHistory
    // 配置路由规则
    routes: [
      { path: '/home', component: () => import('../views/Home.vue') },
      { path: '/login', component: () => import('../views/Login.vue') }
    ]
  })
  // 暴露路由对象
  export default router
  ```
* 导入vue实例：`main.js`​

  ```js
  import { createApp } from 'vue'
  import App from './App.vue'
  // 导入路由文件
  import router from './router'

  createApp(App).use(router).mount('#app')
  ```

### useRouter

* ​`useRouter()`​：**返回响应式**​**​`router`​**​**对象**
* ​`router`​ 类型：部分

  ```js
  currentRoute : Ref<RouteLocationNormalizedLoaded>

  addRoute(parentName, route): () => void //添加路由
  removeRoute(name): void  //删除路由

  push(to): Promise<undefined | void | NavigationFailure>  //路由跳转
  replace(to): Promise<undefined | void | NavigationFailure>  //路由跳转并刷新记录
  forward(): void  //路由前进
  back(): void  //路由返回
  go(delta): void  //前进或后退 delta:number


  beforeEach(guard): () => void  //路由守卫
  afterEach(guard): () => void  //路由守卫
  beforeResolve(guard): () => void  //路由守卫
  ```
* ​`RouteLocationNormalizedLoaded`​：路由配置类型

  ```js
  RouteLocationNormalizedLoaded:{
     name: undefined | null | RouteRecordName;
     path: string;
     fullPath: string;
     params: RouteParams;
     query: LocationQuery;
     meta: RouteMeta;
     hash: string;
     matched: RouteRecordNormalized[];
  }
  ```

### useRoute

* ​`useRoute()`​：返回的路由组件实例对象，里面有当前路由所携带的信息，可通过侦听部分属性`watch(()=>route.params.id,()=>{})`​
* ​`route`​类型：最好还是看文档，小小脑子完全记不住😭

  ```js
  name: undefined | null | RouteRecordName//匹配的路由名称。
  path: string//经过百分号编码的 URL 中的 pathname 段。
  fullPath: string //包括 search 和 hash 在内的完整地址。该字符串是经过百分号编码的。
  query: LocationQuery//代表当前地址的 search 属性的对象
  params: RouteParams//从 path 中提取出来并解码后的参数对象。
  meta: object// 路由元信息
  hash: string//当前地址的 hash。如果存在则以 # 开头。
  matched: RouteLocationMatched[]//数组，路由记录
  redirectedFrom: undefined | RouteLocation//包含在重定向到当前地址之前，我们最初想访问的地址。
  ```

### 动态路由

* 动态添加路由到`routes`​中，实现动态路由，这两个api都只是对路由进行注册，如果需要对新增的路由进行匹配，要重新  `router.push() `​或` router.replace() `​来手动导航，才能显示新路由

  * ​`router.addRoute()`​  //返回一个函数，执行该函数，添加的路由会被删除
  * ​`router.removeRoute()`​

  ```js
  这两个api都只是对路由进行注册，
  如果需要对新增的路由进行匹配，要重新  router.push() 或 router.replace() 来手动导航，才能显示新路由

  router.addRoute({ path: '/about', component: About })
  router.replace(router.currentRoute.value.fullPath)

  router.addRoute('admin', { path: 'settings', component: AdminSettings })//添加嵌套路由
  等效于
  router.addRoute({
    name: 'admin',
    path: '/admin',
    component: Admin,
    children: [{ path: 'settings', component: AdminSettings }],
  })
  ```
* 路由守卫中添加动态路由

  ```js
  router.beforeEach(to => {
    if (!hasNecessaryRoute(to)) {
      router.addRoute(generateRoute(to))
      // 触发重定向
      return to.fullPath
    }
  })
  ```
* 删除路由

  ```js
  1.通过添加一个名称冲突的路由。如果添加与现有途径名称相同的途径，会先删除路由，再添加路由：
  router.addRoute({ path: '/about', name: 'about', component: About })
  // 这将会删除之前已经添加的路由，因为他们具有相同的名字且名字必须是唯一的
  router.addRoute({ path: '/other', name: 'about', component: Other })

  2. router.addRoute() 返回的回调：
  const removeRoute = router.addRoute(routeRecord)
  removeRoute() // 删除路由如果存在的话

  3.通过使用 router.removeRoute() 按名称删除路由：
  router.addRoute({ path: '/about', name: 'about', component: About })
  // 删除路由
  router.removeRoute('about')
  ```

### 路由传参数

* 路由参数`Router Params`​ ：可以通过在路由路径中定义动态参数来传递参数

  ```js
  // 路由配置
  {
    path: '/user/:id',
    component: UserComponent
  }

  // 组件中获取参数
  this.$route.params.id
  ```
* 查询参数 `Query Params`​：通过在url中字符串拼接的方法

  ```js
  // 路由配置
  {
    path: '/user',
    component: UserComponent
  }

  // URL：/user?id=123
  // 组件中获取参数
  this.$route.query.id

  ```
* 编程式导航传参

  ```js
  // 路由参数
  router.push({path: '/user/123'})
  // 获取参数
  console.log(this.$route.params.id);

  // 查询参数
  router.push({path: '/user', query: { id: 123 } })
  // 获取参数
  console.log(this.$route.query.id);

  ```

### 动态路由匹配

* 概念：多个路径都能匹配到一个组件，改路径上的某些字段其实是`params`​的存在

  ```js
  const User = {
    template: '<div>User</div>',
  }
  // 这些都会传递给 `createRouter`
  const routes = [
    //动态字段以冒号开始， id被称为路径参数
    //路径参数 用冒号 : 表示。
    //当一个路由被匹配时，它的 params 的值将在每个组件中以 this.$route.params 的形式暴露出来。
    { path: '/users/:id', component: User },
  ]
  ```
* 参数的自定义：可利用正则，或限制参数格式 `* `​（0或更多） `+`​（至少一个） `?`​可选参数

  ```js
  const routes = [ //routes 数组的顺序并不重要
    // /:orderId -> 仅匹配数字
    { path: '/:orderId(\\d+)' },  
    // /:productName -> 匹配其他任何内容
    { path: '/:productName' },
    // /:chapters ->  匹配 /one, /one/two, /one/two/three, 等
    { path: '/:chapters+' },
    // /:chapters -> 匹配 /, /one, /one/two, /one/two/three, 等
    { path: '/:chapters*' },

    正则匹配
    // 可以使用正则，仅匹配数字
    // 匹配 /1, /1/2, 等
    { path: '/:chapters(\\d+)+' },
    // 匹配 /, /1, /1/2, 等
    { path: '/:chapters(\\d+)*' },
    
    可选参数
    // 匹配 /users 和 /users/posva
    { path: '/users/:userId?' },
    // 匹配 /users 和 /users/42
    { path: '/users/:userId(\\d+)?' },
  ]
  ```
* 使用方法设置参数：

  ​`chapter:[]`​，表示对chapter这个路由路径添加路径参数

  ```js
  // 给定 { path: '/:chapters*', name: 'chapters' },
  router.resolve({ name: 'chapters', params: { chapters: [] } }).href
  // 产生 /
  router.resolve({ name: 'chapters', params: { chapters: ['a', 'b'] } }).href
  // 产生 /a/b

  // 给定 { path: '/:chapters+', name: 'chapters' },
  router.resolve({ name: 'chapters', params: { chapters: [] } }).href
  // 抛出错误，因为 `chapters` 为空
  ```

### 路由嵌套

* 概念：`<router-view>`​代表路由出口，组件内可以再设置 `<router-view>`​ 来进行嵌套处理

  ```js
  顶层的路由出口，vue是控制 #id div的
  <div id="app">
    <router-view></router-view>
  </div>

  const User = {
    template: '<div>User {{ $route.params.id }}</div>',
  }
  // 这些都会传递给 `createRouter`
  const routes = [{ path: '/user/:id', component: User }]
  ```
* 路由嵌套：再组件内再放置一个 `<router-view>`​ 路由出口，并在`router`​中设置改组件路径下的 `children`​ 属性

  ```js
  const User = {
    template: `
      <div class="user">
        <h2>User {{ $route.params.id }}</h2>
        <router-view></router-view>
      </div>
    `,
  }

  const routes = [
    {
      path: '/user/:id',
      component: User,
      children: [
        // UserHome 将被渲染到 User 的 <router-view> 内部
        { path: '', component: UserHome },//配置一个空路径，不管什么用都能匹配到
        // 当 /user/:id/profile 匹配成功
        // UserProfile 将被渲染到 User 的 <router-view> 内部
        {path: 'profile', component: UserProfile,},
        // 当 /user/:id/posts 匹配成功
        // UserPosts 将被渲染到 User 的 <router-view> 内部
        { path: 'posts',component: UserPosts,},
      ],
    },
  ]
  ```

### 路由跳转

* 声明式路由跳转：类似于 `a标签`​，再页面上声明好了路由需要跳转的路径，参数可以是字符串，也可以是对象。和router.push()参数一致

  ```js
  <router-link :to="/user"> //跳转到 /user
  <router-link :to="{ name: 'user', params: { username: 'erina' }}">User</router-link>
  ```
* 编程式跳转：js中使用 `router`​实例的方法，来进行路由跳转。
* ​`router.push()`​：参数可以是对象，也可以是字符串

  ```js
  router.push('/users/eduardo') // 字符串路径
  router.push({ path: '/users/eduardo' }) // 带有路径的对象
  router.push({ name: 'user', params: { username: 'eduardo' } }) // 命名的路由，并加上参数，让路由建立 url
  router.push({ path: '/register', query: { plan: 'private' } }) // 带查询参数，结果是 /register?plan=private
  router.push({ path: '/about', hash: '#team' })// 带 hash，结果是 /about#team

  path、name 都可以找到路径路径
  有 path 的情况下，query 会被忽略，query 本质是拼接在路由的路径上，path 已经是代表路径了
  router.push({ path: `/user/${username}` }) // -> /user/eduardo
  // 如果可能的话，使用 `name` 和 `params` 从自动 URL 编码中获益
  router.push({ name: 'user', params: { username } }) // -> /user/eduardo
  // `params` 不能与 `path` 一起使用
  router.push({ path: '/user', params: { username } }) // -> /user

  replace:boolean。是否替换掉路径
  router.push({ path: '/home', replace: true })
  // 相当于
  router.replace({ path: '/home' })
  ```
* ​`router.go()`​：参数是数字，类似于`window.history.go(n)`​。

  ```js
  router.go(1)// 向前移动一条记录，与 router.forward() 相同
  router.go(-1)// 返回一条记录，与 router.back() 相同
  router.go(3)// 前进 3 条记录
  router.go(-100)// 如果没有那么多记录，静默失败
  router.go(100)// 如果没有那么多记录，静默失败
  ```

### 命名视图

* 概念：给` <router-view name="componentName"><router-view> `​ 添加`name`​属性，匹配`routes`​中的`components`​的组件名称

  ```js
  <router-view class="view left-sidebar" name="LeftSidebar"></router-view>
  <router-view class="view main-content"></router-view> //默认name是default
  <router-view class="view right-sidebar" name="RightSidebar"></router-view>

  const router = createRouter({
    history: createWebHashHistory(),
    routes: [
      {
        path: '/',
        components: {
          default: Home,
          LeftSidebar, // LeftSidebar: LeftSidebar 的缩写
          RightSidebar, // 它们与 `<router-view>` 上的 `name` 属性匹配
        },
      },
    ],
  })
  ```

### 路由重定向

‍

1. **[导航守卫](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html)**​**并没有应用在跳转路由上，而仅仅应用在其目标上**。在上面的例子中，在 `/home`​ 路由中添加 `beforeEnter`​ 守卫不会有任何效果。
2. 在写 `redirect`​ 的时候，可以省略 `component`​ 配置，因为它从来没有被直接访问过，所以没有组件要渲染。唯一的例外是[嵌套路由](https://router.vuejs.org/zh/guide/essentials/nested-routes.html)：如果一个路由记录有 `children`​ 和 `redirect`​ 属性，它也应该有 `component`​ 属性。

* 概念：将路由重定向到其他路径，访问 `/home`​ 时，URL 会被 `/`​ 替换，然后匹配成 `/`​

  ```js
  //这三种写法是一样的
  const routes = [{ path: '/home', redirect: '/' }]
  const routes = [{ path: '/home', redirect: { name: 'homepage' } }]
  const routes = [
    {
      path: '/search/:searchText',
      redirect: to => {
        //  方法接收目标路由作为参数,return 重定向的字符串路径/路径对象
        return { path: '/search', query: { q: to.params.searchText } }
      },
    },
  ]
  ```
* 相对重定向

  ```js
  const routes = [
    {
      // 将总是把/users/123/posts重定向到/users/123/profile。
      path: '/users/:id/posts',
      redirect: to => {
        // 该函数接收目标路由作为参数
        // 相对位置不以`/`开头
        // 或 { path: 'profile'}
        return 'profile'
      },
    },
  ]
  ```
* 注意

  ```
  在写 redirect 的时候，可以省略 component 配置，因为它从来没有被直接访问过，所以没有组件要渲染。
  唯一的例外是嵌套路由：如果一个路由记录有 children 和 redirect 属性，它也应该有 component 属性
  ```

### 路径别名

* 概念：**访问 **​ **​`/home`​**​ ** 时，URL 仍然是 **​ **​`/home`​**​ **，但会被匹配为用户正在访问 **​ **​`/`​** ​ **。**

  ```js
  const routes = [{ path: '/', component: Homepage, alias: '/home' }]

  const routes = [
    {
      path: '/users',
      component: UsersLayout,
      children: [
        // 为这 3 个 URL 呈现 UserList  /users  /users/list  /people
        { path: '', component: UserList, alias: ['/people', 'list'] },
      ],
    },
  ]
  ```

### 路由模式

* Hash 模式：`history: createWebHashHistory()`​

  它在内部传递的实际 URL 之前使用了一个哈希字符（`#`​）。由于这部分 URL 从未被发送到服务器，所以它不需要在服务器层面上进行任何特殊处理。不过，**它在 SEO 中有不好的影响**。

  ```js
  import { createRouter, createWebHashHistory } from 'vue-router'
  const router = createRouter({
    history: createWebHashHistory(),
    routes: [],
  })
  ```
* HTML5 模式： `createWebHistory()`​

  当使用这种历史模式时，路径就会看起来正常，如： `https://example.com/user/id`​

  应用是一个单页的客户端应用，如果没有适当的服务器配置，用户在浏览器中直接访问 `https://example.com/user/id`​，就会得到一个 404 错误

  要解决这个问题，你需要做的就是在你的服务器上添加一个简单的回退路由。如果 URL 不匹配任何静态资源，它应提供与你的应用程序中的 `index.html`​ 相同的页面

  ```js
  import { createRouter, createWebHistory } from 'vue-router'
  const router = createRouter({
    history: createWebHistory(),
    routes: [],
  })
  ```

### 路由守卫

* 路由解析过程

  ```js
  1.导航被触发。
  2.在失活的组件里调用 beforeRouteLeave 守卫。
  3.调用全局的 beforeEach 守卫。
  4.在重用的组件里调用 beforeRouteUpdate 守卫(2.2+)。
  5.在路由配置里调用 beforeEnter。
  6.解析异步路由组件。
  7.在被激活的组件里调用 beforeRouteEnter。
  8.调用全局的 beforeResolve 守卫(2.5+)。
  9.导航被确认。
  10.调用全局的 afterEach 钩子。
  11.触发 DOM 更新。
  12.调用 beforeRouteEnter 守卫中传给 next 的回调函数，创建好的组件实例会作为回调函数的参数传入。
  ```

* 全局守卫：

  * ​`router.beforeEach()`​全局前置守卫
  * ​`router.beforeResolve()`​全局解析守卫，导航被确认之前、**所有组件内守卫和异步路由组件被解析之后**调用
  * ​`router.afterEach()`​全局后置守卫

  ```js
  1.router.beforeEach()
  const router = createRouter({ ... })
  router.beforeEach((to, from,next) => {}) //返回值是false则停止路由跳转，
  to: 即将要进入的目标
  from: 当前导航正要离开的路由
  next:兼容vue2的写法

  2.router.beforeResolve()
  router.beforeResolve 是获取数据或执行任何其他操作（如果用户无法进入页面时你希望避免执行的操作）的理想位置。
  还没进入路由，但是路由组件这些东西都已经准备好了情况下调用这个钩子
  router.beforeResolve(async to => {
    if (to.meta.requiresCamera) {
      try {
        await askForCameraPermission()
      } catch (error) {
        if (error instanceof NotAllowedError) {
          // ... 处理错误，然后取消导航
          return false
        } else {
          // 意料之外的错误，取消导航并把错误传给全局处理器
          throw error
        }
      }
    }
  })

  3.router.afterEach()
  router.afterEach((to, from) => {
    sendToAnalytics(to.fullPath)
  })
  ```
* 路由独享守卫：直接在路由配置上定义 `beforeEnter()`​​ 守卫，**只在进入路由时触发**，不会在 `params`​​、`query`​​ 或 `hash`​​ 改变时触发。

  ```js
  1. /users/2 进入到 /users/3 或者从 /users/2#info 进入到 /users/2#projects 不会触发
  2. 只有在 从一个不同的 路由导航时，才会被触发
  const routes = [
    {
      path: '/users/:id',
      component: UserDetails,
      beforeEnter: (to, from) => {
        // reject the navigation
        return false
      },
    },
  ]
  ```
* 组件内守卫：

  * ​`beforeRouteEnter(to, from,next)`​ ，只有改钩子可以通过 `next`​拿到组件实例，另外两个不行，因为没有必要了
  * ​`beforeRouteUpdate(to, from))`​：路由复用时触发
  * ​`beforeRouteLeave(to, from))`​​ ：通常用来预防用户在还未保存修改前突然离开。该导航可以通过返回 `false`​​ 来取消。

  ```js
  vue2中可以直接写，使用vue3的setup函数时候，需要导入钩子，同时前面加on
  import { onBeforeRouteLeave, onBeforeRouteUpdate } from 'vue-router'

  const UserDetails = {
    template: `...`,
    beforeRouteEnter(to, from,next) {
      // 在渲染该组件的对应路由被验证前调用
      // 不能获取组件实例 `this` ！
      // 因为当守卫执行时，组件实例还没被创建！
      // 可以通过传一个回调给 next 来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数
      next(vm => {通过 `vm` 访问组件实例})
    },
    beforeRouteUpdate(to, from) {
      // 在当前路由改变，但是该组件被复用时调用
      // 举例来说，对于一个带有动态参数的路径 `/users/:id`，在 `/users/1` 和 `/users/2` 之间跳转的时候，
      // 由于会渲染同样的 `UserDetails` 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
      // 因为在这种情况发生的时候，组件已经挂载好了，导航守卫可以访问组件实例 `this`
    },
    beforeRouteLeave(to, from) {
      // 在导航离开渲染该组件的对应路由时调用
      // 与 `beforeRouteUpdate` 一样，它可以访问组件实例 `this`
    },
  }
  ```

### 路由滚动

* ​`scrollBehavior()`​控制页面切换时，页面的滚动位置

  ```js
  to、form是路由对象
  savedPosition，只有当这是一个 popstate 导航时才可用（由浏览器的后退/前进按钮触发）
  scrollBehavior:返回一个位置对象
  { top: 0 }//始终在顶部
  false、{}//不会发生滚动
  {el: to.hash,}//类似于锚点行为
  {el: to.hash,behavior: 'smooth',}//behavior 浏览器的滚动行为
  可以使用定时器或者promise来实现延时效果
  const router = createRouter({
    scrollBehavior(to, from, savedPosition) {
      // 始终在元素 #main 上方滚动 10px
      return {
        // el: document.getElementById('main'),
        el: '#main',
        top: -10,
      }

    
    },
  })
  ```

### 懒加载

```js
const UserDetails = () => import('./views/UserDetails.vue')
const router = createRouter({
  routes: [{ path: '/users/:id', component: UserDetails }],
})

component (和 components) 配置接收一个返回 Promise 组件的函数
Vue Router 只会在第一次进入页面时才会获取这个函数，然后使用缓存数据。这意味着你也可以使用更复杂的函数，只要它们返回一个 Promise
```

# pinia

### 基本使用

* 下载

  ```js
  npm i pinia
  ```
* 挂载：创建实例并挂载

  ```js
  import { createApp } from 'vue'
  import App from './App.vue'
  // 导入 createPinia函数： 创建 pinia 对象
  import { createPinia } from 'pinia'
  // 创建一个 pinia 对象
  const pinia = createPinia()
  createApp(App).use(pinia).mount('#app')
  ```
* 创建文件：`src/store/couter.js`​

  ```js
  // 导入 defineStore 函数：创建 pinia 的实例
  import { defineStore } from 'pinia'
  //创建一个 store 函数 defineStore('store 实例的名称', { // store 实例的属性})
  const useCouterStore = defineStore('couter', {
    state: () => {
      return {
        // 定义一个状态：
        count: 110,
        name: '张三'
      }
    },
    getters: {},
    actions: {}
  })
  // 暴露 
  export default useCouterStore
  ```

### store定义

* 选项式：把`state`​、`getter`​、`actions`​都定义好

  ```js
  import { defineStore } from "pinia"
  const useStore = defineStore('mian',{
    state:()=>({
      ......
    })
    getter:{}
    actions:{}
  })
  ```
* 组合式：第一个参数传递入函数，就和 `setup`​一样定义，

  * ​`ref()`​ 就是 `state`​ 属性
  * ​`computed()`​ 就是 `getters`​
  * ​`function()`​ 就是 `actions`​

  ```js
  import { defineStore } from "pinia"
  import { ref,computed } from "vue"
  const useStore = defineStore('mian',()=>{
    1.ref 是 state
    const name = ref('name')
    2.computed() 就是 getters
    const myName = computed(()=>{this.name.value})
    3.function 是 actions
    const setName = (newName)=>{
      this.name.value = newName
    }
    return {
      name,
      myName ,
      setName 
    }
  })
  ```

### 模块化

核心思路就是在`store/index.js`​中导入其他的`store`​文件，然后暴露返回他们的实例化函数

* 暴露实例化函数

  ```js
  import useCountStore from "src/store/info.js"
  import useInfoStore from "src/store/info.js"
  export default function(){
    return {
      info:useInfoStore()
      count:useCountStore()
    }
  }
  ```
* 使用

  ```js
  import useStore from './store/index'
  let { info } = useStore()  // let info = useInfoStroe()
  ```

### store

* 定义：`src/stroe/counter.js`​

  ```js
  defineStore(storeName,options)//创建store实例的时候，必须传入storeName,storeName是唯一的

  import { defineStore } from 'pinia'
  export default const useCounterStore = defineStore('main', {
    state:()=>({}),
    actions:{},
    getters:{}
  })
  ```
* 使用：页面中创建`store`​实例

  ```js
  import { useCounterStore } from '@/stores/counter'
  const counter = useCounterStore()//获得了这个store实例，利用点方法，使用里面的方法和属性

  //store是一个用reactive包裹的对象，不需要进行.value
  counter.name//属性
  counter.double//计算属性
  counter.fn()//方法
  ```
* ​`storeToRefs`​：解构`store`​对象，像`setup`​ 中的`props`​ 一样，**不能对其进行解构 （直接解构不是响应式）**

  ```js
  //storeToRefs(store实例)
  //可以解构属性和计算属性，不会解构actions和其他非响应式数据。直接解构会失去响应式
  import { useCounterStore } from '@/stores/counter'
  import { storeToRefs } from 'pinia'//它将为任何响应式属性创建 refs
  const counter = useCounterStore()
  const {name,double} = storeToRefs(counter)//解构
  ```

### state

* 定义

  ```js
  import { defineStore } from 'pinia'
  const useStore = defineStore('storeId', {
    // 推荐使用 完整类型推断的箭头函数
    state: () => {
      return {
        counter: 0,// 所有这些属性都将自动推断其类型
        name: 'Eduardo',
        isAdmin: true,
      }
    },

    //或者,少写个return
    state: () => ({
      counter: 0,
      name: 'Eduardo',
      isAdmin: true,
    })
  })
  ```
* 使用

  ```js
  import useCounterStore from "@/store/counter.js"
  const counter = useCounterStore()
  counter.xxx
  counter.xxx++//可以直接修改

  //多部分修改直接修改
  counter.$path({
    xxx:counter.xxx++
    name:'zhansan'
  })
  //多部分使用回调函数，参数是store实例
  counter.$path((store)=>{
    state.items.push({ name: 'shoes', quantity: 1 }) //方便使用数组方法
  })

  ```

### getter

* 概念：`pinia`​中的计算属性，需要传参的话，返回一个函数既可
* 定义：和`state`​一样

  ```js
  import { defineStore } from 'pinia'
  const useStore = defineStore('storeId', {
    getter: {
      doubleCount:(state)=>{
        return state.counter * 2
      },
      //不传参，在ts中，使用this获得store内的值，需要明确返回值的类型
      doubleCount():number{
        return this.counter * 2//可以通过this访问
      }
      //利用返回值里面写一个箭头函数，箭头函数为输入的参数来达到计算属性传参的效果
      fn(state): number {
        return (num)=> {
          return  num * this.counter
        }
      }
    }
  })
  ```
* 使用：创建`store`​实例，然后点语法使用

  ```js
  import useCounterStore from "@/store/counter.js"
  const counter = useCounterStore()
  <template>
    <div>{{counter.doubleCount}}</div>
  </template>
  ```

### actions

* 概念：可以异步和同步，用来操作`pinia`​内的state数据
* 定义：和`methods`​​一样，可以自定义传参

  ```js
  import { defineStore } from 'pinia'
  const useStore = defineStore('storeId', {
    state:()=>({
      number:10
    })
    actions:{
      async getUserInfo(login, password) {
       this.number...
      },
    }
  })
  ```
* 使用：创建`store`​实例，然后使用点语法，也可以解构使用（反正方法不用是响应式）

  ```js
  import useCounterStore from "@/store/counter.js"
  const counter = useCounterStore()
  fn(){
    counter.getUserInfo()
  }
  //解构写法
  let {getUserInfo} = counter
  ```

### ​store.$reset​

​`store.$reset()`​​​：重置状态到初始值

```js
const store = useStore()
store.$reset()
```

### ​store.$patch​

​`store.$patch(Object|Function)`​：批量变更

```js
1.同时修改多个值
store.$patch({name:'李四',age:20})

2.方便使用函数方法
store.$patch((state)=>{
  state.list.push()
  ...
})
```

### ​store.$subscribe​

​`store.$subscribe((mutation，state)=>{}, { detached: boolean})`​：订阅state变化

```js
就是store的侦听器，在状态(state,getters)有改变后触发，一般用于数据保存
返回值是一个停止订阅的函数
store.$subscribe(callback, { detached: true })//第二个参数，代表组件销毁，这个订阅还在
store.$subscribe((mutation,store), { detached: true })//第二个参数，代表组件销毁，这个订阅还在

mutation.type // 'direct' | 'patch object' | 'patch function' ,触发本次更新的类型，是直接修改，还是使用 store.$path(obj) \ store.$path(callback)
mutation.storeId // storeId  的id
mutation.payload // 只有 mutation.type === 'patch object'的情况下才可用，传递给 cartStore.$patch() 的补丁对象。
```

### storeToRefs

​`storeToRefs(store)`​：解构

```js
如果直接从pinia中解构数据，会丢失响应式，storeToRefs可以保证解构出来的数据也是响应式的
只能解构属性，不能解构方法
import useCountStore from './store/count'
import { storeToRefs } from 'pinia'
const count = useCountStore()
const {name,age} = storeToRefs(count)//这样解构的数据，才能保持响应式
```

### 持久化

* 插件名称：`pinia-plugin-persistedstate`​
* 安装：​`yarn add pinia-plugin-persistedstate`​
* 引入

  ```js
  import { createApp } from 'vue'
  import { createPinia } from 'pinia'
  import piniaPluginPersistedstate  from 'pinia-plugin-persistedstate'
  import App from './App.vue'

  const app = createApp(App)
  const pinia = createPinia()
  pinia.use(piniaPluginPersistedstate)//pinia使用插件
  app.use(pinia)
  app.mount('#app')
  ```
* 使用：`defineStore`​第三个参数

  ```js
  export const useStore = defineStore("YourStore", () => {}, {
      persist: {
          enabled: true,
          strategies: [{
              // 自定义存储的 key，默认是 store.$id
              key: "custom storageKey",
              // 可以指定任何 extends Storage 的实例，默认是 sessionStorage
              storage: localStorage,
              // state 中的字段名，按组打包储存
              paths: ["foo", "bar"]
          }],
      }
  })

  ```

# vuex

### 基本使用

* 下载安装

  ```bash
  # 目前这是给vue3使用
  npm i vuex

  # 这是给vue2使用
  npm i vuex@3.x
  ```
* 导入`vuex`​：在项目中的`main.js`​文件中配置`Vuex`​

  ```js
  //导入Vuex
  import Vuex from 'vuex'
  // 安装vuex
  Vue.use(Vuex)
  // 实例化一个store对象（vuex对象）
  const store = new Vuex.Store()

  // 挂载到vue实例上
  new Vue({
      store
  }).$mount('#app')
  ```
* 优化：抽取到独立的js文件，把vuex的代码放到`src/store/index.js`里，然后再在`main.js`中进行导入使用

  ```js
  //创建  src/store/index.js  文件。进行如下配置，再导入到main.js中
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)

  export default new Vuex.Store({
    state: {},
    getters: {},
    mutations: {},
    actions: {},
    modules: {}
  })
  ```
* 快速配置：创建好vue项目后，在终端输入以下命令

  ```js
  vue add vuex
  ```

### state

* 概念：`state`​就是`vuex`​里保存数据的地方，要想某个数据在所有组件都可以访问，就放到`state`​里。**用点语法访问数据**
* 初始化数据

  ```js
  // 初始化vuex对象
  const store = new Vuex.Store({
    state: {
      // 管理数据
      count: 0
    }
  })
  ```
* 访问数据

  ```js
  //没有modules
  $store.state.数据名
  $store.state.count

  //有modules并开启了命名空间
  $store.state.模块名.数据名
  $store.state.userInfo.name
  ```
* 注意：数据的修改要用`mutations`​中的方法，直接修改，如果开了严格模式会报错，其次调试工具无法正确的实时追踪到改变，不利于调试

  ```js
  严格来说不能这样该，开启严格模式会报错
  $store.state.count = newValue
  ```

### getters

* 概念：`getters`​是`vuex`​中的计算属性，依赖一个或者多个值(`state`​)产生一个新的值，有缓存。**用点语法**
* 设置方法

  ```js
  getters: {
    getNum (state) {//可以访问state
      return newValue
    }
  }
  //拓展使用
  getters:{
    方法名(state,getters,rootState,rootGetters){
      state:当前模块的state
      getters:当前模块的getters
      rootState:根模块的state,它可以获取所有模块的state
           rootState===this.$store.state
      rootGetters:根模块的getters,它可以获取所有模块的getters
           rootGetters===this.$store.getters
    return  //这边就可以使用别的模块的数据
    },
  }
  ```
* 使用方法

  ```js
  //没有modules
  $store.getters.方法名
  //有modules并开启了命名空间
  $store.getters.模块名.方法名
  ```

### mutations

* 概念：`state`​数据的修改**只能**通过`mutations`​，并且`mutations`​必须是同步更新，目的是形成**数据快照。** 有一个命名规定，**名字全大写。**
* 设置方法：和`state`​平级

  ```js
  state: {
    ...
  },
  mutations: {
    // 参数1:固定参数叫state,获取的就是当前store对象里的state
    // 参数2:调用方法时,方法传进来的参数，只能传入一个，所以多个参数使用对象或数组传入，否则多余参数变nudefined
    方法名(state,newValue) {
      state.count=newValue
      ...
    }
  }
  ```
* 方法调用

  ```js
  //没有modules
  $store.commit('方法名',newValue)//只能传入一个，多个数值使用对象或数组
  //有modules并开启了命名空间
  $store.commit('模块名/方法名',newValue)
  ```

### actions

* 概念：使用异步时候调用`actions`​里面的方法，通过调用`mutations`​里面的方法来实现间接修改数据，不能直接修改方法
* 设置方法

  ```js
  actions: {
    // 参数1:context或者简写ctx，它是当前的整个store对象,里面就有state、commit、dispatch等
    // 参数2:和mutations中的是一样的
    方法名 (ctx, 额外的参数) {
      ctx.commit('方法名',xxx)
      ...
    }
  }

  //ctx里面有什么
  actions:{
      方法名(ctx,value){
        ctx:{
          state:当前模块的state,
          getters:当前模块的getters,
          rootState:根模块的state,
          rootGetters:根模块的getters,
          commit:调用mutations方法,
                调用当前模块mutations方法：  commit('方法名'，实参)
                调用其它模块的mutations方法：commit('模块名/方法名',实参,{root:true})
          dispatch:调用actions方法
                调用当前模块actions方法：  dispatch('方法名'，实参)
                调用其它模块的actions方法：dispatch('模块名/方法名',实参,{root:true})  
        }
      }
  }

  ```
* 调用方法

  ```js
  //没有modules
  $store.dispatch('方法名', 额外参数)
  //有modules并开启了命名空间
  $store.dispatch('模块名/方法名',额外参数)
  ```

### [modules](https://juejin.cn/post/7126591741829840932)​

* 概念：由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。也就是说如果把所有的状态都放在state中，当项目变得越来越大的时候，Vuex会变得越来越难以维护。所以Vuex有他的模块化
* 封装模块：在`store`文件夹中新建一个`modules`文件夹，新建`xxx文件夹/index.js`​，用来保存模块化数据

  ```js
  export default {
    namespaced: true,//命名空间,不开启命名空间，里面的数据、方法还是可以通过全局调用的,不开启命名空间，命名重复调用会报错
    state: {}，
    mutations:{},
    actions:{},
    getter:{}
  }
  ```
* 导入模块：导入到`store/index.js`

  ```js
  import user from './modules/user'//导入模块化的store,存储专门的数据
  import xxx from './modules/xxx'//导入模块化的store,存储专门的数据
  //使用模块
  export default new Vuex.Store({
    modules: {
      user,
      xxx,
    }
  })
  ```
* 调用模块方法或数据

  ```js
  namespaced: true, // 默认是false,不开启则是全局调用
  state访问：this.$store.state.模块名.属性名
  getters访问：this.$store.getters['模块名/方法名']//计算属性不能再用点语法
  mutations访问：this.$store.commit('模块名/方法名',实参)
  actions访问:this.$store.dispatch('模块名/方法名',实参)
  ```

### map辅助函数

* 概念：方便调用`modules`​里面的数据和方法
* 正常使用`vuex`​模块化方法或数值

  ```js
  state:  
  this.$store.state.模块名.属性名

  getters:
  this.$store.getters['模块名/方法名']

  mutations:
  this.$store.commit('模块名/方法名',实参)

  actions:
  this.$store.dispatch('模块名/方法名',实参)

  ```
* 使用辅助函数

  * 导入辅助函数：在需要使用的页面导入

    ```js
    import {mapState,mapMutations,mapActions,mapGetters} from 'vuex'
    ```
  * 使用辅助函数：计算属性和数据使用在`computed`中，方法使用在`methods`​中

    ```js
    computed:{
      ...mapState('模块名',['属性名']),
      ...mapGetters('模块名',['方法名'])
    },
    methods:{
      ...mapMutations('模块名',['方法名']),
      ...mapActions('模块名',['方法名'])
    }

    //修改属性或方法名
    (模块名，[方法名] => (模块名，{新方法名：原方法名})

    改map的方法或者属性名
    ['属性名/方法名'] => {新的名字:'属性名/方法名'}
    ```
  * 使用模块化方法或数据

    ```js
    state:  
    this.属性名

    getters:
    this.方法名

    mutations:
    this.方法名(实参)

    actions:
    this.方法(实参)
    ```
  * 使用案例

    ```js
    import { mapState, mapMutations, mapActions, mapGetters } from 'vuex'
    export default {
      computed: {
        // ...{
        //   userName () {
        //     return this.$store.state.user.userName
        //   },
        //   age () {
        //     return this.$store.state.user.age
        //   }
        // }
        ...mapState('user', ['userName', 'age']),
        ...mapGetters('user', ['getUserName'])
      },
      methods: {
        ...mapMutations('user', ['SETUSERNAME']),
        // ...mapActions('user', ['setUserNameAsync']),
        ...mapActions('user', { setName: 'setUserNameAsync' }),//改名字
        setUserNameAsync () {},
        btnClick () {
          // this.$store.commit('user/SETUSERNAME', '李四同学')
          this.SETUSERNAME('李四同学')

          // console.log(mapState('user', ['userName', 'age']))
        },
        btnClick2 () {
          // this.$store.dispatch('user/setUserNameAsync', '李四同学')
          this.setName('李四同学')
        }
      }
    }
    </script>
    ```

# vite

### 创建项目

```js
//会自动下载vite的，不需要npm i xxx
npm create vite
yarn create vite

//快速创建项目
npm create vite vite-demo --template vue//创建普通vue项目
npm create vite vite-demo-ts --template vue-ts//创建基于ts模板的项目
```

### 配置代理

```js
// 配置代理
server: {
  proxy: {
    "/api": {
      target: "http://jsonplaceholder.typicode.com",
      changeOrigin: true,
      rewrite: (path) => path.replace(/^\/api/, ""),
    },
  },
},
```

### 环境变量

* 注意：环境变量名必须以 `VITE_APP`​开头

  ```js
  在根目录新建:
  .env.development  //开发环境设置文件
  .env.production  //生产环境设置文件
  .env.test  //测试环境设置文件
  ```
* 开发环境

  ```js
  # 开发环境：.env.development
  NODE_ENV=development
  VITE_APP_ENV=development

  # 服务器地址
  VITE_APP_HOST=开发环境服务器地址

  # 基础路径
  VITE_APP_BASE=""

  # appID相关
  VITE_APP_APPID_WEIXIN=微信appID
  VITE_APP_APPID_ALIPAY=支付宝appID
  ```
* 生产环境

  ```js
  # 生产环境：.env.production 
  NODE_ENV=production
  VITE_APP_ENV=production

  # 服务器地址
  VITE_APP_HOST=生产环境服务器地址

  # 基础路径
  VITE_APP_BASE=""

  # appID相关
  VITE_APP_APPID_WEIXIN=微信appID
  VITE_APP_APPID_ALIPAY=支付宝appID
  ```

‍

‍
