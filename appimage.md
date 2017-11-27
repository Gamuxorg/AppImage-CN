# 什么是AppImage？

__AppImage__ 是一种把应用打包成单一文件的格式，允许在各种不同的目标系统（基础系统(Debian、RHEL等)，发行版(Ubuntu、Deepin等)）上运行，无需进一步修改。

https://en.wikipedia.org/wiki/AppImage

__AppImageKit__ 是AppImage的构建工具包，提供了`appimagetool`和`appimaged`等工具来方便地处理AppImage。

`appimagetool`将AppDir转换成自挂载的文件系统镜像。 `appimaged`是一个守护进程，用于处理AppImage与系统（菜单条目，图标，MIME类型，二进制增量更新等）的注册或注销。

提供[[AppImage](http://appimage.org/)]格式的包用于分发应用相对于其他格式，具有以下优点：
- 使用AppImage打包的应用程序可以在许多发行版上运行（包括Ubuntu，Fedora，openSUSE，CentOS，basicOS，Linux Mint等）
- 一个应用程序 = 一个文件 = 对用户超级简单：只需下载一个AppImage文件，[[使其可执行](http://discourse.appimage.org/t/how-to-make-an-appimage-executable/80)] ，然后运行
- 不需要解压或安装
- 不需要root权限（通常在Debian及其衍生版你需要sudo apt install来安装软件并需要输入密码）
- 不会改变系统的依赖库
- 开箱即用，无需安装运行时（使用snap和flatpak却需要）
- 可选择桌面与`appimaged`集成（注册快捷方式等）
- 可以二进制增量更新，例如，用于连续构建（仅下载二进制比较）的AppImageUpdate工具
- 可以使用GPG2 - （在文件内）签署你的AppImage应用
- 在Live ISOs上运行
- 系统安装多个发行版时可以使用相同的AppImage应用
- 可以在AppImage的应用中心[[AppImageHub](https://appimage.github.io/apps)]发布和下载应用
- 可以使用“--appimage-extract”参数来自解压应用，例如
```
./typora.AppImage --appimage-extract
```

这里是官方上游已分发的AppImage格式应用的[[概览](https://appimage.github.io/apps)]。

如果您有任何问题，请到irc.freenode.net上的#AppImage频道进行询问。

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