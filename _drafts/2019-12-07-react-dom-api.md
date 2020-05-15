---
layout: post
title: ReactDOM 的三个基本方法
categories: [JavaScript, React]
description: ReactDOM 的三个基本方法
keywords: React, ReactDOM
---

### ReactDOM 的三个基本方法

我们在使用react开发网页时，会下载两个包，一个是react，一个是react-dom，其中react包是react的核心代码，react-dom则是React剥离出的涉及DOM操作的部分。

react的核心思想是虚拟DOM，react包含了生成虚拟DOM的函数react.createElement，及Component类。当我们自己封装组件时，就需要继承Component类，才能使用生命周期函数等。而react-dom包的核心功能就是把这些虚拟DOM渲染到文档中变成实际DOM。

react-dom主要包含三个API：findDOMNode、unmountComponentAtNode 和 render。下面按触发先后顺序进行介绍。

1、render
render用于将React渲染的虚拟DOM渲染到浏览器DOM，一般在顶层组件使用。该方法把元素挂载到 container 中，并且返回 element 的实例（即 refs 引用），如果是无状态组件，render 会返回 null。当组件装载完毕时，callback 就会被调用。其语法为：


`render(ReactElement element,DOMElement container,[function callback])`

例如：

```jsx
import ReactDOM from 'react-dom';

ReactDOM.render(<Example />, document.getElementById('root'));
```

2、findDOMNode
findDOMNode用于获取真正的DOM元素，以便对DOM节点进行操作。

在此之前，首先要知道：在React中，虚拟DOM真正被添加到HTML中转变为真实DOM是在组件挂载（render()）后，故而我们可以在componentDidMount和componentDidUpdate这两个方法中获取。示例如下：

```jsx
import { findDOMNode } from 'react-dom';

<Example ref={ node=>{ this.node = node} }> // 利用ref获取Example组件的实例

const dom = findDOMNode(this.node); // 通过findDOMNode获取实例对应的真实DOM
```

注意：当涉及复杂操作时，还有很多元素DOM API可用，然而DOM操作会对性能产生很大影响，所以，应当尽量减少DOM操作。

3、unmountComponentAtNode
unmountComponentAtNode用于执行卸载操作，执行在componentWillUnmount之前。


```jsx
let node = null
const Loading = () => {
    <div>loading</div>
}

export const show = () => {
    node = document.createElement('div')
    document.appentChild(node)
    ReactDOM.render(<Loading/>, node)
}
export const hide = () => {
    if (node) {
        ReactDOM.unmountComponentAtNode(node)
        document.removeChild(node)
    }
}
export default { hide, show }
```
