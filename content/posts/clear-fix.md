+++
date = '2026-09-27T17:19:37+08:00'
title = 'CSS 清除 Float'
categories = 'CSS'
tags = ['CSS']
description = '浮动元素不会撑开父容器的高度，本文介绍用 overflow: hidden 建立 BFC 和 clearfix 伪元素两种处理方式，以及各自的适用场景。'
+++

浮动元素会脱离普通文档流。它仍然影响周围行内内容的排列，但父元素计算自身高度时，通常不会把浮动子元素算进去。如果父元素里没有其他足够高的普通流内容，边框和背景就只包住段落、包不住浮动盒子——父元素看起来像"没有高度"，后面的内容也可能和浮动元素发生意外重叠。

先看一个例子：

```html
<div class="float-container">
  <div class="float-box">浮动内容</div>
  <p>普通文档流中的内容</p>
</div>
```

```css
.float-container {
  padding: 8px;
  border: 1px solid #ccc;
}

.float-box {
  float: left;
  width: 100px;
  height: 100px;
  padding: 8px;
  margin: 8px;
  line-height: 84px;
  background-color: beige;
  border: 1px solid #ccc;
  text-align: center;
}
```

容器的边框没有包住浮动盒子：

![未清除浮动时的效果](/images/Snipaste_2026-09-27_17-33-56.png)

常见的处理方式有两种：让父元素建立块级格式化上下文（BFC），或者使用 clearfix 伪元素。两者都能让父元素包住内部的浮动，但原理不同。

## 方法一：用 `overflow: hidden` 建立 BFC

给容器加上一个类：

```css
.overflow-hidden {
  overflow: hidden;
}
```

```html
<div class="float-container overflow-hidden">
  <div class="float-box">浮动内容</div>
  <p>普通文档流中的内容</p>
</div>
```

![overflow: hidden 的效果](/images/Snipaste_2026-09-27_17-34-29.png)

`overflow` 取 `visible` 和 `clip` 之外的值（这里是 `hidden`）时，元素会建立新的 BFC。BFC 在计算自身高度时会包含内部的浮动元素，所以父容器的边框和背景延伸到了浮动盒子底部。

### 注意事项

`overflow: hidden` 并不只是"清除浮动"：超出容器边界的内容会被裁切，比如阴影、定位出去的弹层和下拉菜单，它还会创建一个可编程滚动的裁剪区域。如果本来不需要裁切内容，就不要只为了包住浮动而随手加这一句。

## 方法二：使用 clearfix 伪元素

```css
.clear-fix::after {
  content: "";
  display: block;
  clear: both;
}
```

```html
<div class="float-container clear-fix">
  <div class="float-box">浮动内容</div>
  <p>普通文档流中的内容</p>
</div>
```

![clearfix 的效果](/images/Snipaste_2026-09-27_17-35-18.png)

`::after` 在容器内容的末尾生成一个空的块级伪元素，`clear: both` 让它避开前面的左右浮动，排到浮动盒子下方。这个伪元素处于普通文档流中，于是把父容器的自动高度撑到了浮动元素底部。和 `overflow: hidden` 不同，clearfix 不会裁切容器溢出的内容。

## 两种方式怎么选

| 方式               | 如何让父容器包住浮动                 | 需要留意的地方                 |
| ------------------ | ------------------------------------ | ------------------------------ |
| `overflow: hidden` | 建立 BFC，BFC 的高度计算包含内部浮动 | 可能裁切溢出内容               |
| clearfix 伪元素    | 在浮动之后生成一个清除浮动的普通流块 | 需要在父容器上应用 clearfix 类 |

如果确实希望裁切溢出内容，`overflow: hidden` 简单直接；如果只想让容器包含浮动、不改变溢出内容的显示方式，clearfix 更合适。

现代 CSS 还可以用 `display: flow-root` 明确地为容器建立 BFC：

```css
.float-container {
  display: flow-root;
}
```

它同样能让父容器包含内部浮动，且不会像 `overflow: hidden` 那样裁切内容。写新代码时，如果目标只是建立 BFC，通常优先考虑 `flow-root`。
