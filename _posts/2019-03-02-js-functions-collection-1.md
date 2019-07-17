---
layout: post
title: JavaScript 可能会用到的工具函数（一）
categories: JavaScript
description: JavaScript 可能会用到的工具函数
keywords: JavaScript
---

### **代码来源于网络，我只是一个搬运工。**

### 字符串长度截取

<!--more-->
```js
function cutstr(str, len) {
  var temp,
      icount = 0,
      patrn = /[^\x00-\xff]/,
      strre = "";
  for (var i = 0; i < str.length; i++) {
    if (icount < len) {
      temp = str.substr(i, 1);
      if (patrn.exec(temp) == null) {
        icount = icount + 1;
      } else {
        icount = icount + 2;
      }
      strre += temp;
    } else {
      break;
    }
  }
  return strre;
}
```

### 替换全部

```js
String.prototype.replaceAll = function(s1, s2) {
  return this.replace(new RegExp(s1, "gm"), s2);
}
```

### 清除空格

```js
String.prototype.trim = function() {
  var reExtraSpace = /^\s*(.*?)\s+$/;
  return this.replace(reExtraSpace, "$1");
}
```

### 清除左空格/右空格

```js
function ltrim(s){ return s.replace( /^(\s*|　*)/, ""); } 
function rtrim(s){ return s.replace( /(\s*|　*)$/, ""); }
```

### 判断是否以某个字符串开头

```js
String.prototype.startWith = function(s) {
  return this.indexOf(s) == 0;
}
```

### 判断是否以某个字符串结束

```js
String.prototype.endWith = function(s) {
  var d = this.length - s.length;
  return (d >= 0 && this.lastIndexOf(s) == d);
}
```

### 清除相同的数组

```js
String.prototype.unique = function(){
  var x = this.split(/[\r\n]+/);
  var y = '';
  for(var i = 0; i < x.length; i++){
    if (!new RegExp("^"+x.replace(/([^\w])/ig,"\\$1")+"$","igm").test(y)) {
      y += x + "\r\n";
    }
  }
  return y;
};
```

### 按字母排序，对每行进行数组排序

```js
function SetSort(){
  var text = K1.value.split(/[\r\n]/).sort().join("\r\n");//顺序
  var test = K1.value.split(/[\r\n]/).sort().reverse().join("\r\n");//反序
  K1.value = K1.value != text ? text : test;
}
```

### 字符串反序

```js
function IsReverse(text){
  return text.split('').reverse().join('');
}
```

### 转义特殊字符为 html 实体

```js
HtmlEncode: function(str){
  return str.replace(/&/g, '&amp;').replace(/\"/g, '&quot;').replace(/</g, '&lt;').replace(/>/g, '&gt;').replace(/'/g, '&apos;');
}
```

### 判断是否为数字类型

```js
function isDigit(value) {
  var patrn = /^[0-9]*$/;
  if (patrn.exec(value) == null || value == "") {
    return false;
  }
  return true;
}
```

### 时间日期格式转换

```js
Date.prototype.Format = function(formatStr) {
  var str = formatStr;
  var Week = ['日', '一', '二', '三', '四', '五', '六'];
  str = str.replace(/yyyy|YYYY/, this.getFullYear());
  str = str.replace(/yy|YY/, (this.getYear() % 100) > 9 ? (this.getYear() % 100).toString() : '0' + (this.getYear() % 100));
  str = str.replace(/MM/, (this.getMonth() + 1) > 9 ? (this.getMonth() + 1).toString() : '0' + (this.getMonth() + 1));
  str = str.replace(/M/g, (this.getMonth() + 1));
  str = str.replace(/w|W/g, Week[this.getDay()]);
  str = str.replace(/dd|DD/, this.getDate() > 9 ? this.getDate().toString() : '0' + this.getDate());
  str = str.replace(/d|D/g, this.getDate());
  str = str.replace(/hh|HH/, this.getHours() > 9 ? this.getHours().toString() : '0' + this.getHours());
  str = str.replace(/h|H/g, this.getHours());
  str = str.replace(/mm/, this.getMinutes() > 9 ? this.getMinutes().toString() : '0' + this.getMinutes());
  str = str.replace(/m/g, this.getMinutes());
  str = str.replace(/ss|SS/, this.getSeconds() > 9 ? this.getSeconds().toString() : '0' + this.getSeconds());
  str = str.replace(/s|S/g, this.getSeconds());
  return str
}
```

### 日期格式化函数+调用方法

```js
Date.prototype.format = function(format){
  var o = {
    "M+": this.getMonth()+1, //month
    "d+": this.getDate(),    //day
    "h+": this.getHours(),   //hour
    "m+": this.getMinutes(), //minute
    "s+": this.getSeconds(), //second
    "q+": Math.floor((this.getMonth()+3)/3),  //quarter
    "S": this.getMilliseconds() //millisecond
  };
  if(/(y+)/.test(format)) {
    format = format.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
  }
  for(var k in o){
    if(new RegExp("("+ k +")").test(format)) {
      format = format.replace(RegExp.$1, RegExp.$1.length == 1 ? o[k] : ("00" + o[k]).substr(("" + o[k]).length));
    }
  }
  return format;
}
console.log(new Date().format("yyyy-MM-dd hh:mm:ss"));
```

### 时间个性化输出功能

```js
/*
1、< 60s, 显示为“刚刚”
2、>= 1min && < 60 min, 显示与当前时间差“XX分钟前”
3、>= 60min && < 1day, 显示与当前时间差“今天 XX:XX”
4、>= 1day && < 1year, 显示日期“XX月XX日 XX:XX”
5、>= 1year, 显示具体日期“XXXX年XX月XX日 XX:XX”
*/
function timeFormat(time){
  var date = new Date(time),
      curDate = new Date(),
      year = date.getFullYear(),
      month = date.getMonth() + 10,
      day = date.getDate(),
      hour = date.getHours(),
      minute = date.getMinutes(),
      curYear = curDate.getFullYear(),
      curHour = curDate.getHours(),
      timeStr;

  if (year < curYear) {
    timeStr = year +'年'+ month +'月'+ day +'日 '+ hour +':'+ minute;
  } else {
    var pastTime = curDate - date,
        pastH = pastTime/3600000;

    if (pastH > curHour) {
      timeStr = month +'月'+ day +'日 '+ hour +':'+ minute;
    } else if (pastH >= 1) {
      timeStr = '今天 ' + hour +':'+ minute +'分';
    } else {
      var pastM = curDate.getMinutes() - minute;
      if (pastM > 1) {
        timeStr = pastM +'分钟前';
      } else {
        timeStr = '刚刚';
      }
    }
  }
  return timeStr;
}
```

### 随机数时间戳

```js
function uniqueId(){
  var a = Math.random, b = parseInt;
  return Number(new Date()).toString()+b(10*a())+b(10*a())+b(10*a());
}
```

### 实现 base64 解码

```js
function base64_decode(data){
  var b64 = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";
  var o1, o2, o3, h1, h2, h3, h4, bits, i = 0,ac = 0,dec = "",tmp_arr = [];
  if (!data) return data;
  data += '';
  do {
    h1 = b64.indexOf(data.charAt(i++));
    h2 = b64.indexOf(data.charAt(i++));
    h3 = b64.indexOf(data.charAt(i++));
    h4 = b64.indexOf(data.charAt(i++));
    bits = h1 << 18 | h2 << 12 | h3 << 6 | h4;
    o1 = bits >> 16 & 0xff;
    o2 = bits >> 8 & 0xff;
    o3 = bits & 0xff;
    if (h3 == 64) {
      tmp_arr[ac++] = String.fromCharCode(o1);
    } else if (h4 == 64) {
      tmp_arr[ac++] = String.fromCharCode(o1, o2);
    } else {
      tmp_arr[ac++] = String.fromCharCode(o1, o2, o3);
    }
  } while (i < data.length);
  dec = tmp_arr.join('');
  dec = utf8_decode(dec);
  return dec;
}
```

### 实现 utf8 解码

```js
function utf8_decode(str_data){
  var tmp_arr = [],i = 0,ac = 0,c1 = 0,c2 = 0,c3 = 0;str_data += '';
  while (i < str_data.length) {
    c1 = str_data.charCodeAt(i);
    if (c1 < 128) {
      tmp_arr[ac++] = String.fromCharCode(c1);
      i++;
    } else if (c1 > 191 && c1 < 224) {       
      c2 = str_data.charCodeAt(i + 1);
      tmp_arr[ac++] = String.fromCharCode(((c1 & 31) << 6) | (c2 & 63));
      i += 2;
    } else {
      c2 = str_data.charCodeAt(i + 1);
      c3 = str_data.charCodeAt(i + 2);
      tmp_arr[ac++] = String.fromCharCode(((c1 & 15) << 12) | ((c2 & 63) << 6) | (c3 & 63));
      i += 3;
    }
  } 
  return tmp_arr.join('');
}
```

### 半角转换为全角函数

```js
function ToDBC(str){
  var result = '';
  for(var i=0; i < str.length; i++){
    code = str.charCodeAt(i);
    if (code >= 33 && code <= 126) {
      result += String.fromCharCode(str.charCodeAt(i) + 65248);
    } else if (code == 32) {
      result += String.fromCharCode(str.charCodeAt(i) + 12288 - 32);
    } else {
      result += str.charAt(i);
    }
  }
  return result;
}
```

### 全角转换为半角函数

```js
function ToCDB(str){
  var result = '';
  for(var i=0; i < str.length; i++){
    code = str.charCodeAt(i);
    if (code >= 65281 && code <= 65374) {
      result += String.fromCharCode(str.charCodeAt(i) - 65248);
    } else if (code == 12288) {
      result += String.fromCharCode(str.charCodeAt(i) - 12288 + 32);
    } else {
      result += str.charAt(i);
    }
  }
  return result;
}
```

### 全角半角转换

```js
// iCase: 0全到半，1半到全，其他不转化
function chgCase(sStr, iCase){
  if (typeof sStr != "string" || sStr.length <= 0 || !(iCase === 0 || iCase == 1)) {
    return sStr;
  }
  var i, oRs=[], iCode;
  if (iCase) {/*半->全*/
    for (i=0; i<sStr.length;i+=1) {
      iCode = sStr.charCodeAt(i);
      if (iCode == 32) {
        iCode = 12288;
      } else if (iCode < 127) {
        iCode += 65248;
      }
      oRs.push(String.fromCharCode(iCode));
    }
  } else {/*全->半*/
    for (i=0; i<sStr.length;i+=1) {
      iCode = sStr.charCodeAt(i);
      if (iCode == 12288) {
        iCode = 32;
      } else if (iCode > 65280 && iCode < 65375) {
        iCode -= 65248;
      }
      oRs.push(String.fromCharCode(iCode));
    }
  }
  return oRs.join("");
}
```
