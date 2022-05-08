---
layout: post
title: 使用 we-cropper 实现微信小程序头像裁剪并上传
categories: [Mini Program, JavaScript]
description: 使用 we-cropper 实现微信小程序头像裁剪并上传
keywords: [Mini Program, JavaScript]
---

工作中需要在微信小程序中实现头像裁剪并上传的功能，图片上传配合后端可以实现，图片裁剪则需要在小程序中实现。在 Github 上找到一个微信小程序图片裁剪工具 [we-cropper](https://github.com/we-plugin/we-cropper)，在这里记录一下使用步骤。

### 引用插件资源

- 克隆项目或者直接下载 zip 包

```
git clone https://github.com/we-plugin/we-cropper.git
```

dist 目录下是我们要使用的资源。

```
we-cropper.js
we-cropper.min.js
we-cropper.wxml
```

- WXML

首先需要在 WXML 结构中置入一个 canvas 作为裁剪图片的容器，并绑定相应的事件。

这里直接使用项目 `/dist/we-cropper.wxml` 里的结构。

canvas 的宽高需要和 we-cropper 构造器参数中的保持一致。

```html
<template name="we-cropper">
  <canvas
    class="cropper"
    disable-scroll="true"
    bindtouchstart="touchStart"
    bindtouchmove="touchMove"
    bindtouchend="touchEnd"
    style="width:{{width}}px;height:{{height}}px;background-color: rgba(0, 0, 0, 0.8)"
    canvas-id="{{id}}">
  </canvas>
  <canvas
    class="cropper"
    disable-scroll="true"
    style="position: fixed; top: -{{width * pixelRatio}}px; left: -{{height * pixelRatio}}px; width:{{width * pixelRatio}}px;height:{{height * pixelRatio}}px;"
    canvas-id="{{targetId}}">
  </canvas>
</template>
```

- 引入 WeCropper 插件

引用 `/dist/we-cropper.js` 或 `/dist/we-cropper.min.js`

```js
import WeCropper from 'we-cropper.min.js'
// import WeCropper from 'we-cropper.js'

const device = wx.getSystemInfoSync() // 获取设备信息
const width = device.windowWidth // 示例为一个与屏幕等宽的正方形裁剪框
const height = device.windowHeight

Page({
  data: {
    cropperOpt: {
      src: '', // 图片的路径
      id: 'cropper', // 用于手势操作的canvas组件标识符
      targetId: 'targetCropper', // 用于用于生成截图的canvas组件标识符
      pixelRatio: device.pixelRatio, // 传入设备像素比
      width,  // 画布宽度
      height, // 画布高度
      scale: 2.5, // 最大缩放倍数
      zoom: 8, // 缩放系数
      cut: {
        x: (width - 300) / 2, // 裁剪框x轴起点
        y: (width - 300) / 2, // 裁剪框y轴期起点
        width: 300, // 裁剪框宽度
        height: 300 // 裁剪框高度
      },
      boundStyle: {
        mask: 'rgba(0, 0, 0, 0.8)'
      }
    }
  }
})
```

### WeCropper 实例化

- 在 onload 中实例化

```js
this.cropper = new WeCropper(this.cropperOpt).on('ready', ctx => {

}).on('beforeImageLoad', ctx => {

}).on('imageLoad', ctx => {

})
```

- 事件绑定

```js
touchStart(e) {
  this.cropper.touchStart({
    touches: e.touches.filter(i => i.x !== undefined)
  })
}

touchMove(e) {
  this.cropper.touchMove({
    touches: e.touches.filter(i => i.x !== undefined)
  })
}

touchEnd(e) {
  this.cropper.touchEnd(e)
}
```

`touches: e.touches.filter(i => i.x !== undefined)` 是为了解决[移动图片后黑屏](https://github.com/we-plugin/we-cropper/issues/41)的问题。

- 图片载入

实例化时在参数 `this.cropperOpt` 中加入构造参数 `src`，当检测到构造参数 `src` 有值时，会尝试通过 `wx.getImageInfo` 获取图片信息。

也可以在实例化后通过 `this.cropper.pushOrign(src)` 方法载入图片，当检测到通过 `pushOrign` 方法传入的值不为空时，会尝试通过 `wx.getImageInfo` 获取图片信息。

`src` 是图片的路径，可以是相对路径、临时文件路径、存储文件路径、网络图片路径。

### 生成裁剪图片

调整好要裁剪的图片，点击完成按钮，调用 `getCropperImage` 方法，得到裁剪后图片的临时路径，然后与后端配合进行图片上传就可以了。

```js
getCropperImage() {
  this.wecropper.getCropperImage({fileType: 'jpg'}).then(tempFilePath => {
    // tempFilePath 为裁剪后的图片临时路径
    if (tempFilePath) {
      wx.previewImage({
        current: tempFilePath,
        urls: [tempFilePath]
      })
    } else {
      console.log('获取图片地址失败，请稍后重试')
    }
  })
}
```

关于微信小程序上传文件的 API `wx.uploadFile`，详见[微信官方文档](https://developers.weixin.qq.com/miniprogram/dev/api/network/upload/wx.uploadFile.html)。

以上。
