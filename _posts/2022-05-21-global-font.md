---
layout: post
title: 网页设置全局字体的正确打开方式
categories: [CSS]
description: 网页设置全局字体的正确打开方式
keywords: [CSS]
---

### font-family

> CSS 属性 [font-family](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-family) 允许通过给定一个有先后顺序的由字体名或者字体族名组成的列表来为选定的元素设置字体，属性值用逗号隔开。浏览器会选择列表中第一个该计算机上有安装的字体，或者是通过 @font-face 指定的可以直接下载的字体。

### @font-face

> CSS at-rule [@font-face](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face) 指定一个用于显示文本的自定义字体，字体能从远程服务器或者用户本地安装的字体中加载。如果提供了 local() 函数，从用户本地查找指定的字体名称，并且找到了一个匹配项，本地字体就会被使用，否则字体就会使用 url() 函数下载的资源。

### 在项目中的应用

我们通常会给页面的根元素设置 font-family 属性来指定全局字体，但是不同系统对于字体的支持效果不同，比如苹果系统中的苹方字体在 Windows 系统的 1 倍屏下是瘦的，而且边缘模糊。

对于同一系统来说，不同版本的系统字体也有所不同，比如 macOS：

| macOS 版本     | 系统字体      |
|:--------------|:-------------|
| 10.11         | San Francisco |
| 10.9          | Lucida Grande |
| 10.1          | Helvetica Neue|

如果给 font-family 设置一种具体的字体，不仅可能不适配不同系统，而且在系统升级时，网页将无法享受字体升级的体验，所以全局字体应该设置成系统字体，不要指定具体的字体。

全局字体 font-family 应该包含：

- 系统字体
- 兜底字体：系统字体不支持时使用的字体
- Emoji 字体：支持彩色的 emoji 表情 👉 😏😳😱

### 怎样设置全局字体

直接上干货：

```css
/* font-family 最佳设置 */
@font-face {
  font-family: Emoji;
  src:
    local("Apple Color Emojiji"), /* macOS/iOS */
    local("Segoe UI Emoji"), /* Windows8.1/10 */
    local("Segoe UI Symbol"), /* windows7/8 */
    local("Noto Color Emoji"); /* Android */
  unicode-range: U+1F000-1F644, U+203C-3299;
}

html {
  font-family:
    system-ui, /* 最新规范的系统字体 Chrome/Safari */
    —apple-system, /* 系统字体 macOS 下的 FireFox */
    Segoe UI, /* 兜底字体 Windows */
    Rototo, /* 兜底字体 Android */
    Emoji, /* Emoji 自定义字体 */
    Helvetica, /* 兜底字体 macOS/iOS */
    Arial, /* 兜底字体 old Windows */
    sans-serif; /* 兜底字体 old Android */
}
```

注意 Emoji 字体的位置，因为 Helvetica 和 Arial 字体的 Unicode 范围会覆盖 Emoji 字体，所以要把 Emoji 字体放在它们前边。把 Emoji 字体放在最前边也会有问题，因为可能会覆盖其他普通字体，这时可以通过 @font-face 中的 unicode-range 来限制 Emoji 字体作用的字符范围，即 Unicode 在此范围内时可以使用 Emoji 字体。

还有一些其他字体的设置：

```css
/* 衬线字体 */
.font-serif {
  font-family: Georgia, Cambria, "Times New Roman", Times, serif;
}

/* 等宽字体 */
.font-mono {
  font-family: Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
}
```

### 举一反三

- 通用字体 generic-name 是某一类字体的统称，它又包括多个字体族。在 font-family 列表的末尾应该至少有一个通用字体族名，这是一种备选机制，可以在指定的字体不可用时显示较好的字体。

  - serif 带衬线字体
  - sans-serif 无衬线字体
  - monospace 等宽字体
  - cursive 草书字体
  - fantasy 具有特殊艺术效果的字体
  - system-ui 从浏览器所处平台处获取的默认用户界面字体
  - math 针对显示数学相关字符的特殊样式问题而设计的字体
  - emoji 专门用于呈现 Emoji 表情符号的字体
  - fangsong 一种汉字字体

- 在 font-family 列表中，字体族名如果包含空格，应该加引号。

```css
font-family: "Gill Sans Extrabold", sans-serif;
```

### 参考资料

- [网页的默认全局字体该如何设置？- 哔哩哔哩](https://b23.tv/8zYhEob)
- [font-family | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-family)
- [@font-face | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face)
