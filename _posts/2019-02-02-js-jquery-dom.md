---
layout: post
title: jQuery 与原生 JS 的 DOM 操作对比
categories: [jQuery, JavaScript]
description: jQuery 与原生 JS 的 DOM 操作对比
keywords: jQuery, JavaScript
---


### 1. 创建元素节点

- 原生 JS 创建元素节点：`document.createElement("p");`
- jQuery 创建元素节点：`$('<p></p>');`
<!--more-->
### 2. 创建并添加文本节点:

- 原生 JS 创建文本节点：`document.createTextNode("Text Content");`
- 通常创建文本节点和创建元素节点配合使用，比如：

```js
var textEl = document.createTextNode("Hello World.");
var pEl = document.createElement("p");
pEl.appendChild(textEl);
```

- jQuery 创建并添加文本节点：`var $p = $('<p>Hello World.</p>');`

### 3. 复制节点

- 原生 JS 复制节点：`var newEl = pEl.cloneNode(true);  `
	- `true` 和 `false` 的区别：
        - `true`：克隆整个`'<p>Hello World.</p>'`节点
        - `false`：只克隆`'<p></p>' `，不克隆文本`'Hello World.'`

- jQuery 复制节点：`$newEl = $('#pEl').clone(true);`

**注意**：克隆节点要避免 `id` 重复

### 4. 插入节点

- 原生 JS 向子节点列表的末尾添加新的子节点：
`El.appendChild(newNode);`
- 原生 JS 在节点的已有子节点之前插入一个新的子节点：
`El.insertBefore(newNode, targetNode);`

- 在 jQuery 中，插入节点的方法比原生 JS 多的多：
  - `$('#El').append('<p>Hello World.</p>');`	 在匹配元素子节点列表结尾添加内容
  - `$('<p>Hello World.</p>').appendTo('#El');`  把匹配元素添加到目标元素子节点列表结尾
  - `$('#El').prepend('<p>Hello World.</p>');`  在匹配元素子节点列表开头添加内容
  - `$('<p>Hello World.</p>').prependTo('#El');`  把匹配元素添加到目标元素子节点列表开头
  - `$('#El').before('<p>Hello World.</p>');`  在匹配元素之前添加目标内容
  - `$('<p>Hello World.</p>').insertBefore('#El');`  把匹配元素添加到目标元素之前
  - `$('#El').after('<p>Hello World.</p>');`  在匹配元素之后添加目标内容
  - `$('<p>Hello World.</p>').insertAfter('#El');`  把匹配元素添加到目标元素之后

### 5. 删除节点

- 原生 JS 删除节点：`El.parentNode.removeChild(El);`
- jQuery 删除节点：` $('#El').remove();`

### 6. 替换节点

- 原生 JS 替换节点：`El.repalceChild(newNode, oldNode);`

**注意**：`oldNode` 必须是 `parentEl` 真实存在的一个子节点

- jQuery 替换节点:` $('p').replaceWith('<p>Hello World.</p>');`

### 7. 设置属性 / 获取属性

- 原生 JS 设置属性 / 获取属性:
   - `imgEl.setAttribute("title", "logo");`
   - `imgEl.getAttribute("title");`
   - `checkboxEl.checked = true;`
   - `checkboxEl.checked;`

- jQuery 设置属性 / 获取属性:

   -` $("#logo").attr({"title": "logo"});`
   - `$("#logo").attr("title");`
   - `$("#checkbox").prop({"checked": true});`
   - `$("#checkbox").prop("checked");`

