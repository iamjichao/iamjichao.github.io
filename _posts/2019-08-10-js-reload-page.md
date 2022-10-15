---
layout: post
title: JS 脚本实现页面自动刷新
categories: [JavaScript]
description: JS 脚本实现页面自动刷新
keywords: [JavaScript, 页面刷新]
---

如何在浏览器中刷新当前页面呢？如果你觉得这个问题很简单，那可以关闭这篇不太实用的文章，因为它可能会浪费你几分钟时间。

### 如何刷新当前页面

- 点击浏览器的刷新按钮
- 鼠标右键刷新
- 键盘快捷键 F5/Ctrl+F5(Windows) Command+R/Command+Shift+R(macOS)
- 控制台通过 JS 代码刷新

### 通过 JS 刷新页面

JS 有以下几种刷新页面的方法：

```js
history.go(0);

location.reload();

location = location;

location.href = location.href;

location.assign(location);

location.replace(location);
```

在控制台中执行这几种方法都可以刷新当前页面，但只能刷新一次。怎样可以让页面持续刷新呢？

### 重复刷新页面

将以下代码在控制台中执行，弹出提示框，输入刷新时间间隔（秒），每过几秒就会刷新一次页面。

```js
var timeout = prompt('设置刷新时间间隔(S)');
// 获取当前的 url
var current = location.href;

if (timeout > 0) {
  // 时间间隔大于 0，timeout 秒之后执行 reload 函数
  setTimeout(reload, 1000 * timeout);
} else {
  // 时间间隔不大于 0，仅刷新一次
  location.replace(current);
}

function reload() {
  // timeout 秒后执行 reload 函数，实现无限循环刷新
  setTimeout(reload, 1000 * timeout);
  // 下面的代码格式化后的内容为：
  // <frameset cols="*">
  //   <frame src="当前地址栏的 url" />
  // </frameset>
  var frame = '<frameset cols="*">\n<frame src="' + current + '" />\n</frameset>';
  // 调用 write 方法写入框架，打开新窗口
  document.write(frame);
  // 关闭上面的窗口
  document.close();
}
```

没用的知识又增加了。

以上。
