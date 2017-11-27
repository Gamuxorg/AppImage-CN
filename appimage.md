# 什么是AppImage？

__AppImage__格式是一种用于打包应用程序的格式
允许在各种不同的目标系统（基本操作系统，发行版）上运行，无需进一步修改。

https://en.wikipedia.org/wiki/AppImage

__AppImageKit__是AppImage格式的具体实现，提供了`appimagetool`和`appimaged`等工具来方便地处理AppImage。

`appimagetool`将AppDir转换成自装载的文件系统镜像。 `appimaged`是一个守护进程，用于处理与系统（例如，菜单条目，图标，MIME类型，二进制增量更新等）一起注册和取消注册AppImage。

提供用于分发应用的[[AppImage](http://appimage.org/)]具有以下优点：
- 作为AppImage打包的应用程序可以在许多发行版上运行（包括Ubuntu，Fedora，openSUSE，CentOS，basicOS，Linux Mint等）
- 一个应用程序=一个文件=对用户超级简单：只需下载一个AppImage文件，[[使其可执行](http://discourse.appimage.org/t/how-to-make-an-appimage-executable/80)] ，然后运行
- 不需要拆包或安装
- 没有根需要
- 没有系统库改变
- 开箱即用，无需安装运行时
- 可选桌面与`appimaged`集成
- 可选的二进制增量更新，例如，用于连续构建（仅下载二进制比较）使用AppImageUpdate
- 可以选择GPG2 - 签署你的AppImages（在文件内）
- 在Live ISOs上工作
- 双启动多个发行版时可以使用相同的AppImages
- 可以列在可用AppImage的[[AppImageHub](https://appimage.github.io/apps)]中央目录中
- 可以使用“--appimage-extract”参数作为自解压压缩归档

[[这里是一个概述](https://appimage.github.io/apps)]已经分发上游提供的官方AppImages项目。

如果您有任何问题，AppImage开发人员正在使用irc.freenode.net上的#AppImage。

# What is AppImage?

The __AppImage__ format is a format for packaging applications in a way that allows them to
run on a variety of different target systems (base operating systems, distributions) without further modification. 

https://en.wikipedia.org/wiki/AppImage

__AppImageKit__  is  a  concrete  implementation  of  the  AppImage  format  and  provides  tools such as `appimagetool` and `appimaged` for conveniently handling AppImages.

`appimagetool` converts an AppDir into a self-mounting filesystem image. `appimaged` is a daemon that handles registering and unregistering AppImages with the system (e.g., menu entries, icons, MIME types, binary delta updates, and such).

Providing an [AppImage](http://appimage.org/) for distributing application has, among others, these advantages:
- Applications packaged as an AppImage can run on many distributions (including Ubuntu, Fedora, openSUSE, CentOS, elementaryOS, Linux Mint, and others)
- One app = one file = super simple for users: just download one AppImage file, [make it executable](http://discourse.appimage.org/t/how-to-make-an-appimage-executable/80), and run
- No unpacking or installation necessary
- No root needed
- No system libraries changed
- Works out of the box, no installation of runtimes needed
- Optional desktop integration with `appimaged`
- Optional binary delta updates, e.g., for continuous builds (only download the binary diff) using AppImageUpdate
- Can optionally GPG2-sign your AppImages (inside the file)
- Works on Live ISOs
- Can use the same AppImages when dual-booting multiple distributions
- Can be listed in the [AppImageHub](https://appimage.github.io/apps) central directory of available AppImages
- Can double as a self-extracting compressed archive with the `--appimage-extract` parameter

[Here is an overview](https://appimage.github.io/apps) of projects that are already distributing upstream-provided, official AppImages.

If you have questions, AppImage developers are on #AppImage on irc.freenode.net.