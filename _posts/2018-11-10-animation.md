---
layout: post
title: CSS3 animation 属性
categories: [CSS]
description: CSS3 animation 属性
keywords: [animation]
---

CSS [animation](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation) 属性是 `animation-name`，`animation-duration`, `animation-timing-function`，`animation-delay`，`animation-iteration-count`，`animation-direction`，`animation-fill-mode` 和 `animation-play-state` 的一个简写属性。

### animation 属性值

- [**animation-name**](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-name) 指定动画名称，每个名称代表一个由 [@keyframes](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-name) 定义的动画序列。

- [**animation-duration**](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-duration) 指定一个动画周期的时长。默认值为 0，表示无动画。

- [**animation-timing-function**](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-timing-function) 定义 CSS 动画在每一动画周期中执行的节奏。

- [**animation-delay**](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-delay) 定义动画于何时开始，即从动画应用在元素上到动画开始的这段时间的长度。

- [**animation-iteration-count**](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-iteration-count) 定义动画在结束前运行的次数。

- [**animation-direction**](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-direction) 指示动画是否反向播放。

- [**animation-fill-mode**](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-fill-mode) 设置 CSS 动画在执行之前和之后如何将样式应用于其目标。

- [**animation-play-state**](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-play-state) 定义一个动画是否运行或者暂停，可以通过查询它来确定动画是否正在运行。

### @keyframes 规则

- 起止关键帧可以不设置

以下代码 fadeIn 动画没有设置起始 opacity，会自动使用当前元素的 opacity 作为动画的起点。

```css
@keyframes fadeIn {
  100% {
    opacity: 1;
  }
}

div {
  opacity: 0.5;
}

/* opacity: 0.5 -> 1 */
div.active {
  animation: fadeIn 1s;
}
```

- 不同关键帧是无序的，CSS 属性相同的关键帧可合并

```css
@keyframes flash {
  0%, 50%, 100% {
    opacity: 0.5;
    top: 2px;
  }
  25%, 75% {
    opacity: 1;
  }
}
```

- 同一关键帧的同一 CSS 属性会被覆盖

```css
/* opacity: 0.6 -> 1 */
/* top: 0 -> 10px */
@keyframes flash {
  0% {
    opacity: 0.2;
    top: 0;
  }
  0% {
    opacity: 0.6;
  }
  100% {
    opacity: 1;
    top: 10px;
  }
}
```

- 关键帧的 CSS 属性可以不连续

```css
/* opacity: 0%/0.2 -> 33%/0.6 -> 100%/1 */
/* top: 0%/0 -> 66%/10px -> 100%/20px */
@keyframes flash {
  0% {
    opacity: 0.2;
    top: 0;
  }
  33% {
    opacity: 0.6;
  }
  66% {
    top: 10px;
  }
  100% {
    opacity: 1;
    top: 20px;
  }
}
```

- 关键帧中的 CSS 属性优先级最高，设置 `!important` 无效

```html
<div style="opacity: 0.5 !important;"></div>
```

```css
@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

/* opacity: 0 -> 1 */
div {
  animation: fadeIn 1s;
}
```

- @keyframes 命名规范

名称必须符合 CSS 语法中对标识符的定义：[\<custom-ident\> - MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/custom-ident)。

### animation 应用

- 将复杂动画细化，分别设置，然后组合在一起，方便重复利用

- 使用负延时让动画立即播放（提前播放）

```html
<div class="loading">
  <i></i>
  <i></i>
  <i></i>
  <i></i>
</div>
```

```css
@keyframes scale {
  to {
    transform: scaleY(10);
  }
}

/* loading 动画执行时所有元素都在动画进程中了 */
.loading i {
  height: 2px;
  border-left: 2px solid deepskyblue;
  animation: scale 3s linear infinite alternate;
}

.loading i:nth-child(2) {
  animation-delay: -1s;
}

.loading i:nth-child(3) {
  animation-delay: -2s;
}

.loading i:nth-child(4) {
  animation-delay: -3s;
}
```

- 使用 `animation-direction` reverse 关键字让动画倒放

```css
/* 顺时针和逆时针只需要实现一个动画规则 */
@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

/* 顺时针 */
.clockwise {
  animation: spin 5s infinite;
}

/* 逆时针 */
.anticlockwise {
  animation: spin reverse 5s infinite;
}
```

- 巧用 `animation-direction` alternate 关键字实现钟摆动画

```css
@keyframes swing {
  0% {
    transform: rotate(-10deg);
  }
  100% {
    transform: rotate(10deg);
  }
}

/* 钟摆运动 */
.clock {
  transform-origin: top;
  animation: swing 1s infinite alternate ease-in-out;
}
```

- 设置 `animation-iteration-count` 为小数让动画播放一部分

```css
@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

/* opacity: 0 -> 1 */
.visible {
  animation: fadeIn .4s both;
}

/* opacity: 0 -> 0.4 */
.visible:disabled {
  animation: fadeIn 1s .4 both;
}
```

- 设置 `animation-play-state` 给固定颜色添加透明度

```css
@keyframes opacityColor {
  0% {
    color: transparent;
  }
  100% {
    color: deepskyblue;
  }
}

/* p 元素内的文字色值为 50% 透明度的 deepskyblue */
/* 动画提前运行一半然后暂停，这想法绝了 */
p {
  animation: opacityColor 1s -.5s linear paused;
}
```

以上。
