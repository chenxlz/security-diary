![image](assets/06-20220926112058-q4yr196.jpg)

# 样式问题

### 盒子居中

* ​`transform`​

  ```css
  //设置定位盒子居中：利用子觉父相，和css3中的 transform 2D位移 
  .father{
    width: 800px;
    height: 400px;
    margin: 0 auto;
    position: relative;
  }
  .son{
    width: 200px;
    height: 50px;
    background-color: pink;
    position: absolute;
    /* 走父盒子的50% */
    left: 50%;
    top: 50%;
    /* 走自己盒子的50% */
    transform: translate(-50%,-50%);   
  }
  ```

### 宽度适应内容

```js
//可以让块级盒子的宽度不是占据一行，而是自适应内容
width:fit-content
```

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

```js
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
* **如果200x200的图，你写200x200，相当于400x400个像素点去渲染200x200的图，**​**~~2个点去渲染一个点，相当于图片放大了，就模糊了~~**

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

# CSS

### 引入方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!-- 1.内嵌式：css代码写在style标签中 -->
    <!-- 写在head里面，title下面 -->
    <!-- 影响范围：当前页面 -->
    <style>
        div {
            color: aqua;
        }
    </style>

    <!-- 2.外链式：单独存在的CSS文件，使用link标签便可以导入使用 -->
    <!-- 影响范围：所有页面，只要导入就可以使用 -->
    <link rel="stylesheet" href="./02-外链式css文件.css">
</head>
<body>
    <!-- 行内式 -->
    <div style="color: brown;">啦啦啦</div>
</body>
</html>
```

### 弹性布局

```css
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

### float

浮动原本是用来解决文字环绕问题的，设置浮动会脱离标准流

初始情况，标准流的子盒子，会把标准流的父盒子高度撑开,设置浮动后，子元素不会把父盒子撑开

```css
<div class="father">
  <div class="left"></div>
  <div class="right"></div>
</div>


.left{
  float:left;
}
.right{
  float:right;
}

//清除浮动影响的方法
1.给父盒子设置高度，只适合固定的盒子高度 
2.利用overflow属性，一般不用，本职是处理溢出内容
.father{
  overflow:hidden
}
3.额外标签法:在所有子元素的最后，添加一个额外标签div，给标签一个额外的属性 clear：both
4.单伪元素清除浮动
.clearfix::after {
  content: "";
  display: inline-block;
  clear: both;
}
5.双伪元素清除浮动
.clearfix::before,
.clearfix::after{
  content: "";
  display: block;
}
```

### position

* ​`position:fixed`​：固定定位

  ```css
  1.固定定位的元素，会脱离标准流，不会占据标准流位置
  2.固定定位的元素，不会随着页面滚动，会固定在某个位置
  3.固定定位的元素，位移参考物是浏览器的可视区域

  div{
    position:fixed;//设置标签的定位方法，
    left:10px;
    top:10px
  }
  ```
* ​`position:absolute`​：绝对定位

  ```css
  1.绝对定位会脱离标准流
  2.绝对定位:父元素没有定位,则绝对定位的子元素参照浏览器位移
  3.如果父元素有定位,则绝对定位会参照最近的父元素定位
  div{
    position:absolute;//设置标签的定位方法，
    left:10px;
    top:10px
  }
  ```
* ​`position:relative`​：相对定位

  ```css
  1.相对定位的元素不会脱标
  2.相对定位的位移参照原先位置
  3.一般都是利用相对定位来配合决定定位使用,形成子绝父相

  div{
    position:relative;//设置标签的定位方法，
    left:10px;
    top:10px
  }
  ```
* ​`position:relative`​：粘性定位

  ```css
  //粘性布局：不是很常用，主要是用在 页面吸顶  就像是固定头部导航栏一样
  //适配性较差，有的时候需要加私有化前缀
  {
    position: sticky;
    top:0;
    left:0;
    z-index:999//提高
  }
  ```
* ​`position:static`​：标准定位，文档流

  ```css
  static：标准定位，就是标准流，默认的方式
  ```

### 选择器

* 标签选择器

  ```css
  p{}
  div{}
  ```
* 类选择器

  ```css
  <div class="box"></div>
  .box{}
  ```
* id选择器

  ```css
  <div id="box"></div>
  #box{}
  ```
* 通配符选择器

  ```css
  *{}
  ```
* 后代选择器

  ```css
  .father .son .baby {}
  ```
* 子代选择器

  ```css
  .father>.son>.baby {}
  ```
* 并集选择器：同时控制多个目标标签, 将多个条件用 逗号 连接

  ```css
  div,span,p,a {}//同时作用到这4个标签
  ```
* 交集选择器：多个条件来描述同一个标签, 选择名之间没有任何连接符号

  ```css
  div.box {}
  ```
* ((20240519093736-i4nxnxk '伪类选择器'))

  ```css
  div:hover {} //当鼠标悬停的时候,触发css样式，当鼠标离开元素,则恢复原来的样式
  ```
* ((20240519094604-8v9vjsz '结构伪类选择器'))
* 属性选择器

  ```css
  input[type="text"]{}//input标签中有type="text"属性的标签

  ```

### 伪元素

必须要有`content`​属性

```css
.father::before{
  content: "我在前面";//必备属性
  display: inline-block;//设置成行内块元素
}
.father::after{
  content: "我是后面";//必备属性
  display: block;//设置成块级元素
}
```

### 伪类选择器

* 链接伪类选择器

  ```css
  /* 链接伪类选择器，控制不同状态下的a标签 */
  /* 如果真的有a标签,需要同时设置以上四个伪类选择器,必须按照顺序设置 love (link visited) hate (hover active) */
  /* 访问前 访问后 悬停 拖动 */
  /* 1未被访问的状态（浏览器没有访问记录） */
  a:link{
    color: red;
  }
  /* 2.访问后的状态 */
  a:visited{
    color: aqua;
  }
  /* 3.鼠标悬停的状态 */
  a:hover{
    color: blue;
  }
  a:active{
    color: gold;
  }
  ```
* ​`:focus`​：当输入框聚焦状态

  ```css
  input:focus{
    ...
  }

  ```
* ​`:hover`​：鼠标悬浮状态

  ```css
  div:hover{
    ...
  }
  ```

‍

### 结构伪类选择器

```css
li:first-child{}//同级别的一堆li为一组，第一个li
li:last-child{}//最后一个
li:nth-child(5){}//第五个
li:nth-child(even){}//偶数个:even，2n
li:nth-child(odd){}//奇数个:odd，2n+1
li:nth-child(-n+5){}//前五个，n从0开始
li:nth-child(n+6){}//第6个开始，n从0开始
```

### font

```css
div{
  /* 在谷歌浏览器中，默认字体大小是16px，最小字体大小是12px */
  font-size:16px;

  /* front-width:控制字体宽度，字体粗细可以通过数字100-900之间的整百数进行调整，400是字体默认（normal）700（bold） */
  font-weight: 700;

  /* font-style：控制字体的样式，normal：正常  italic：倾斜 */
  font-style: normal;

  /* font-family 是修改标签内文字的字体，从左到右依次选择 */
  font-family: 楷体, 黑体, 宋体;

  /* 属性连写 */
  /* 连写规定的顺序：swsf（稍微舒服） style weight size family */
  /* 字体类型（倾斜、正常），字体宽度，字体大小，字体样式 */
  /* 至少要留下（连写有四个默认属性，另外两个被设置为默认了）：sf（舒服） size family */
  font:italic bold 50px 楷体;
}
```

### background

```css
div{
  /* 背景颜色 */
  background-color: red;//背景颜色，利用颜色的英文单词
  background-color: rgb(0,0,255);//利用rgb表示法，0-255
  background-color: rgba(255, 0, 0, 0.5);//利用rgba表示法，0-1控制透明程度，0透明，1不透明
  background-color: #ff0000;//十六进制表示法，0-9，a-f
  /* 背景图片，默认如果图片比容器小,则会平铺开 */
  background-image: url(./images/1.jpg);
  /* 设置背景图片的大小 */
  background-size: 200px 200px;//直接设置宽高、设置百分比（参考父盒子）
  /* contain 碰到边框就不变了 */
  background-size: contain;
  /* 等比例放大,知道覆盖完全部盒子大小,其他部分剪切 */
  background-size: cover;
  /* no-repeat: 不平铺 */
  background-repeat: no-repeat;
  /* 方位名词(九宫格火锅)，先水平后垂直 */
  background-position: right top;
  /* 数字px，先水平往右正，垂直往下正 */
  background-position: 100px 50px;
  /* 连写方式: bg */
  background: blue url(./images/1.jpg) no-repeat center center;
}
```

### text

```css
div{
  /* 1.控制文本首行缩进 */
  /* text-indent: 20px; */
  /* 2.方式：数字em：空几个字符大小 */
  text-indent:2em;
  /* overline上划线 */
  /* none去掉字体横线 */
  text-decoration: overline;

  /* 控制文本对齐方式：text-align */
  /* text-align：left center right 左对齐  居中 右对齐 */
  text-align: center;
  /* 让文字在盒子里面垂直居中，让文本的行高等于容器的高度 */
  line-height: 800px;
  /* 数字：1 让行高等于当前文字的文字高度，不要带单位，因为文字本身有单位，数字1可以自动适配文字高度单位 */
  line-height:1 ;

}
```

### border

```css
div{
   /* 边框的连写基本属性：宽度 颜色 线形 */
  border-color: blue;
  border-width: 5px;
  /* 1个值控制四边,2个值控制上下、左右,三个值控制上、左右、下,四个值控制上、右、下、左 */
  border-style: double;//none 无样式  solid 实线  deshan 虚线  double 双实线
  border:1px solid blue;
  /* 设置边框圆角 */
  border-radius: 50%;

  /* 设置内边距，1个值控制四边,2个值控制上下、左右,三个值控制上、左右、下,四个值控制上、右、下、左 */
  padding:10px; //padding-top:20px;(top,bottom,left,right)
  /* 设置外边距，1个值控制四边,2个值控制上下、左右,三个值控制上、左右、下,四个值控制上、右、下、左 */
  margin:10px; //margin-top:20px;(top,bottom,left,right)
}
```

# Less

### 导入less文件

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

### @变量

使用`@`​定义变量

* 样式变量

  ```css
  @color:#999
  p{
    color:@color
  }
  ```
* 选择器变量：变量名必须用`{}`​包裹

  ```css
  @{myDiv}{//放在里面的选择器会有下面的属性
    color:blue;
    font-size:16px
  }

  .{id}{}
  ===
  .id{
    color:blue;
    font-size:16px
  }
  ```

### 计算

```less
div{
  width:20*5px
}

```

### 混入

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

### 继承

继承：同时拥有一样的后者的属性

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

# Sass

### $变量

```scss
定义 $color：red
使用 
div{
  color：$color;
}
```

### 继承

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

### 混入

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
