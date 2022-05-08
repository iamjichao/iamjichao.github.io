---
layout: post
title: Vue 中导出 Excel 表格
categories: [JavaScript, Vue]
description: Vue 中导出 Excel 表格
keywords: [JavaScript, Vue]
---

工作中需要将 Vue 项目中的表格导出为 Excel，使用了 [vue-json-excel](https://github.com/jecovier/vue-json-excel) 插件，在这里记录一下使用方法和注意事项。

### 使用步骤

- 安装依赖。在项目根目录下执行：

```
npm install vue-json-excel --save
```

- 在项目入口注册组件：

```js
import Vue from 'vue'
import JsonExcel from 'vue-json-excel'

Vue.component('downloadExcel', JsonExcel)
```

- 在页面中使用：

```html
<download-excel
  :data="json_data"
  :fields="json_fields"
  name="filename.xls">
  <button>导出 Excel</button>
</download-excel>
```
这里 data 属性是必需的。

### 组件属性

属性 | 类型 | 描述
---|---|---
data | Array | 要导出的数据
fields | Object | 要导出的 JSON 对象中的字段。如果没有给定，则导出 JSON 中的所有属性
export-fields (exportFields) | Object | 当其他组件使用了 fields 属性时，用这个属性代替，用法与 fields 相同
type | String | 导出的文件类型，支持两种格式 xls 和 csv，默认是 xls
name | String | 要导出的文件名，默认是 data.xls
title | String / Array | 数据的标题，可以是字符串或字符串数组（多个标题）
footer | String / Array | 数据的页脚，可以是字符串或字符串数组（多个页脚）
default-value (defaultValue) | String | 某一项没有值时，使用这个默认值，默认是''
worksheet | String | 工作表选项卡的名称，默认是 Sheet1
fetch | Function | 下载表格之前获取数据的回调函数。它会在鼠标点击后立即运行，并在下载表格之前运行。**重要提示**：只有在没有定义 data 属性时才有效
before-generate | Function | 在生成 / 获取数据之前调用的方法，例如显示加载进度
before-finish | Function | 在下载框弹出之前调用的方法，例如隐藏加载进度

### 注意事项

- 多行值会在 Excel 中的单个单元格中显示。

- 使用 fetch 属性实现按需获取数据并导出。

- 如果需要自定义导出的数据，可以使用回调函数对要导出的数据进行格式化：

```js
let json_fields = {
  // 常规 field
  fieldLabel: attributeName, // 支持嵌套属性
  // 回调函数进行数据格式化
  anotherFieldLabel: {
    field: anotherAttributeName, // 支持嵌套属性
    callback: value => {
      return `formatted value ${value}`
    }
  }
}
```

回调函数接收的参数 value 和 fields 定义的检索对象一致，如果没有定义 fields，那 value 就是整个 JSON 对象。以下是常见的三种情况。

```js
let json_fields = {
  'Complete name': 'name',
  'City': 'city',
  'Telephone': 'phone.mobile',
  'Telephone2': {
    field: 'phone.landline',
    callback: value => {
      return `Landline Phone - ${value}`;
    }
  }
}

let json_fields = {
  'Complete name': 'name',
  'City': 'city',
  'Telephone': 'phone.mobile',
  'Telephone2': {
    field: 'phone',
    callback: value => {
      return `Landline Phone - ${value.landline}`;
    }
  }
}

let json_fields = {
  'Complete name': 'name',
  'City': 'city',
  'Telephone': 'phone.mobile',
  'Telephone2': {
    callback: value => {
      return `Landline Phone - ${value.phone.landline}`;
    }
  }
}
```

以上。
