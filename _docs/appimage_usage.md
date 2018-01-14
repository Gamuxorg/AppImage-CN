---
title: AppImage的使用方法
permalink: /docs/appimage-usage/
---

### AppImage的使用方法

<iframe src="https://player.youku.com/embed/XMzIyODAzNDk1Mg==" width="100%" height="400" frameborder="0" allowfullscreen="true"></iframe>

运行一个AppImage将会挂载文件系统镜像，并透明地运行包含的应用程序。所以AppImage的使用通常应该等于它包含的应用程序的使用。但是，如本文所说，他还有其他特殊功能。如果你收到的AppImgae不支持这些选项，请让AppImage的作者使用最新的`appimagetool`（或`linuxdeployqt`）重新构建。

#### 命令行参数

如果你运行一个由较新AppImageKit构建的AppImage并附加以下参数之一，那么AppImage的行为将有所不同：

- `--appimage-help` 显示帮助选项
- `--appimage-offset` 显示文件系统镜像开始的偏移量，然后退出。如果你想使用`mount -o loop，offset=...` 命令来循环挂载文件系统镜像，这很有用
- `--appimage-extract` 从文件系统镜像中提取内容，然后退出。如果你在不支持FUSE的系统上使用AppImage，这非常有用
- `--appimage-mount` 挂载嵌入式文件系统镜像并打印挂载地址，然后等待直到结束。如果你想检查AppImage的内容而不执行包含的应用程序内容，这非常有用
- `--appimage-version` 显示AppImageKit的版本，然后退出。当你需要提问的时候，这很有用
- `--appimage-updateinformation` 显示AppImage中的更新信息，然后退出。这对调试增量更新非常有用
- `--appimage-signature` 显示AppImage中的数字签名，然后退出。这对调试增量更新非常有用。如果你想验证此签名，则应使用AppImageKit的另一所属命令行工具 `validate`

#### 特殊的目录

通常情况下，包含在AppImage中的应用程序将存储它的配置文件，无论它通常存储在哪里（最常见的是在$HOME中的某处）。如果你运行由较新AppImageKit构建的AppImage，并且存在以下特殊目录之一，那么配置文件将与AppImage一起存储。这对于便携式使用情况是有用的，例如，在U盘上携带AppImage以及其数据。

- 如果有一个和AppImage文件同名的目录名且加`.home`，那么在执行应用程序之前，会自动将$HOME设置为它
- 如果有一个和AppImage文件同名的目录名且加`.config`，那么在执行应用程序之前，会自动将$XDG_CONFIG_HOME设置为它