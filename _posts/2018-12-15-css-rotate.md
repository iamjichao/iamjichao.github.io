---
layout: post
title: CSS3 实现卡片翻转效果
categories: [CSS]
description: CSS3 实现卡片翻转效果
keywords: [CSS3, rotate]
---

做微信小程序时有一个翻卡片的需求，所以研究了一下用 CSS3 实现翻转的动画渐变效果。主要使用了 transition 和 transform 结合实现。

### 了解 transition 和 transform

transition 属性在 [transition 使用心得](https://iamjichao.net/2018/12/08/transition/) 中有详细介绍。

transform 属性向元素应用 2D 或 3D 转换。该属性允许我们对元素进行旋转、缩放、移动或倾斜。

**none** 定义不进行转换。

**matrix(n,n,n,n,n,n)** 定义 2D 转换，使用六个值的矩阵。

**matrix3d(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n)** 定义 3D 转换，使用 16 个值的 4x4 矩阵。

**translate(x,y)** 定义 2D 转换。

**translate3d(x,y,z)** 定义 3D 转换。

**translateX(x)** 定义转换，只是用 X 轴的值。

**translateY(y)** 定义转换，只是用 Y 轴的值。

**translateZ(z)** 定义 3D 转换，只是用 Z 轴的值。

**scale(x,y)** 定义 2D 缩放转换。

**scale3d(x,y,z)** 定义 3D 缩放转换。

**scaleX(x)** 通过设置 X 轴的值来定义缩放转换。

**scaleY(y)** 通过设置 Y 轴的值来定义缩放转换。

**scaleZ(z)** 通过设置 Z 轴的值来定义 3D 缩放转换。

**rotate(angle)** 定义 2D 旋转，在参数中规定角度。

**rotate3d(x,y,z,angle)** 定义 3D 旋转。

**rotateX(angle)** 定义沿着 X 轴的 3D 旋转。

**rotateY(angle)** 定义沿着 Y 轴的 3D 旋转。

**rotateZ(angle)** 定义沿着 Z 轴的 3D 旋转。

**skew(x-angle,y-angle)** 定义沿着 X 和 Y 轴的 2D 倾斜转换。

**skewX(angle)** 定义沿着 X 轴的 2D 倾斜转换。

**skewY(angle)** 定义沿着 Y 轴的 2D 倾斜转换。

**perspective(n)** 为 3D 转换元素定义透视视图。

### 实现动画效果

先创建卡片的结构，一个容器 div ，内部定位两个 div （卡片的正面和反面）。

```html
<div class="content-box">
  <div class="content front">正面</div>
  <div class="content back">反面</div>
</div>
```

```css
.content-box {
  width: 600px;
  height: 600px;
  position: relative;
  /* 距离观测者的距离，使翻转效果有立体感 */
  perspective: 2000px;
  -webkit-perspective: 2000px;
}

.content {
  width: 600px;
  height: 600px;
  position: absolute;
  left: 0;
  top: 0;
  backface-visibility: hidden;
  transition: 1s;
}

.back {
  transform: rotateY(180deg);
}
```

容器 div.content-box 的两个关键属性：

```css
perspective: 2000px;
-webkit-perspective: 2000px;
```

perspective 属性定义 3D 元素距视图的距离，以像素计。该属性允许改变 3D 元素查看 3D 元素的视图。可以理解为元素和观测者的距离，设置该属性可以使动画效果有立体感。当为元素定义 perspective 属性时，其子元素会获得透视效果，而不是元素本身。

perspective 属性只影响 3D 转换元素。可以和 perspective-origin 属性一起使用定义 3D 元素所基于的 X 轴和 Y 轴。

正面 div.front 和反面 div.back 的关键属性：

```css
backface-visibility: hidden;
transition: 1s;
```

backface-visibility 属性定义当元素不面向屏幕时是否可见。设置为 hidden 即正面和反面只显示面向屏幕的一面。

transition 属性定义动画过渡的时间。

反面 div.back 还有一个关键属性：

```css
transform: rotateY(180deg);
```

即反面初始状态时先旋转180度（从卡片上方看逆时针），此时反面背向屏幕，所以不显示。

当触发翻转事件（hover/click/...）时给正面和反面增加以下 CSS 属性：

```css
.front.turn {
  transform: rotateY(-180deg);
}

.back.turn {
  transform: rotateY(0deg);
}
```

正面和反面都旋转了180度（从卡片上方看顺时针），此时正面背向屏幕不显示，反面面向屏幕显示，这样就实现了卡片翻转的动画效果。

[效果展示](https://lab.iamjichao.com)
