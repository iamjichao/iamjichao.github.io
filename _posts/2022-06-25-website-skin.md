---
layout: post
title: 网站换肤的正确打开方式
categories: [HTML, CSS]
description: 网站换肤的正确打开方式
keywords: [HTML, CSS]
---

网站换肤是一个常见的前端需求，比如切换日间和夜间模式。它的思路很简单，就是通过加载不同的 CSS 让页面显示不同的样式。实现方法有很多，但效果不尽相同。

### 常见的网站换肤实现

- 一个全局的 class 控制样式切换

```css
.default.light {
   background: white;
}

.dark {
   background: gray;
}
```

这个方法的缺点是代码啰嗦，优先级不好控制，维护成本高，不推荐。

- 改变 `link` 元素的 `href` 地址

```html
<link id="skin" href="default.css" rel="stylesheet" type="text/css" />
```

通过 JS 改变 `href` 地址：

```js
document.querySelector('#skin').href = 'dark.css';
```

这个方法的缺点是有延迟，切换不流畅。


### 使用原生 HTML 特性实现流畅的换肤效果

首先了解一下 `link` 标签的 `rel` `title` `disabled` 属性，详细内容参考 [\<link\>：外部资源链接元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/link)。

- rel 属性用于声明链接文档与当前文档的关系，属性值参考 [链接类型值](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Link_types)，多个用空格分隔。

```html
<link rel="stylesheet" />
<link rel="alternate stylesheet" />
```

- title 属性用于 `<link rel="stylesheet">` 时会定义一个首选样式表或备用样式表。

- disabled 属性仅用于 `<link rel="stylesheet">`，控制是否加载并应用所描述的样式表。如果指定了 `disabled`，在页面加载期间不会加载此样式表。如果将 `disabled` 属性更改为 `false` 或删除，样式表将按需加载。

网页中的样式表分为三类：

- Persistent (no rel="alternate", no title)：总是应用。

- Preferred (no rel="alternate", with title="…")：默认应用，如果选择了另一个样式表则禁用。只能有一个首选样式表，多个不同标题的样式表只能生效一个。

- Alternate (rel="alternate stylesheet", title="…")：默认禁用，可以选择。

```html
<!-- 加载并渲染，总是应用 -->
<link href="basic.css" rel="stylesheet" type="text/css" />

<!-- 加载并渲染，默认应用 -->
<link href="default.css" rel="stylesheet" type="text/css" title="Default" />

<!-- 只加载不渲染，备用可选 -->
<link href="light.css" rel="alternate stylesheet" type="text/css" title="Light" />
<link href="dark.css" rel="alternate stylesheet" type="text/css" title="Dark" />
```

通过 JS 控制 `link` 元素的 `disabled` 属性来启用和禁用样式表：

```js
// 启用
document.querySelector('link[href="light.css"]').disabled = false;

// 禁用
document.querySelector('link[href="light.css"]').disabled = true;
```

这个方法兼容性很好，切换不会有延迟，交互体验更好，推荐使用。

以上。

### 参考资料

- [网站换肤的最佳实现 - 哔哩哔哩](https://b23.tv/PLImiGs)
- [\<link\>：外部资源链接元素 - MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/link)
- [链接类型 - MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Link_types)
- [Alternative style sheets - MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Alternative_style_sheets)
