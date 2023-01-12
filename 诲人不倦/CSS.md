---
title: CSS
date: 2022-07-26T19:31:43Z
lastmod: 2022-10-22T09:48:27Z
---

![image](assets/06-20220926112058-q4yr196.jpg)

# CSS

# CSS基础

## Less

* 概念：Less、Sass、Stylus三大css预处理语言
* 变量：`@`​定义变量

  ```less
  //样式变量
  @color:#999
  p{
    color:@color
  }

  //选择器变量,变量名必须用{}包裹
  @{myDiv}{//放在里面的选择器会有下面的属性
    color:blue;
    font-size:16px
  }

  .{id}{}===
  .id{
    color:blue;
    font-size:16px
  }
  ```
* 嵌套：`&`​代表的上一层选择器的名字

  ```less
  div{
    p{}
  }

  div{
    &:after{} //此时的&就是代表上面的div
  }
  ```
* 计算

  ```less
  div{
    width:20*5px
  }
  ```
* 混入：和函数一样，甚至里面可以些参数

  ```less
  .card { // 等价于 .card()
      background: #f6f6f6;
      -webkit-box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
      box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
  }

  #wrap{
    .card;//等价于.card();
  }
  /* 生成的 CSS */
  #wrap{
    background: #f6f6f6;
    -webkit-box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
    box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
  }

  //建议写成
  .card(){
    //something...
  }
  #wrap{
    .card();
  }
  ```
* 继承：同时拥有一样的后者的属性

  ```less
  .animation{
      transition: all .3s ease-out;
      .hide{
        transform:scale(0);
      }
  }
  #main{
      &:extend(.animation);
  }
  #con{
      &:extend(.animation .hide);
  }

  /* 生成后的 CSS */
  .animation,#main{
    transition: all .3s ease-out;
  }
  .animation .hide , #con{
      transform:scale(0);
  }
  ```
* 导入less文件

  ```less
  1.import导入
  import "main"; 
  //等价于
  import "main.less";

  2.@import 导入
  #main{
    font-size:15px;
  }
  @import "style";

  3.使用@import (reference)导入外部文件，但不会添加 把导入的文件 编译到最终输出中，只引用。
  @import (reference) "bootstrap.less"; 
  #wrap:extend(.navbar all){}

  4.@import语句的默认行为。这表明相同的文件只会被导入一次，而随后的导入文件的重复代码都不会解析
  @import (once) "foo.less";
  @import (once) "foo.less"; // this statement will be ignored
  ```

## SCSS

　　​`scss`​和`sass`​其实是一样的`css`​预处理语言，`scss`​是 `sass3 ​`​引入新的语法。后缀名分别为：`.scss`​、`.sass`​。

* 变量

  ```scss
  定义 $color：red
  使用 
  div{
    color：$color;
  }
  ```
* 嵌套

  ```scss
  div{
    p{
      .classname{
        ...
      }
    }
  }
  ```
* 计算

  ```scss
  div{width：3*5px}
  ```
* 代表父级

  ```scss
  .father === &
  .father{
    &-son{
    //子盒子类命名为 <class='father-son'>
    //这样方便看标签关系
    }
  }
  ```
* 继承：变量是$继承的是%

  ```scss
  设置要继承的属性
  %border{
    border:1px solid red
    ...
  }

  使用该继承属性
  .father{
    @extend %border;
  }
  ```
* 混入

  ```scss
  定义：
  @mixin setwh($w,$h){
    width:$w;
    height:$h;
  }

  使用
  .father{
    include @setwh(100px,100px)
  }
  ```

# 样式问题

### 隐藏元素的方式

```js
display: none;//他会在检查的dom里面，但是实际是不渲染的
DOM 结构：浏览器不会渲染 display 属性为 none 的元素，不占据空间；
事件监听：无法进行 DOM 事件监听；
性能：动态改变此属性时会引起重排，性能较差；
继承：不会被子元素继承，毕竟子类也不会被渲染；
transition：transition 不支持 display。

visibility: hidden;
DOM 结构：元素被隐藏，但是会被渲染不会消失，占据空间；
事件监听：无法进行 DOM 事件监听；
性能：动态改变此属性时会引起重绘，性能较高；
继承：会被子元素继承，子元素可以通过设置 visibility: visible; 来取消隐藏；
transition：visibility 会立即显示，隐藏时会延时

opacity: 0;
DOM 结构：透明度为 100%，元素隐藏，占据空间；
事件监听：可以进行 DOM 事件监听；
性能：提升为合成层，不会触发重绘，性能较高；
继承：会被子元素继承,且子元素并不能通过 opacity: 1 来取消隐藏；
transition：opacity 可以延时显示和隐藏
```

### 设置显示省略号

```css
.title{
  display: -webkit-box;
  overflow: hidden;
  text-overflow: ellipsis;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3;//设置几行超过就显示省略号
}

//超过一行就省略号
.title{
  overflow:hidden;
  text-overflow:ellipsis;
  white-space: nowrap;
}
```

### 设置动态滚动条

```css
选择器{
  overflow: auto;
  position: absolute;
  top: 96px;
  bottom: 50px;
}
```

### 设置图片不超过页面宽度

```css
选择器 {
  word-break: break-all;
}
```

### HTML5新标签

　　主要是新增了语义化标签

```html
video    表示一段视频并提供播放的用户界面  
audio    表示音频  
canvas    表示位图区域  
source    为video和audio提供数据源  
track    为video和audio指定字母  
svg    定义矢量图  
code    代码段  
figure    和文档有关的图例  
figcaption    图例的说明  
main  
time    日期和时间值  
mark    高亮的引用文字  
datalist    提供给其他控件的预定义选项  
keygen    秘钥对生成器控件  
output    计算值  
progress    进度条  
menu    菜单  
embed    嵌入的外部资源  
menuitem    用户可点击的菜单项  
menu    菜单  
template  
section  
nav  
aside  
article  
footer  
header
```

### CSS3新特性

1. 属性选择器
2. 结构伪类选择器
3. 伪元素选择器
4. CSS3 2D转换
5. CSS3 动画
6. CSS3 3D转换
7. 浏览器私有前缀

### 二倍图

* 物理像素指的是屏幕显示的最小颗粒，是物理真实存在的。
* 物理像素比或屏幕像素比：一个px能显示的物理像素的个数
* 现在的手机屏幕1px 不一定等于 1个物理像素
* ui图片给的200x200px，实际css样式中写100x100px（实际像素点还是200*200）
* **如果200x200的图，你写200x200，相当于400x400个像素点去渲染200x200的图，**​~~**2个点去渲染一个点，相当于图片放大了，就模糊了**~~

### Vant屏幕适配

　　二倍图是我们是使用图片的时候专门设置的，1的盒子去装大小2的图。设计稿其他还是要1：1

　　rem适配移动端，750px的图，就要设置75.0px=1font-size=1rem

　　`vant`是以375为逻辑像素的，那么设置37.5=1font-size=1rem。**同时，设计稿要缩放为375大小**

* 使用插件

  * postcss-pxtorem：px=>rem
  * flexible：移动端适配

### 移动端适配单位

* rem：10rem=视口宽度=1html根字号
* vw：100vw=视口宽度
* vh：100vh=视口高度

### 弹性布局

```js
{
display:flex;//父盒子开启

设置主轴方向
flex-direction:column;  //将主轴改为y轴，纵轴
flex-direction:row;  //将主轴改为x轴，横轴
flex-direction:row-reverse;  //主轴为x轴，并且翻转
flex-direction:column-reverse;  //主轴为y轴，并且翻转

设置主轴子元素排列形式
justify-content:flex-start; //所有子元素在主轴头部显示
justify-content:flex-end; //所有子元素在主轴尾部显示
justify-content:flex-center; //所有子元素在主轴居中对齐
justify-content:space-around; //所有子元素平分剩余空间
justify-content:space-between; //所有子元素先两边贴边在平分剩余空间

设置侧轴元素对齐的方式，在子项为单项（单行）的时候使用
align-items:flex-start; //表示从头开始
align-items:flex-end; //表示从结尾开始
align-items:center; //表示居中显示
align-items:stretch; //会将子元素拉伸

设置行对齐方式,和justify-content一样,出现换行的时候使用，单行的情况下是无法使用的
align-content:...; 

设置允许换行
flex-warp:wap//可以换行
}

{
//子盒子
flex:1;//设置数字，为比例
justify-self: ;属性设置单个盒子在其布局容器适当轴中的对其方式；
align-self: ;该盒子在侧轴上的对齐方式。
}

```

### 粘性布局

```js
//粘性布局：不是很常用，主要是用在**页面吸顶**就想是固定头部导航栏一样
//适配性较差，有的时候需要加私有化前缀
{
  position: sticky;
  top:0;
  left:0;
  z-index:999//提高
}
```

　　‍
