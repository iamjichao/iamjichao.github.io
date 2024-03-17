---
layout: post
title: React 组件生命周期
categories: [React, JavaScript]
description: React 组件生命周期
keywords: [React, 生命周期]
---

## 旧版本生命周期（v16.3 以前）

### 实例化

- 首次实例化

```
getDefaultProps
getInitialState
componentWillMount
render
componentDidMount
```

- 实例化完成后的更新

```
getInitialState
componentWillMount
render
componentDidMount
```

### 存在期

- 组件已存在时的状态改变

```
componentWillReceiveProps
shouldComponentUpdate
componentWillUpdate
render
componentDidUpdate
```

### 销毁 & 清理期

```
componentWillUnmount
```

### 生命周期共提供了 10 个不同的 API

- getDefaultProps

作用于组件类，只调用一次，返回对象用于设置默认的 props，对于引用值，会在实例中共享。

- getInitialState

作用于组件的实例，在实例创建时调用一次，用于初始化每个实例的 state，此时可以访问 this.props。

- componentWillMount

在完成首次渲染之前调用，此时仍可以修改组件的 state。

- render

必选的方法，创建虚拟 DOM，该方法具有以下特殊的规则：

1. 只能通过 this.props 和 this.state 访问数据；

2. 可以返回 null、false 或任何 React 组件；

3. 只能出现一个顶级组件（不能返回数组）；

4. 不能改变组件的状态；

5. 不能修改 DOM 的输出。

- componentDidMount

真实的 DOM 被渲染出来后调用，在该方法中可通过 this.getDOMNode() 访问到真实的 DOM 元素。此时已可以使用其他类库来操作这个 DOM。

在服务端中，该方法不会被调用。

- componentWillReceiveProps

组件接收到新的 props 时调用，并将其作为参数 nextProps 使用，此时可以更改组件 props 及 state。

```js
componentWillReceiveProps: function(nextProps) {
  if (nextProps.bool) {
    this.setState({ bool: true });
  }
}
```

- shouldComponentUpdate

组件是否应当渲染新的 props 或 state，返回 false 表示跳过后续的生命周期方法，通常不需要使用以避免出现 bug。在出现应用的瓶颈时，可通过该方法进行适当的优化。

在首次渲染期间或者调用了 forceUpdate 方法后，该方法不会被调用。

- componentWillUpdate

接收到新的 props 或者 state 后，进行渲染之前调用，此时不允许更新 props 或 state。

- componentDidUpdate

完成渲染新的 props 或者 state 后调用，此时可以访问到新的 DOM 元素。

- componentWillUnmount

组件被移除之前被调用，可以用于做一些清理工作，在 componentDidMount 方法中添加的所有任务都需要在该方法中撤销，比如创建的定时器或添加的事件监听器。


### 新版本生命周期（v16.3）
