---
title: 04、css笔记
date: 2021-01-30 13:04:36
tags:
---

#### 概念

回流（reflow）：当 render tree 中的元素宽高、布局、显示、隐藏或元素内部文字结构发生变化，会影响自身及父元素，甚至追溯到更多的祖先元素发生改变，则会导致元素内部、周围甚至整个页面的重新渲染，页面发生重构，回流就产生了。

重绘（repaint）：元素的结构（宽高、布局、显示隐藏、内部文字大小）未发生改变，只是元素的外观样式发生改变，比如背景颜色、内部文字颜色、边框颜色等。此时会引起浏览器重绘，显然重绘的速度快于回流。

回流一定会触发重绘，重绘不一定触发回流。

#### 对性能影响

这里了解一个知识点：渲染 css 样式会影响 js 执行的时间，使得加载 js 脚本变慢。原因如下：

浏览器渲染一个网页的时候会启用两条线程：一条渲染 javascript 脚本，另一条渲染 ui 即 css 样式的渲染。但这两条线程是互斥的，当 javascript 线程运行的时候 ui 线程则会中止暂停，反之亦然。因为当 ui 线程运行对页面进行渲染的时候， js 脚本难免会涉及到页面视图上的一些样式的改变，为了使这个改变更加准确 js 脚本只好等待 ui 线程渲染完成的时候才去执行。

所以当一个页面的元素样式改动频繁的时候 ui 线程就会持续渲染，造成 js 代码反应慢半拍，卡顿的情况。回流和重绘都会使得 ui 线程渲染时间加长，太多就会使得网站性能变差，因此要尽量减少 reflow 和 repaint。

#### 常见回流

改变窗口大小、文字大小、激活伪类、操作 class，style 属性，脚本操作 dom、计算 offsetWidth 和 offsetHeight
对应 css 属性：盒子模型、定位浮动、节点内部文字结构

#### 常见重绘

元素外观样式改变：边框颜色、文字颜色、背景颜色

#### 减少回流和重绘注意点

1、用 transform 代替 top，left ，margin-top， margin-left... 这些位移属性

#### 弹性子元素内容超出处理

```js
<div class="one">
  <div class="l"></div>
  <div class="c">
    <p>
      ssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssss
    </p>
  </div>
  <div class="r"></div>
</div>
.one {
    width: 100%;
    display: flex;
}

.l,
.r {
    height: 100px;
    width: 200px;
    background: red;
}

.c {
    flex: 1;
    display: table-cell;
}

.c p {
    display: table;
    width: 100%;
    table-layout: fixed;
    word-wrap: break-word;
}
```
