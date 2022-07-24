---
layout: post
title: 与用户行为相关的 CSS 属性
categories: [CSS]
description: 与用户行为相关的 CSS 属性
keywords: [CSS]
---

CSS 中有几个和用户行为密切相关的属性，虽然平时不怎么用，但要知道有这几个东西，以备不时之需。

### user-select

> CSS 属性 [user-select](https://developer.mozilla.org/zh-CN/docs/Web/CSS/user-select) 控制用户能否选中文本。

```css
user-select: none; /* 文本不可选中 */
user-select: auto; /* 自动 */
user-select: text; /* 文本可选中 */
user-select: all; /* 元素及其子元素会全部被选中 */
user-select: contain; /* 允许在元素内选择 */
```

我们常实现的禁止文本复制功能，手机端会设置如下 CSS 代码，此时整个页面无法通过长按唤起系统自带的复制等操作：

```css
body {
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
```

这个属性的兼容性还不错，可以放心使用。

![image](https://fehub.net/images/posts/user-behavior-css-1.png)

### user-drag

> CSS 属性 -webkit-user-drag 控制用户拖拽行为。

```css
-webkit-user-drag: none; /* 图片文字可拖拽 */
-webkit-user-drag: auto; /* 图片文字不可拖拽 */
-webkit-user-drag: text; /* 元素可拖拽 Safari only */
```

设置图片不可拖拽：

```css
img.no-drag {
  -webkit-user-drag: none;
}
```

这个属性兼容性不好，要谨慎使用。

![image](https://fehub.net/images/posts/user-behavior-css-2.png)

如果有相关需求可以使用 HTML 元素的 [`draggable`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/draggable) 属性实现：

```html
<!-- 元素可以拖拽 -->
<img src="demo.png" draggable="true" />

<!-- 元素不可以拖拽 -->
<img src="demo.png" draggable="false" />
```

### user-modify

CSS 属性 [user-modify](https://developer.mozilla.org/en-US/docs/Web/CSS/user-modify) 控制元素的内容是否可以由用户编辑。

```css
user-modify: read-only; /* 内容只读 */
user-modify: read-write; /* 用户能够读取和写入内容，支持富文本 */
user-modify: read-write-plaintext-only; /* 用户能够读取和写入内容，但富文本格式将丢失 */
user-modify: write-only; /* 用户可以编辑内容，但不能阅读 */
```

这个属性的兼容性还不错，但它不是标准语法，可能会从相关的 Web 标准中删除，不再推荐使用此功能。

![image](https://fehub.net/images/posts/user-behavior-css-3.png)

如果有相关需求可以使用 HTML 元素的 [`contenteditable`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/contenteditable) 属性实现：

```html
<!-- 元素可编辑 -->
<div contenteditable="true"></div>

<!-- 元素可编辑，只能输入纯文本 -->
<div contenteditable="plaintext-only"></div>

<!-- 元素不可编辑 -->
<div contenteditable="false"></div>
```

以上。

### 参考资料

- [用户行为CSS三剑客 - 哔哩哔哩](https://b23.tv/C17dK3j)
- [user-select CSS - MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/user-select)
- [user-modify CSS - MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/user-modify)
- [draggable HTML - MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/draggable)
- [contenteditable HTML - MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/contenteditable)
