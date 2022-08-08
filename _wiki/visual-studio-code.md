---
layout: wiki
title: Visual Studio Code
cate1: Tools
cate2:
description: Visual Studio Code 常用快捷键与使用技巧
keywords: Visual Studio Code
---

## 快捷键约定

Cmd --> Command

| 功能              | Windows      |  macOS       |
|:------------------|:------------|:--------------|
| 打开文件          | Ctrl+o        |  Cmd+o        |
| 打开文件夹        | Ctrl+k Ctrl+o |               |
| 关闭文件夹        | Ctrl+k f      |               |
| 命令面板          | Ctrl+Shift+p  |  Cmd+Shift+p  |
| 资源管理器        | Ctrl+Shift+e   |               |
| 全局搜索           | Ctrl+Shift+f  |  Cmd+Shift+f  |
| 按名称搜索文件      | Ctrl+p        |  Cmd+p        |
| Git              | Ctrl+Shift+g  |               |
| 调试              | Ctrl+Shift+d  |  Cmd+Shift+d  |
| 插件              | Ctrl+Shift+x  |  Cmd+Shift+x  |
| Markdown 侧边预览  | Ctrl+k v      |  Cmd+k v      |
| Markdown 预览     | Ctrl+Shift+v  |  Cmd+Shift+v  |

## 使用 VSCode 作为 mergetool

编辑 ~/.gitconfig 文件，添加如下内容：

```
[merge]
    tool = vscode
[mergetool "vscode"]
    cmd = code --wait $MERGED
```

需要的时候执行 git mergetool 就会调起了。

参考：<https://blog.kulman.sk/using-vscode-as-git-merge-tool/>

## VSCodeVim 支持按键重复

在 macOS，默认情况 VSCodeVim 模式下是不支持按键重复的，比如你在 Normal 模式下长按 `L`，结果光标只向右移动了一次，而没有像你预期的那样一直移动。

启用按键重复的方法在插件的 REAME 有说明，链接：<https://github.com/VSCodeVim/Vim#mac>

方法：

按需执行下面的某一行命令并重启 VSCode。

```sh
$ defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false         # For VS Code
$ defaults write com.microsoft.VSCodeInsiders ApplePressAndHoldEnabled -bool false # For VS Code Insider
$ defaults write com.visualstudio.code.oss ApplePressAndHoldEnabled -bool false    # For VS Codium
$ defaults delete -g ApplePressAndHoldEnabled                                      # If necessary, reset global default
```

如果有需要，调整「系统偏好设置」—「键盘」里的「按键重复」和「重复前延迟」。
