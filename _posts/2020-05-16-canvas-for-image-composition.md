---
layout: post
title: 使用 canvas 实现图片的合成
categories: [JavaScript, HTML]
description: 使用 canvas 实现图片的合成
keywords: [canvas]
---

工作中需要实现这样一个需求：根据不同的推荐链接生成不同的二维码，将二维码和一张静态的图片合成一张完整的海报，用于不同推荐人的推广。

### 思路

先用 [qrcode.js](https://github.com/davidshimjs/qrcodejs) 生成对应链接的二维码图片，然后用一个画布 [canvas](https://www.runoob.com/html/html5-canvas.html) 把静态图片画上，再把生成的二维码画到静态图片上的指定位置，最后将整个 canvas 转成图片并下载。

### 实现

- 首先用 qrcode.js 生成二维码，代码如下。

```html
<div id='qrcode'></div>
```

```js
const qrcode = new QRCode(document.getElementById('qrcode'), 'https://fehub.net/');
```

- 创建一个画布，将静态图片画上去，代码如下。

```html
<div id='canbox'>
  <canvas id='myCanvas'></canvas>
</div>
```

```js
import CARD from '../assets/card.jpg';
const width = 1000;
const height = 1000;
const c = document.getElementById('myCanvas');
c.width = width;
c.height = height;
const ctx = c.getContext('2d');
// 首先画上背景图
const img = new Image();
img.src = CARD;
// img.setAttribute('crossOrigin', 'anonymous');
// 必须等待图片加载完成
img.onload = () => {
  ctx.drawImage(img, 0, 0, width, height);
}
```

这里有两点要注意：

1. 由于 canvas 的绘制需要的是已经加载完成的图片，我们需要确保绘制的素材图片是已经加载完成的，因此我们需要使用 img 的 onload 事件，可以使用 html 中已存在的图片，或者用js创建一个图片对象；
2. 图片加载并绘制涉及了图片的跨域问题，因此如果是一张在线的图片，需要在图片服务器上设置跨域头，并且在前端加载图片之前将 img 标签的 crossOrigin 属性设置为 anonymous，否则绘制到画布的时候会报跨域的错误。这里我用的本地资源，所以没有跨域问题，不需要设置 crossOrigin 属性。关于 crossOrigin 可以参考这篇 [文章](http://ilucas.me/2017/04/19/crossorign-attribute-img-tag/)。

- 将生成的二维码画到静态图片指定位置

```js
img.onload = () => {
  ctx.drawImage(img, 0, 0, width, height);
  // 直接使用生成的 base64 图片
  const qrImg = document.getElementById("qrcode").getElementsByTagName("img")[0];
  // 把 canvas 转成图片
  // const canvas = document.getElementById('qrcode').getElementsByTagName('canvas')[0];
  // const data = canvas.toDataURL('image/jpg');
  // const qrImg = new Image();
  // qrImg.src = data;
  // qrImg.setAttribute('crossOrigin', 'Anonymous');
  // 把二维码图片画上
  ctx.drawImage(qrImg, 400, 400, 200, 200);
}
```

这里要注意：

qrcode.js 会在容器内生成一个 canvas 和一个 img，我们可以直接把这个 base64 图片画到画布上，也可以把生成的 canvas 转成图片，再画到画布上。

- 将整个画布转成图片并下载

```js
img.onload = () => {
  // 画上背景图
  ctx.drawImage(img, 0, 0, width, height);
  // 画上二维码
  const qrImg = document.getElementById("qrcode").getElementsByTagName("img")[0];
  // const canvas = document.getElementById('qrcode').getElementsByTagName('canvas')[0];
  // const data = canvas.toDataURL('image/jpg');
  // const qrImg = new Image();
  // qrImg.src = data;
  // qrImg.setAttribute('crossOrigin', 'Anonymous');
  ctx.drawImage(qrImg, 400, 400, 200, 200);
  // 画布转图片
  const myCanvas = document.getElementById('myCanvas');
  const src = myCanvas.toDataURL('image/jpg');
  const cardImg = new Image();
  cardImg.src = src;
  // cardImg.setAttribute('crossOrigin', 'Anonymous');
  const filename = 'iamjichao.net';
  saveFile(src, filename);
}

function saveFile(data, filename) {
  const save_link = document.createElementNS('http://www.w3.org/1999/xhtml', 'a');
  save_link.href = data;
  save_link.download = filename;
  const event = document.createEvent('MouseEvents');
  event.initMouseEvent('click', true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
  save_link.dispatchEvent(event);
};
```

以上。
