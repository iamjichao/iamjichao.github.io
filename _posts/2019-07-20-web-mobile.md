---
layout: post
title: 移动端 web 开发常见问题及解决方案
categories: [JavaScript, CSS, HTML]
description: 移动端 web 开发常见问题及解决方案
keywords: [移动端, web]
---

### HTML

- video 禁止全屏播放

```html
<video x-webkit-airplay="true" webkit-playsinline="true" playsinline="true"></video>
```

### CSS

- 去除点击元素产生背景或边框

```css
* {
  -webkit-tap-highlight-color: rgba(0,0,0,0);
}
```

- 禁止长按链接与图片弹出菜单

```css
a, img {
  -webkit-touch-callout: none;
}
```

- 禁止用户选中图片或文字

```css
body {
  -webkit-user-select: none;
}
```

- 禁用 webkit 内核浏览器的文字大小调整功能

```css
body {
  -webkit-text-size-adjust: none;
}
```

- 去除表单元素默认样式

```css
input {
  -webkit-appearance: none;
  outline: none;
}
```

- 改变输入框 placeholder 的颜色

```css
::-webkit-input-placeholder {
  color: #999;
}

input:focus::-webkit-input-placeholder {
  color: #999;
}
```

- line-height=height 在部分安卓手机不垂直居中，显示偏上

```css
p {
  line-height: normal;
  padding: 10px 0;
}
```

- 屏幕旋转的事件和样式

```js
var resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize';
window.addEventListener(resizeEvt, function(){}, false);

// 竖屏时样式
@media all and (orientation:portrait){}
// 横屏时样式
@media all and (orientation:landscape){}
```

### JS

- iOS 中不支持"2000-01-01 00:00:00"格式，返回 NaN

```js
var time = "2000-01-01 00:00:00";
time = time.replace(/\-/g, "/"); // 将时间格式的'-'转成'/'形式，兼容 iOS
```

- iOS 中 active 点击态样式不生效

```js
document.body.addEventListener('ontouchstart', function(){})
```

### 其他

- 滚动穿透，当有 fixed 的背景遮罩层时，滑动屏幕会引起页面的滚动

```js
body.modal-open {
  position: fixed;
}
```

如果只设置这个样式，会导致背景遮罩层弹出时滚动位置丢失，还需要用 js 来保存滚动条的位置

```js
if (showModal) {
  let  scrollTop = document.body.scrollTop || document.documentElement.scrollTop;
  document.body.classList.add('modal-open');
  document.body.style.top = -scrollTop +'px';
} else {
  document.body.classList.remove('modal-open');
  window.scrollTo(0,this.scrollTop);
}
```

- 输入框被键盘挡住问题

```js
window.addEventListener('resize', function() {
  if (
    document.activeElement.tagName === 'INPUT' ||
    document.activeElement.tagName === 'TEXTAREA'
  ) {
    window.setTimeout(function() {
      if ('scrollIntoView' in document.activeElement) {
        document.activeElement.scrollIntoView();
      } else {
        document.activeElement.scrollIntoViewIfNeeded();
      }
    }, 0);
  }
});
```

- android 加载 h5 页面部分机型滑动时卡顿回弹

解决办法：开启硬件加速

```java
webView.setLayerType(View.LAYER_TYPE_HARDWARE,null);
```

不定时更新。
