---
title: 什么是AppImage？
permalink: /docs/appimage/
---


### 什么是AppImage？

__AppImage__ 是一种把应用打包成单一文件的格式，允许在各种不同的目标系统（基础系统(Debian、RHEL等)，发行版(Ubuntu、Deepin等)）上运行，无需进一步修改。

https://en.wikipedia.org/wiki/AppImage

__AppImageKit__ 是AppImage的构建工具包，提供了`appimagetool`和`appimaged`等工具来方便地处理AppImage。

`appimagetool`将AppDir目录转换成自挂载的文件系统镜像。 `appimaged`是一个守护进程，用于处理AppImage与系统（菜单条目，图标，MIME类型，二进制增量更新等）的注册或注销。

提供[AppImage](http://appimage.org/)格式的包用于分发应用相对于其他格式，具有以下优点：
- 使用AppImage打包的应用程序可以在许多发行版上运行（包括Ubuntu，Fedora，openSUSE，CentOS，basicOS，Linux Mint等）
- 一个应用程序 = 一个文件 = 对用户超级简单：只需下载一个AppImage文件，[使其可执行](http://discourse.appimage.org/t/how-to-make-an-appimage-executable/80) ，然后运行
- 不需要解压或安装
- 不需要root权限（通常在Debian及其衍生版你需要sudo apt install来安装软件并需要输入密码）
- 不会改变系统的依赖库
- 开箱即用，无需安装运行库（使用snap和flatpak却需要）
- 可选的`appimaged`桌面集成功能（注册快捷方式等）
- 可以二进制增量更新，例如，用于连续构建（仅下载二进制比较）的AppImageUpdate工具
- 可以使用GPG2 - （在文件内）签署你的AppImage应用
- 在Live ISOs上运行
- 系统安装多个发行版时可以使用相同的AppImage应用
- 可以在AppImage的应用中心[AppImageHub](https://appimage.github.io/apps)发布和下载应用
- 可以使用“--appimage-extract”参数来自解压应用，例如
```
./typora.AppImage --appimage-extract
```

这里是官方上游已分发的AppImage格式应用的[概览](https://appimage.github.io/apps)。

如果您有任何问题，请到irc.freenode.net上的#AppImage频道进行询问。
