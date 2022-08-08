---
layout: post
title: 推荐一些好用的 VSCode 扩展
categories: [VSCode]
description: 推荐一些好用的 VSCode 扩展
keywords: [VSCode, 扩展]
---

VSCode（Visual Studio Code）作为代码编辑器，不仅开源、免费、颜值高，而且有丰富的扩展帮助我们提高开发效率，在前端开发中广受青睐。这篇文章总结了一些好用的扩展，不定期更新。

### Chinese (Simplified) (简体中文) Language Pack

刚下载的 VSCode 默认是英文，可以选择安装官方的中文语言包。[【传送门】](https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-zh-hans)

### 更换主题

`Command+Shift+P` 打开命令框，输入 color theme，进入主题列表，可以设置你喜欢的主题。

### 添加代码片段

`Command+Shift+P` 打开命令框，输入 user snippets，新建全局代码片段文件，在文件中设置代码片段。

```json
{
  // Example
  "Print to console": {
    "scope": "javascript,typescript",
    "prefix": "log",
    "body": [
      "console.log('$1');",
      "$2"
    ],
    "description": "Log output to console"
  }
}
```

### Auto Close Tag

输入 HTML 标签时自动关闭。[【传送门】](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag)

比如，输入 `<div>` 会自动填充后半部分 `</div>`。

### Auto Rename Tag

修改 HTML 标签时自动修改整个标签。[【传送门】](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag)

比如，我想把 `<div></div>` 修改为 `<p></p>`，只需要把第一个 div 修改为 p，第二个 div 会自动修改。

### Highlight Matching Tag

突出显示匹配的开始和结束标签。[【传送门】](https://marketplace.visualstudio.com/items?itemName=vincaslt.highlight-matching-tag)

比如，一个闭合的标签 `<div></div>`，当把光标定位到第一个 `div` 上时，两个 `div` 都会高亮显示。当元素内部层级较多，开始和结束的两个标签相隔较远时，此扩展对于快速定位到某个闭合标签很有用处。

### CSS Peek

`Command+点击` id 或 class 名可以从 HTML 文件定位到 CSS 文件对应位置。[【传送门】](https://marketplace.visualstudio.com/items?itemName=pranaygp.vscode-css-peek)

### Autoprefixer

自动给 CSS 代码添加兼容性前缀。[【传送门】](https://marketplace.visualstudio.com/items?itemName=mrmlnc.vscode-autoprefixer)

### Image Preview

引用图片时会在代码左侧显示图片的缩略图，方便预览。[【传送门】](https://marketplace.visualstudio.com/items?itemName=kisstkondoros.vscode-gutter-preview)

```css
div {
  background: url('assets/play.png') no-repeat center; 
}
```

### Code Spell Checker

一个拼写检查器，帮助捕获常见的拼写错误。[【传送门】](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)

### ESLint

代码格式校验工具，配合项目中的校验规则，实现保存时格式化代码。[【传送门】](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

### Prettier

代码格式化工具，在项目中可以配置自己的格式化规则。[【传送门】](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

### Javascript console utils

帮助插入和删除 `console.(*)` 语句。[【传送门】](https://marketplace.visualstudio.com/items?itemName=whtouche.vscode-js-console-utils)

- 选择一个变量 `test`，`Command+Shift+L` 会在下一行代码插入 `console.log('test', test)` 语句。
- `Command+Shift+D` 将删除当前文档中的所有 `console.log` 语句。

### koroFileHeader

用于生成文件头部注释和函数注释。[【传送门】](https://marketplace.visualstudio.com/items?itemName=OBKoro1.korofileheader)

- `Command+Shift+I` 生成文件头部注释。
- `Command+Shift+T` 生成函数注释。

### npm Intellisense

在 import 语句中自动完成 npm 模块。[【传送门】](https://marketplace.visualstudio.com/items?itemName=christian-kohler.npm-intellisense)

### Path Intellisense

自动识别文件路径，自动完成文件名。[【传送门】](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense)

### Import Cost

在编辑器中显示 import/require 包的大小。[【传送门】](https://marketplace.visualstudio.com/items?itemName=wix.vscode-import-cost)

### GitLens

可以查看每行代码的提交记录，包括提交人、提交时间、提交备注等。[【传送门】](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)

### Git History

在 VSCode 中集成 Git 工具，可以查看 Git 日志、文件历史、比较分支或提交代码。[【传送门】](https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory)

### Git Graph

类似于 SourceTree 的可视化版本控制插件，可以查看提交记录或执行 Git 操作。[【传送门】](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph)

### Todo Tree

在树形图中显示 TODO、FIXME 等注释标签。单击树形图中的注释标签将打开文件并将光标定位在包含注释标签的行上。[【传送门】](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree)

安装插件后会在左侧菜单栏生成一个待办事项菜单，点击可以查看项目下注释标签的树形图。

以上。

### 参考资料

- [VScode 常用必备插件](https://blog.csdn.net/weixin_43363871/article/details/122041130)
