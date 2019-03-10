---
layout: post
title: 从零开始创建 react+webpack 项目
categories: [JS, React, webpack]
description: 从零开始创建 react+webpack 项目
keywords: JS, React, webpack
---

React 是 Facebook 开发的一款用于构建用户界面的 JavaScript 库。

webpack 是模块打包的工具，执行编译时，webpack 会分析项目结构，找到 JavaScript 模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript 等），并将其转换和打包为合适的格式供浏览器使用。

### 准备工作
---

安装 node.js。

### 项目初始化
---

- 创建项目文件夹 react-webpack，在项目目录下执行：

```
npm init -y
```

- 执行后会生成一个 package.json 文件，package.json 用于管理项目里的依赖包，记录我们在 node-modules 中安装的模块包的版本信息等。


### 安装项目依赖
---

- 首先安装 react 和 webpack 依赖，在项目目录下执行：

```
npm install react react-dom --save
npm install webpack --save-dev
```

- 因为接下来要使用 es6 的语法写组件，所以需要安装用于解析 es6 的依赖。在项目目录下执行：

```
npm install babel-core babel-loader babel-preset-es2015 babel-preset-react --save
```

**babel-core** 一些代码需要调用 babel 的 API 进行转码

**babel-loader** 转化 es6 代码

**babel-preset-es2015** 解析 es6

**babel-preset-react** 解析 jsx

- 安装 css 和 html 相关的依赖，在项目目录下执行：

```
npm install css-loader style-loader --save
npm install html-webpack-plugin --save
```

**css-loader** 在 webpack 中，通过 css-loader 可以实现在 js 文件中通过 require 方式引入 css

**style-loader** 在 html 中通过 style 方式嵌入 css

**less-loader/less** 如果需要在 js 中通过 require 引入 less 文件，则安装这两个包

**html-webpack-plugin** 生成 html 文件

经过以上 3 个步骤，项目目录如下：

![image](images/posts/react-webpack.png)

### 创建代码基本结构
---

- 在项目目录下创建 src 文件夹，用于存放项目代码。

- 在 src 下分别创建 common，components 和 pages 文件夹，分别存放公共代码，组件代码和页面代码。

- 在 src 下新建 index.jsx 文件，代码如下：

```
import React from 'react';
import ReactDOM from 'react-dom';
import './common/common.css';
import App from './components/App';

ReactDOM.render(<App />, document.getElementById('app'));
```

- 在 src 下新建 index.html 文件，html 结构如下：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <title>React!</title>
  </head>
  <body>
    <div id="app"></div>
  </body>
</html>
```

### 添加本地访问方式
---

- 安装 webpack-dev-server，在项目目录下执行：

```
npm install webpack-dev-server --save
```

- 然后在 package.json 的 scripts 中设置启动本地服务的脚本，如：

```
set NODE_ENV=dev&&webpack-dev-server --inline --progress --colors --host 0.0.0.0 --port 8080
```

### 配置 webpack.config.js
---

webpack.config.js 文件主要是让 webpack 知道如何处理我们的项目目录结构，根据我们设置好的文件路径进行打包，最后生成被浏览器支持的 js 和 html 文件。

### 启动本地服务，在浏览器中打开
---

根据 package.json 中的启动脚本启动本地服务器，然后在浏览器中访问 http://localhost:8080/ 就可以看到项目内容了。
