---
layout: post
title: 图片加载出错的处理方式
categories: [HTML, CSS]
description: 图片加载出错的处理方式
keywords: [图片加载出错]
---

通常情况下，HTML 中的 img 标签加载出错后会默认显示一张裂开的图片，如果设置了 `alt` 属性，还会在默认图片后面显示 `alt` 属性的内容。

比如这张不存在的图片：

```html
<img src="https://fehub.net/images/xxx.png" alt="我是一张图片">
```

在浏览器中的显示效果如图：

![image](https://fehub.net/images/posts/2020/img-load-error-1.png)

这种默认效果显得非常粗糙，对于用户也不太友好。

### 使用默认图片占位

当图片加载出错的时候，使用一张默认图片（比如一张经过设计师设计的裂开的图片）进行占位，保证页面结构不会错乱。

实现方法如下：

```html
<img src="https://fehub.net/images/xxx.png" alt="我是一张图片" onerror="this.src='error.svg'">
```

```css
img[src$="error.svg"] {
  object-fit: contain;
}
```

这里要注意 `error.svg` 必须可以访问到，否则会一直触发 `onerror` 进入死循环。

在浏览器中的显示效果如图：

![image](https://fehub.net/images/posts/2020/img-load-error-2.png)

### 增加内容识别

当图片加载出错的时候，如果仅仅使用一张默认图片，用户其实并不能识别这张图片的含义。从用户的角度讲，最佳的实现方法应该兼顾视觉表现和内容识别。

实现方法如下：

```html
<img src="https://fehub.net/images/xxx.png" alt="我是一张图片">
```

```css
img {
  display: inline-block;
  transform: scale(1);
}

img::before {
  content: ' ';
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: #f5f5f5 url(error.svg) no-repeat center / 50% 50%;
  color: transparent;
}

img::after {
  content: attr(alt);
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  line-height: 2;
  background-color: rgba(0, 0, 0, .5);
  color: #ffffff;
  font-size: 12px;
  text-align: center;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

在浏览器中的显示效果：

![image](https://fehub.net/images/posts/2020/img-load-error-3.png)

### 实现用户没有头像时显示昵称的第一个汉字或字母

实现思路如下：

- 如果 `img` 加载正常，`::before` 不会生效，利用这个特性在 `::before` 中实现没有头像时的显示效果。

- 利用大字体大行高以及文字换行，在 `::before` 中实现只显示昵称的第一个字。

实现方法如下：

```html
<img class="avatar" src="https://fehub.net/images/avatar.png" alt="FEHub">
```

```css
img.avatar {
  display: inline-block;
  --s: var(--size, 1.5rem);
  width: var(--s);
  height: var(--s);
  border-radius: 50%;
  white-space: normal;
  overflow: hidden;
}

img.avatar::before {
  content: attr(alt);
  display: flex;
  font-size: calc(var(--s) / 2);
  width: 100%;
  height: 100%;
  background-color: bisque;
  line-height: var(--s);
  letter-spacing: var(--s);
  text-indent: var(--s);
  justify-content: center;
  text-align: center;
  color: inherit;
  line-break: anywhere;
}
```

在浏览器中的显示效果如图：

![image](https://fehub.net/images/posts/2020/img-load-error-4.png)

[查看完整 demo](https://fehub.net/demos/posts/2020/img-load-error.html)

以上。
