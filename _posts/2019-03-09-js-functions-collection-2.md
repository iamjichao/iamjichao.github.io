---
layout: post
title: JavaScript 可能会用到的工具函数（二）
categories: JavaScript
description: JavaScript 可能会用到的工具函数
keywords: JavaScript
---

### **代码来源于网络，我只是一个搬运工。**

### 判断是哪种类型的浏览器

```js
WhichBrowser: function(){
  var userAgent = navigator.userAgent;

  var isOpera = userAgent.indexOf("Opera") > -1;
  var isIE = userAgent.indexOf("compatible") > -1 && userAgent.indexOf("MSIE") > -1 && !isOpera;
  var isFF = userAgent.indexOf("Firefox") > -1;
  var isCH = userAgent.indexOf("Chrome") > -1;
  var isSafari = userAgent.indexOf("Safari") > -1;

  if (isIE) {
    var IE5 = IE55 = IE6 = IE7 = IE8 = false;
    var reIE = new RegExp("MSIE (\\d+\\.\\d+);");
    reIE.test(userAgent);
    var fIEVersion = parseFloat(RegExp["$1"]);
    IE55 = fIEVersion == 5.5;
    IE6 = fIEVersion == 6.0;
    IE7 = fIEVersion == 7.0;
    IE8 = fIEVersion == 8.0;
    if (IE55) return "IE55";
    if (IE6) return "IE6";
    if (IE7) return "IE7";
    if (IE8) return "IE8";
  }

  if (isFF) return "Firefox";
  if (isCH) return "Chrome";
  if (isOpera) return "Opera";
  if (isSafari) return "Safari";
}
```

### 判断是否移动设备

```js
function isMobile(){
  if (typeof this._isMobile === 'boolean') {
    return this._isMobile;
  }
  var screenWidth = this.getScreenWidth();
  var fixViewPortsExperiment = rendererModel.runningExperiments.FixViewport ||rendererModel.runningExperiments.fixviewport;
  var fixViewPortsExperimentRunning = fixViewPortsExperiment && (fixViewPortsExperiment.toLowerCase() === "new");
  if (!fixViewPortsExperiment) {
    if (!this.isAppleMobileDevice()) {
      screenWidth = screenWidth/window.devicePixelRatio;
    }
  }
  var isMobileScreenSize = screenWidth < 600;
  var isMobileUserAgent = false;
  this._isMobile = isMobileScreenSize && this.isTouchScreen();
  return this._isMobile;
}
```

### 判断是否移动设备访问

```js
function isMobileUserAgent(){
  return (/iphone|ipod|android.*mobile|windows.*phone|blackberry.*mobile/i.test(window.navigator.userAgent.toLowerCase()));
}
```

### 判断是否苹果移动设备访问

```js
function isAppleMobileDevice(){
  return (/iphone|ipod|ipad|Macintosh/i.test(navigator.userAgent.toLowerCase()));
}
```

### 判断是否安卓移动设备访问

```js
function isAndroidMobileDevice(){
  return (/android/i.test(navigator.userAgent.toLowerCase()));
}
```

### 判断是否 touch 屏幕

```js
function isTouchScreen(){
  return (('ontouchstart' in window) || window.DocumentTouch && document instanceof DocumentTouch);
}
```

### 判断是否打开视窗

```js
function isViewportOpen() {
  return !!document.getElementById('wixMobileViewport');
}
```

### 获取移动设备初始化大小

```js
function getInitZoom(){
  if(!this._initZoom){
    var screenWidth = Math.min(screen.height, screen.width);
    if(this.isAndroidMobileDevice() && !this.isNewChromeOnAndroid()){
      screenWidth = screenWidth / window.devicePixelRatio;
    }
    this._initZoom = screenWidth / document.body.offsetWidth;
  }
  return this._initZoom;
}
```

### 取移动设备最大化大小

```js
function getZoom(){
  var screenWidth = (Math.abs(window.orientation) === 90) ? Math.max(screen.height, screen.width) : Math.min(screen.height, screen.width);
  if(this.isAndroidMobileDevice() && !this.isNewChromeOnAndroid()){
    screenWidth = screenWidth/window.devicePixelRatio;
  }
  var FixViewPortsExperiment = rendererModel.runningExperiments.FixViewport || rendererModel.runningExperiments.fixviewport;
  var FixViewPortsExperimentRunning = FixViewPortsExperiment && (FixViewPortsExperiment === "New" || FixViewPortsExperiment === "new");
  if(FixViewPortsExperimentRunning){
    return screenWidth / window.innerWidth;
  }
  return screenWidth / document.body.offsetWidth;
}
```

### 取移动设备屏幕宽度

```js
function getScreenWidth(){
  var smallerSide = Math.min(screen.width, screen.height);
  var fixViewPortsExperiment = rendererModel.runningExperiments.FixViewport || rendererModel.runningExperiments.fixviewport;
  var fixViewPortsExperimentRunning = fixViewPortsExperiment && (fixViewPortsExperiment.toLowerCase() === "new");
  if(fixViewPortsExperiment){
    if(this.isAndroidMobileDevice() && !this.isNewChromeOnAndroid()){
      smallerSide = smallerSide/window.devicePixelRatio;
    }
  }
  return smallerSide;
}
```

### 取页面 scrollTop

```js
function getPageScrollTop(){
  var a = document;
  return a.documentElement.scrollTop || a.body.scrollTop;
}
```

### 取页面 scrollLeft

```js
function getPageScrollLeft(){
  var a = document;
  return a.documentElement.scrollLeft || a.body.scrollLeft;
}
```

### 取页面高度

```js
function getPageHeight(){
  var g = document,
      a = g.body,
      f = g.documentElement,
      d = g.compatMode == "BackCompat" ? a : g.documentElement;
  return Math.max(f.scrollHeight, a.scrollHeight, d.clientHeight);
}
```

### 取页面可视高度

```js
function getPageViewHeight() {
  var d = document,
      a = d.compatMode == "BackCompat" ? d.body : d.documentElement;
  return a.clientHeight;
}
```

### 取页面宽度

```js
function getPageWidth(){
  var g = document,
      a = g.body,
      f = g.documentElement,
      d = g.compatMode == "BackCompat" ? a : g.documentElement;
  return Math.max(f.scrollWidth, a.scrollWidth, d.clientWidth);
}
```

### 取页面可视宽度

```js
function getPageViewWidth(){
  var d = document,
      a = d.compatMode == "BackCompat" ? d.body : d.documentElement;
  return a.clientWidth;
}
```

### 获取网页被卷去的位置

```js
function getScrollXY() {
  return document.body.scrollTop ? {
    x: document.body.scrollLeft,
    y: document.body.scrollTop
  } : {
    x: document.documentElement.scrollLeft,
    y: document.documentElement.scrollTop
  }
}
```

### 获取窗体可见范围的宽与高

```js
function getViewSize(){
  var de = document.documentElement;
  var db = document.body;
  var viewW = de.clientWidth == 0 ? db.clientWidth : de.clientWidth;
  var viewH = de.clientHeight == 0 ? db.clientHeight : de.clientHeight;
  return Array(viewW, viewH);
}
```

### 解决 offsetX 兼容性问题

```js
// 针对火狐不支持offsetX/Y
function getOffset(e){
  var target = e.target, // 当前触发的目标对象
      eventCoord,
      pageCoord,
      offsetCoord;

  // 计算当前触发元素到文档的距离
  pageCoord = getPageCoord(target);

  // 计算光标到文档的距离
  eventCoord = {
    X: window.pageXOffset + e.clientX,
    Y: window.pageYOffset + e.clientY
  };

  // 相减获取光标到第一个定位的父元素的坐标
  offsetCoord = {
    X: eventCoord.X - pageCoord.X,
    Y: eventCoord.Y - pageCoord.Y
  };
  return offsetCoord;
}
 
function getPageCoord(element){
  var coord = { X : 0, Y : 0 };
  // 计算从当前触发元素到根节点为止，
  // 各级 offsetParent 元素的 offsetLeft 或 offsetTop 值之和
  while (element) {
    coord.X += element.offsetLeft;
    coord.Y += element.offsetTop;
    element = element.offsetParent;
  }
  return coord;
}
```

### 返回顶部的通用方法

```js
function backTop(btnId) {
  var btn = document.getElementById(btnId);
  var d = document.documentElement;
  var b = document.body;
  window.onscroll = set;
  btn.style.display = "none";
  btn.onclick = function() {
    btn.style.display = "none";
    window.onscroll = null;
    this.timer = setInterval(function() {
      d.scrollTop -= Math.ceil((d.scrollTop + b.scrollTop) * 0.1);
      b.scrollTop -= Math.ceil((d.scrollTop + b.scrollTop) * 0.1);
      if ((d.scrollTop + b.scrollTop) == 0) clearInterval(btn.timer, window.onscroll = set);
    }, 10);
  };
  function set() {
    btn.style.display = (d.scrollTop + b.scrollTop > 100) ? "block" : "none"
  }
};
backTop("goTop");
```

### resize 操作

```js
(function(){
  var fn = function(){
    var w = document.documentElement ? document.documentElement.clientWidth : document.body.clientWidth,
        r = 1255,
        b = Element.extend(document.body),
        classname = b.className;
    if (w < r) {
      //当窗体的宽度小于1255的时候执行相应的操作
    } else {
      //当窗体的宽度大于1255的时候执行相应的操作
    }
  }
  if (window.addEventListener) {
    window.addEventListener('resize', function(){ fn(); });
  } else if (window.attachEvent) {
    window.attachEvent('onresize', function(){ fn(); });
  }
  fn();
})();
```

### 打开一个窗口通用方法

```js
function openWindow(url,windowName,width,height){
  var x = parseInt(screen.width / 2.0) - (width / 2.0); 
  var y = parseInt(screen.height / 2.0) - (height / 2.0);
  var isMSIE= (navigator.appName == "Microsoft Internet Explorer");
  if (isMSIE) {
    var p = "resizable=1,location=no,scrollbars=no,width=";
    p = p + width;
    p = p + ",height=";
    p = p + height;
    p = p + ",left=";
    p = p + x;
    p = p + ",top=";
    p = p + y;
    retval = window.open(url, windowName, p);
  } else {
    var win = window.open(url, "ZyiisPopup", "top=" + y + ",left=" + x + ",scrollbars=" + scrollbars + ",dialog=yes,modal=yes,width=" + width + ",height=" + height + ",resizable=no" );
    eval("try { win.resizeTo(width, height); } catch(e) { }");
    win.focus();
  }
}
```
