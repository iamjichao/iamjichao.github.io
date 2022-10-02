---
layout: post
title: 修改 Windows 鼠标滚轮方向
categories: [Windows]
description: 修改 Windows 鼠标滚轮方向
keywords: [Windows, 鼠标]
---

由于在平时生活和学习中使用的 Mac，工作中使用的 Windows，而两者鼠标滚轮的方向刚好相反，会造成一些不便，所以就把 Windows 的鼠标设置成了 Mac 模式。以下以 Windows10 为例，记录一下设置步骤。

- 首先按以下顺序获取到需要设置的鼠标的设备实力路径：计算机→右键→管理→系统工具→设备管理器→鼠标和其他指针设备→打开鼠标的属性→详细信息→属性→设备实例路径。

![image](https://fehub.net/images/posts/2019/windows-mouse-wheel-1.png)

- Win+R 输入 `regedit` 打开注册表，在 `计算机\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\xxx\xxx\xxx\Device Parameters` 路径下找到 `FlipFlopWheel` 字段，默认为 0，修改为 1。`xxx\xxx\xxx` 对应第一步中获取到的设备实例路径。

![image](https://fehub.net/images/posts/2019/windows-mouse-wheel-2.png)

- 最后重新连接鼠标即可，不用重启 Windows。

以上。
