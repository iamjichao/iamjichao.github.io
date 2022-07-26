---
layout: post
title: CSS3 transition 属性
categories: [CSS]
description: CSS3 transition 属性
keywords: [transition]
---

transition 属性是一个简写属性，用于设置四个过渡属性：

```css
transition: property duration timing-function delay;
```

默认值：all 0 ease 0

### transition-property

transition-property 属性规定应用过渡效果的 CSS 属性的名称。当指定的 CSS 属性改变时，过渡效果将开始。过渡效果通常在用户将鼠标指针浮动到元素上时发生。

```css
transition-property: none|all|property;
```

默认值：all

**none** 没有属性会获得过渡效果。

**all** 所有属性都将获得过渡效果。

**property** 定义应用过渡效果的 CSS 属性名称列表，列表以逗号分隔。

### transition-duration

transition-duration 属性规定完成过渡效果需要花费的时间（以秒或毫秒计）。请始终设置 transition-duration 属性，否则时长为 0，就不会产生过渡效果。

```css
transition-duration: time;
```

默认值：0

**time** 规定完成过渡效果需要花费的时间（以秒或毫秒计）。默认值是 0，意味着不会有效果。

### transition-timing-function

transition-timing-function 属性规定过渡效果的速度曲线。该属性允许过渡效果随着时间来改变其速度。

```css
transition-timing-function: linear|ease|ease-in|ease-out|ease-in-out|cubic-bezier(n,n,n,n);
```

默认值：ease

**linear** 规定以相同速度开始至结束的过渡效果（等于 cubic-bezier(0,0,1,1)）。

**ease** 规定慢速开始，然后变快，然后慢速结束的过渡效果（等于 cubic-bezier(0.25,0.1,0.25,1)）。

**ease-in** 规定以慢速开始的过渡效果（等于 cubic-bezier(0.42,0,1,1)）。

**ease-out** 规定以慢速结束的过渡效果（等于 cubic-bezier(0,0,0.58,1)）。

**ease-in-out** 规定以慢速开始和结束的过渡效果（等于 cubic-bezier(0.42,0,0.58,1)）。

**cubic-bezier(n,n,n,n)** 在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值。

### transition-delay

transition-delay 属性规定过渡效果何时开始。值以秒或毫秒计。

```css
transition-delay: time;
```

默认值：0

**time** 规定在过渡效果开始之前需要等待的时间，以秒或毫秒计。

### 应用：实现不确定高度元素展开和收起的动画效果

使用 CSS 实现展开收起的动画效果，首先想到的就是通过 height+overflow:hidden 实现，使用这种方法一般都是将 height 的高度固定写死，高度 auto 的话是无法形成过渡动画效果的。auto 是一个关键字值，并非数值，因此 height 从 0 到 auto 是无法计算的。此时可以通过 max-height 实现。

给元素设置 box 类，展开时增加 box-unfolded 类，收起时去掉 box-unfolded 类：

```css
.box {
  max-height: 0;
  overflow: hidden;
  -webkit-transition: max-height 600ms;
  -moz-transition: max-height 600ms;
  -o-transition: max-height 600ms;
  transition: max-height 600ms;
}
.box-unfolded {
  max-height: 2000px;
}
```

max-height 相对 height 来说比较灵活。两者的区别就是计算高度的过程，一个是由人为计算，一个由盒子内容高度去计算然后自适应。这种写法必须给定足够存放内容的高度，所以尽量把 max-height 设置大一点。[效果展示](https://lab.iamjichao.com)

### 应用：模拟长悬停或长按效果

```css
div {
  opacity: .9;
}

div.active {
  opacity: .99;
  transition: opacity 350ms;
}
```

```js
// 鼠标长时间悬停
div.onmouseenter = function () {
  this.classList.add('active');
}
div.onmouseleave = function () {
  this.classList.remove('active');
}

// 移动端长按
div.ontouchstart = function () {
  this.classList.add('active');
}
div.ontouchend = function () {
  this.classList.remove('active');
}

div.addEventListener('transitionend', function () {
  // 触发长悬停/长按
});
```

以上。
