---
layout: post
title: 解决 Git 远程仓库文件名大小写问题
categories: [Git]
description: 解决 Git 远程仓库文件名大小写问题
keywords: [Git, 文件名大小写]
---

Git 远程仓库已经存在了小写文件名，需要将小写改为首字母大写。直接粗暴地修改了文件名，Git 却无法检测出更改。记录一下解决步骤。

### 修改 Git 配置

Git 默认配置为忽略大小写，因此无法正确检测文件大小写的更改。运行以下命令将 Git 配置为大小写敏感：

```
git config core.ignorecase false
```

此时 Git 可以检测出文件名大小写的更改，将修改后的文件 push 到远程仓库后又发现远程仓库中原来的文件还存在。

### 删除多余文件

运行：

```
git rm --cached src/pages/index -r
```

删除文件夹里面的文件，文件夹也会消失。

操作成功提示：

```
rm 'src/pages/index/index.jsx'
rm 'src/pages/index/index.scss'
```

### 提交到远程仓库

```
git add .
git commit -m 'rm files'
git push origin master
```

以上步骤顺利完成，远程仓库更新成功。
