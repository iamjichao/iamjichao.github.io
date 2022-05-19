---
layout: post
title: 使用 clipboard.js 实现文本复制剪切
categories: [JavaScript, HTML]
description: 使用 clipboard.js 实现文本复制剪切
keywords: [JavaScript, clipboard.js]
---

工作中需要实现这样一个需求：在订单号后边提供一个复制订单号的按钮，使用了 [clipboard.js](https://github.com/zenorocha/clipboard.js.git)，在这里记录一下使用方法和注意事项。

### 使用步骤

- 安装依赖。在项目根目录下执行：

```
npm install clipboard --save
```

- 在项目入口引入：

```js
import Clipboard from 'clipboard';
```

- 在页面中使用：

复制输入框中的文本

```html
<!-- Target -->
<input id="copy" value="clipboard.js" />

<!-- Trigger -->
<button class="btn" data-clipboard-target="#copy">复制</button>
```

剪切输入框中的文本

```html
<!-- Target -->
<textarea id="copy">clipboard.js</textarea>

<!-- Trigger -->
<button class="btn" data-clipboard-action="cut" data-clipboard-target="#copy">剪切</button>
```

直接复制文本

```html
<!-- Trigger -->
<button class="btn" data-clipboard-text="clipboard.js">复制</button>
```

自定义事件：复制成功/复制失败

```js
const clipboard = new Clipboard('.copy');

clipboard.on('success', function(e) {
  // 复制成功
  console.log(e);
});

clipboard.on('error', function(e) {
  // 复制失败
  console.log(e);
});
```

销毁 Clipboard 对象，常用于单页面应用中管理 DOM 的生命周期

```js
clipboard.destroy();
```

### 浏览器兼容

- Chrome 42+

- Edge 12+
 
- Firefox 41+

- IE 9+

- Opera 29+

- Safari 10+

可以通过 `ClipboardJS.isSupported()` 来检查是否支持 clipboard.js。

以上。
