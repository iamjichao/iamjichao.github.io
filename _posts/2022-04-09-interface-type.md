---
layout: post
title: interface 和 type 的区别
categories: [TypeScript]
description: interface 和 type 的区别
keywords: [TypeScript, interface, type]
---

在 TypeScript 中，interface 和 type 都可以用来定义类型。它们的主要区别在于：

- interface 可以被继承和实现，而 type 不行；
- type 可以定义更复杂的类型，比如交叉类型和联合类型；
- interface 可以定义多个同名接口，会自动合并属性，而 type 不行；

具体来说，当你想要定义一个对象的类型时，通常使用 interface。而当你需要定义更复杂的类型时，比如联合类型、交叉类型、元组类型等，你就需要用到 type。

例如，下面是一个使用 interface 定义对象类型的简单示例：

```ts
interface User {
  id: number;
  name: string;
  age?: number;
  sayHello(): void;
}

const user: User = {
  id: 1,
  name: 'Tom',
  sayHello() {
    console.log('Hello');
  },
};
```

在上面的示例中，我们使用 interface 定义了一个名为 User 的对象类型，它包含了 id、name、age 和 sayHello 四个属性。然后，我们使用 User 类型定义了一个名为 user 的对象。

如果你想要定义一个更复杂的类型，比如联合类型或交叉类型，你可以使用 type 来定义。例如：

```ts
type MyUnionType = string | number;

type MyIntersectionType = { id: number } & { name: string };

type MyTupleType = [string, number];
```

在上面的示例中，我们使用 type 定义了三种复杂类型：联合类型 MyUnionType、交叉类型 MyIntersectionType 和元组类型 MyTupleType。这些类型使用了 type 独有的语法，可以更加灵活地定义不同类型的结构。

以上。
