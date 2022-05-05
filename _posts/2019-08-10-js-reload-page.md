---
layout: post
title: JS 脚本实现页面自动刷新
categories: [JavaScript]
description: JS 脚本实现页面自动刷新
keywords: [JavaScript, 页面刷新]
---

如果只知道页面的 url，怎样刷新当前页面呢？

JS 有几种刷新页面的方法：

```js
history.go(0)

location.reload()

location = location

location.href = location.href

location.assign(location)

location.replace(location)
```

在控制台中执行这几种方法都可以刷新当前页面，但只能刷新一次。怎样可以让页面持续刷新呢？

将以下代码在控制台中执行，弹出提示框，输入刷新时间间隔（秒），每过几秒就会刷新一次页面。

```js
var timeout = prompt("设置刷新时间间隔[S]");
// 获取当前的URL
var current = location.href;

if (timeout > 0) {
  // 时间间隔大于0，timeout秒之后执行reload函数
  setTimeout('reload()', 1000 * timeout);
} else {
  // 时间间隔不大于0，仅刷新一次
  location.replace(current);
}

function reload() {
  // timeout秒后执行reload函数，实现无限循环刷新
  setTimeout('reload()', 1000 * timeout);
  // 下面两行代码的格式化后的内容为：
  // <frameset cols='*'>
  //   <frame src='当前地址栏的URL' />
  // </frameset>
  var fr4me = '<frameset cols=\'*\'>\n<frame src=\'' + current + '\' />';
  fr4me += '</frameset>';

  with (document) {
    // 引用document对象，调用write方法写入框架，打开新窗口
    write(fr4me);
    // 关闭上面的窗口
    void(close());
  };
}
```

以上。
