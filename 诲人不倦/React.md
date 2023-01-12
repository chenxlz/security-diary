---
title: React
date: 2022-12-11T13:16:16Z
lastmod: 2022-12-26T20:33:34Z
---

![image](assets/v2-f4de783dfb23dc0550fe06fcb5155810_r-20221222173841-glzcvh1.jpg)

# React

# React

## 基本概念

　　属性名表达式

　　[key]  

　　当属性名是一个表达式的时候，要用中括号括起来

## 组件传值

### 父子组件传值

* 核心：函数是一种数据类型，可以作为参数进行传递
* 父组件

  ```js
  import Son from "./Son"
  function Father(){
    return <div>
      <div>
      我是父组件啊
      </div>
      <Son address="北京" girl={20} />
    </div>
  }
  export default Father
  ```

### 兄弟组件传值

### 隔代组件传值

## 生命周期

## 样式

### 行内样式

### 外部样式

### 样式私有

## 非受控组件

# 路由
