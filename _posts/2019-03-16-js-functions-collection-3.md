---
layout: post
title: JavaScript 可能会用到的工具函数（三）
categories: JavaScript
description: JavaScript 可能会用到的工具函数
keywords: JavaScript
---

### **代码来源于网络，我只是一个搬运工。**

### 提取页面代码中所有网址

```js
var aa = document.documentElement.outerHTML.match(/(url\(|src=|href=)[\"\']*([^\"\'\(\)\<\>\[\] ]+)[\"\'\)]*|(http:\/\/[\w\-\.]+[^\"\'\(\)\<\>\[\] ]+)/ig).join("\r\n").replace(/^(src=|href=|url\()[\"\']*|[\"\'\>\) ]*$/igm,"");
alert(aa);
```

### 获取当前路径

```js
var currentPageUrl = "";
if (typeof this.href === "undefined") {
  currentPageUrl = document.location.toString().toLowerCase();
} else {
  currentPageUrl = this.href.toString().toLowerCase();
}
```

### 判断是否为网址

```js
function IsURL(strUrl) {
  var regular = /^\b(((https?|ftp):\/\/)?[-a-z0-9]+(\.[-a-z0-9]+)*\.(?:com|edu|gov|int|mil|net|org|biz|info|name|museum|asia|coop|aero|[a-z][a-z]|((25[0-5])|(2[0-4]\d)|(1\d\d)|([1-9]\d)|\d))\b(\/[-a-z0-9_:\@&?=+,.!\/~%\$]*)?)$/i
  if (regular.test(strUrl)) {
    return true;
  }
  return false;
}
```

### 检验 url 链接是否有效

```js
function getUrlState(URL){ 
  var xmlhttp = new ActiveXObject("microsoft.xmlhttp"); 
  xmlhttp.Open("GET", URL, false);  
  try {
    xmlhttp.Send();
  } catch(e) {
  } finally {
    var result = xmlhttp.responseText;
    if (result) {
      if (xmlhttp.Status == 200) {
        return true;
      }
      return false;
    }
    return false;
  }
}
```

### 获得 url 中全部参数值

```js
function getUrlParams(){ 
  querystr = window.location.href.split("?")
  if (querystr[1]) {
    GETs = querystr[1].split("&");
    GET = [];
    for (i = 0; i < GETs.length; i++) {
      tmp_arr = GETs[i].split("=");
      key = tmp_arr[0];
      GET[key] = tmp_arr[1];
    }
  }
  return GET;
}
```

### 去掉 url 前缀

```js
function removeUrlPrefix(a){
  a = a.replace(/：/g,":").replace(/．/g,".").replace(/／/g,"/");
  while(trim(a).toLowerCase().indexOf("http://") == 0){
    a = trim(a.replace(/http:\/\//i,""));
  }
  return a;
}
```

### 设置 cookie 值

```js
function setCookie(name, value, Hours) {
  var d = new Date();
  var offset = 8;
  var utc = d.getTime() + (d.getTimezoneOffset() * 60000);
  var nd = utc + (3600000 * offset);
  var exp = new Date(nd);
  exp.setTime(exp.getTime() + Hours * 60 * 60 * 1000);
  document.cookie = name + "=" + escape(value) + ";path=/;expires=" + exp.toGMTString() + ";domain=iamjichao.net;"
}
```

### 获取 cookie 值

```js
function getCookie(name) {
  var arr = document.cookie.match(new RegExp("(^| )" + name + "=([^;]*)(;|$)"));
  if (arr != null) return unescape(arr[2]);
  return null;
}
```

### 加入收藏夹

```js
function AddFavorite(sURL, sTitle) {
  try {
    window.external.addFavorite(sURL, sTitle);
  } catch(e) {
    try {
      window.sidebar.addPanel(sTitle, sURL, "")
    } catch(e) {
      alert("加入收藏失败，请使用Ctrl+D进行添加");
    }
  }
}
```

### 设为首页

```js
function setHomepage() {
  if (document.all) {
    document.body.style.behavior = 'url(#default#homepage)';
    document.body.setHomePage('http://w3cboy.com')
  } else if (window.sidebar) {
    if (window.netscape) {
      try {
        netscape.security.PrivilegeManager.enablePrivilege("UniversalXPConnect")
      } catch(e) {
        alert("该操作被浏览器拒绝，如果想启用该功能，请在地址栏内输入 about:config,然后将项 signed.applets.codebase_principal_support 值该为true")
      }
    }
    var prefs = Components.classes['@mozilla.org/preferences-service;1'].getService(Components.interfaces.nsIPrefBranch);
    prefs.setCharPref('browser.startup.homepage', 'http://w3cboy.com')
  }
}
```

### 动态执行 JavaScript 脚本

```js
function javascript(){
  var K1 = document.getElementById("K1").value;
  try{
    eval(K1.value);
  }catch(e){
    alert(e.message);
  }
}
```

### 动态执行 VBScript 脚本

```js
function vbscript(){
  try{
    var script = document.getElementById("K1").value;
    if (script.trim() == "") return;
    window.execScript('On Error Resume Next \n'+script+'\n If Err.Number<>0 Then \n MsgBox "请输入正确的VBScript脚本!",48,"脚本错误!" \n End If',"vbscript")
  }catch(e){
    alert(e.message);
  }
}
```

### 返回脚本内容

```js
function evalscript(s) {
  if(s.indexOf('<script') == -1) return s;
  var p = /<script[^\>]*?>([^\x00]*?)<\/script>/ig;
  var arr = [];
  while(arr = p.exec(s)) {
    var p1 = /<script[^\>]*?src=\"([^\>]*?)\"[^\>]*?(reload=\"1\")?(?:charset=\"([\w\-]+?)\")?><\/script>/i;
    var arr1 = [];
    arr1 = p1.exec(arr[0]);
    if (arr1) {
      appendscript(arr1[1], '', arr1[2], arr1[3]);
    } else {
      p1 = /<script(.*?)>([^\x00]+?)<\/script>/i;
      arr1 = p1.exec(arr[0]);
      appendscript('', arr1[2], arr1[1].indexOf('reload=') != -1);
    }
  }
  return s;
}
```

### 清除脚本内容

```js
function stripscript(s) {
  return s.replace(/<script.*?>.*?<\/script>/ig, '');
}
```

### 清除 html 代码中的脚本

```js
function clear_script(){
  var K1 = document.getElementById("K1").value;
  K1.value = K1.value.replace(/<script.*?>[\s\S]*?<\/script>|\s+on[a-zA-Z]{3,16}\s?=\s?"[\s\S]*?"|\s+on[a-zA-Z]{3,16}\s?=\s?'[\s\S]*?'|\s+on[a-zA-Z]{3,16}\s?=[^ >]+/ig, "");
}
```

### 动态加载脚本文件

```js
function appendscript(src, text, reload, charset) {
  var id = hash(src + text);
  if(!reload && in_array(id, evalscripts)) return;
  if(reload && $(id)) {
    $(id).parentNode.removeChild($(id));
  }

  evalscripts.push(id);
  var scriptNode = document.createElement("script");
  scriptNode.type = "text/javascript";
  scriptNode.id = id;
  scriptNode.charset = charset ? charset : (BROWSER.firefox ? document.characterSet : document.charset);
  try {
    if(src) {
      scriptNode.src = src;
      scriptNode.onloadDone = false;
      scriptNode.onload = function () {
        scriptNode.onloadDone = true;
        JSLOADED[src] = 1;
      };
      scriptNode.onreadystatechange = function () {
        if((scriptNode.readyState == 'loaded' || scriptNode.readyState == 'complete') && !scriptNode.onloadDone) {
          scriptNode.onloadDone = true;
          JSLOADED[src] = 1;
        }
      };
    } else if (text) {
      scriptNode.text = text;
    }
    document.getElementsByTagName('head')[0].appendChild(scriptNode);
  } catch(e) {}
}
```

### 格式化 CSS 代码

```js
function formatCss(s){//格式化代码
  s = s.replace(/\s*([\{\}\:\;\,])\s*/g, "$1");
  s = s.replace(/;\s*;/g, ";"); //清除连续分号
  s = s.replace(/\,[\s\.\#\d]*{/g, "{");
  s = s.replace(/([^\s])\{([^\s])/g, "$1 {\n\t$2");
  s = s.replace(/([^\s])\}([^\n]*)/g, "$1\n}\n$2");
  s = s.replace(/([^\s]);([^\s\}])/g, "$1;\n\t$2");
  return s;
}
```

### 压缩 CSS 代码

```js
function compressCss (s) {//压缩代码
  s = s.replace(/\/\*(.|\n)*?\*\//g, ""); //删除注释
  s = s.replace(/\s*([\{\}\:\;\,])\s*/g, "$1");
  s = s.replace(/\,[\s\.\#\d]*\{/g, "{"); //容错处理
  s = s.replace(/;\s*;/g, ";"); //清除连续分号
  s = s.match(/^\s*(\S+(\s+\S+)*)\s*$/); //去掉首尾空白
  return (s == null) ? "" : s[1];
}
```

### 加载样式文件

```js
function LoadStyle(url) {
  try {
    document.createStyleSheet(url);
  } catch(e) {
    var cssLink = document.createElement('link');
    cssLink.rel = 'stylesheet';
    cssLink.type = 'text/css';
    cssLink.href = url;
    var head = document.getElementsByTagName('head')[0];
    head.appendChild(cssLink);
  }
}
```

### etElementsByClassName

```js
function getElementsByClassName(name) {
  var tags = document.getElementsByTagName('*') || document.all;
  var els = [];
  for (var i = 0; i < tags.length; i++) {
    if (tags.className) {
      var cs = tags.className.split(' ');
      for (var j = 0; j < cs.length; j++) {
        if (name == cs[j]) {
          els.push(tags);
          break;
        }
      }
    }
  }
  return els;
}
```

### 为元素添加 on 方法

```js
Element.prototype.on = Element.prototype.addEventListener;
NodeList.prototype.on = function (event, fn) {
  []['forEach'].call(this, function (el) {
    el.on(event, fn);
  });
  return this;
};
```

### 为元素添加 trigger 方法

```js
Element.prototype.trigger = function (type, data) {
  var event = document.createEvent('HTMLEvents');
  event.initEvent(type, true, true);
  event.data = data || {};
  event.eventName = type;
  event.target = this;
  this.dispatchEvent(event);
  return this;
};

NodeList.prototype.trigger = function (event) {
  []['forEach'].call(this, function (el) {
    el['trigger'](event);
  });
  return this;
};
```

### 跨浏览器绑定事件

```js
function addEventSamp(obj,evt,fn){ 
  if (!oTarget) return;
  if (obj.addEventListener) { 
    obj.addEventListener(evt, fn, false); 
  } else if (obj.attachEvent) { 
    obj.attachEvent('on'+evt,fn); 
  } else {
    oTarget["on" + sEvtType] = fn;
  }
}
```

### 跨浏览器删除事件

```js
function delEvt(obj,evt,fn){
  if (!obj) return;
  if (obj.addEventListener) {
    obj.addEventListener(evt,fn,false);
  } else if (oTarget.attachEvent){
    obj.attachEvent("on" + evt,fn);
  } else {
    obj["on" + evt] = fn;
  }
}
```

### 确认是否键盘有效输入值

```js
function checkKey(iKey){
  if(iKey == 32 || iKey == 229){return true;}/*空格和异常*/
  if(iKey>47 && iKey < 58){return true;}/*数字*/
  if(iKey>64 && iKey < 91){return true;}/*字母*/
  if(iKey>95 && iKey < 108){return true;}/*数字键盘1*/
  if(iKey>108 && iKey < 112){return true;}/*数字键盘2*/
  if(iKey>185 && iKey < 193){return true;}/*符号1*/
  if(iKey>218 && iKey < 223){return true;}/*符号2*/
  return false;
}
```
