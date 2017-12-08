# AppImageKit Wiki

每个人都可以在 https://github.com/AppImage/AppImageKit 编辑wiki

## 对于用户

### 什么是AppImage？

AppImage是一个可下载的Linux文件，其中包含一个应用程序和应用程序运行需要的所有内容（例如，库，图标，字体，转换层等），而这些内容并不一定已经存在于每个目标系统之中。

### 如何运行AppImage？

[添加可执行的权限](http://discourse.appimage.org/t/how-to-make-an-appimage-executable/80)并双击它。

### 如何将AppImage集成到系统里？

使用可选的`appimaged`守护进程，可以轻松地将AppImage与系统集成。守护进程将为AppImages创建菜单，注册MIME类型，图标，所有一切都可以自动完成。你可以从这个仓库下载它，但它并不是必需的。

### 哪里可以下载AppImages？
查看[AppImageHub](https://appimage.github.io/apps/)。

### 可以将AppImages存储在哪里？
如果你不想把它们放在`$HOME/Downloads`中，那么  `$HOME/.local/bin` 和 `$HOME/bin` 是很好的选择：
* 在 CentOS/RHEL 和 Fedora 上：脚本`$HOME/.bash_profile`在登录时运行，将`$HOME/.local/bin:$HOME/bin`添加到路径中。
* 在Ubuntu上：脚本`$HOME/.profile`在登录时运行，将`PATH="$HOME/bin:$HOME/.local/bin"`添加到路径中。

此外，存储任何其他位置也是可以，例如U盘，网络位置或光盘，但是这样AppImages不会在路径中，这意味着你不能简单地在终端输入应用名来运行，而必须使用完整的路径。

### 我可以在哪里请求AppImages？
如果您想运行的应用没有AppImage格式，请您向该应用的作者请求，例如您可以使用应用的问题反馈功能进行请求。举个例子，如果您想要Mozilla Firefox的AppImage版本，请在 https://bugzilla.mozilla.org/show_bug.cgi?id=1249971 上留言。从上游作者请求AppImage的人越多，则提供AppImage的可能性就越大。

### 我在哪里可以得到支持？

请访问 http://discourse.appimage.org/ 。您可以使用现有的Google或GitHub帐户登录，无需注册。

## 对于应用程序开发者

### 为什么要将我的应用程序打包成AppImage？

通过将您的应用程序打包成AppImage，您可以像对Windows和MacOS所使用方式一样为Linux提供官方下载，您作为应用程序作者直接掌握端到端的用户体验，而无需任何中介（打包）在开发者您和最终用户之间。只需一个AppImage，您就可以连结大多数Linux发行版的用户。您可以随时提供新的下载链接，甚至可以为持续构建的项目提供快速不停的更新。

另外，制作AppImage有如下好处：
- 所有主要发行版只需要同一个格式
- 开箱即用，运行时无需安装
- 不需要根用户
- 一个应用程序 ＝ 一个文件 ＝ 极度便利的用户检验
- 可选（！）的`appimaged`用于将AppImage集成入桌面环境
- 二进制增量更新，例如，使用AppImageUpdate用于连续构建时升级应用（只下载二进制增量数据）
- 可以以GPG2的形式签署您的AppImages（文件内）

### 如何将我的应用程序打包为AppImage？

有不同的方法来生成您的应用程序的AppImage：

1.转换现有的二进制包，或者
2.将您的Travis CI builds打包成AppImages，或者
3.使用linuxdeployqt转换您的Qt应用程序，或者
4.使用electron-builder，或者
5.自己编写

更多详情和示例，请参见 https://github.com/probonopd/AppImageKit/wiki/Creating-AppImages 

### 我如何获得支持来帮助打包自己的应用成AppImage？

我们尽力帮助上游应用程序作者，请应用作者通过在AppImage项目中新增一个issue的方式获得支持。如果您不是上游应用程序作者，请在此处新增issue之前先联系上游应用程序作者。

## 对于AppImage开发者

### 我怎样才能贡献？

对AppImage开发感到好奇？想贡献？我们欢迎解决任何公开问题和（或）其他错误修正和（或）功能添加的请求。在添加复杂功能的情况下，您最好在耗费诸多时间之前联系我们。请参阅我们的[问题列表](https://github.com/probonopd/AppImageKit/issues)，并在`irc.freenode.net`上的`＃AppImage`频道中与我们联系。

# AppImageKit Wiki

Everyone can edit the wiki at https://github.com/AppImage/AppImageKit

## For users

### What is an AppImage?

An AppImage is a downloadable file for Linux that contains an application and everything the application needs to run (e.g., libraries, icons, fonts, translations, etc.) that cannot be reasonably expected to be part of each target system.

### How do I run an AppImage?

[Make it executable](http://discourse.appimage.org/t/how-to-make-an-appimage-executable/80) and double-click it.

### How can I integrate AppImages with the system?

Using the optional `appimaged` daemon, you can easily integrate AppImages with the system. The daemon puts AppImages into the menus, registers MIME types, icons, all on the fly. You can download it from this repository. But it is entirely optional.

### Where can I download AppImages?
See the "repository" of [upstream-generated AppImages](https://github.com/probonopd/AppImageKit/wiki/AppImages).

### Where do I store my AppImages?
If you don't want to leave them in `$HOME/Downloads`, then `$HOME/.local/bin` and `$HOME/bin` are good choices:
* On CentOS/RHEL and Fedora: When you login, the script `$HOME/.bash_profile` is executed and this script adds `$HOME/.local/bin:$HOME/bin` to your path.
* On Ubuntu: When you login, the script `$HOME/.profile` is executed and this script adds `PATH="$HOME/bin:$HOME/.local/bin"` to your path.

Besides, every other location works, e.g., a USB thumbdrive, a network location, or a CD-ROM, but then the AppImages won't be on your path, which means that you cannot simply type their name into a terminal but have to use the full path.

### Where can I request AppImages?
If there is no AppImage of your favorite application available, please request it from the author(s) of the application, e.g., as a feature request in the issue tracker of the application. For example, if you would like to see an AppImage of Mozilla Firefox, then please leave a comment at https://bugzilla.mozilla.org/show_bug.cgi?id=1249971. The more people request an AppImage from the upstream authors, the more likely is that an AppImage will be provided.

### Where do I get support?

Please visit http://discourse.appimage.org/. You can log in using your existing Google or GitHub account, no sign-up needed.

## For application developers

### Why should I bundle my application as an AppImage?

By bundling your application as an AppImage, you can provide an official download for Linux like you would do for Windows and macOS where you as the application author can control the end-to-end user experience with no intermediaries between you as the author and your end user. With just one AppImage you can reach users of most Linux distributions. You can provide new download links as often as you like, e.g., for each continuous build.

Also, doing an AppImage has these advantages:
- Just one format for all major distributions
- Works out of the box, no installation of runtimes needed
- No root needed
- One app = one file = super simple for users
- Optional(!) desktop integration with `appimaged`
- Binary delta updates, e.g., for continuous builds (only download the binary diff) using AppImageUpdate
- Can GPG2-sign your AppImages (inside the file)

### How do I bundle my application as an AppImage?

There are different ways to generate an AppImage of your application:

1. Convert existing binary packages, or
2. Bundle your Travis CI builds as AppImages, or
3. Run linuxdeployqt on your Qt application, or
4. Use electron-builder, or
5. Write your own

See https://github.com/probonopd/AppImageKit/wiki/Creating-AppImages for more information and examples.

### How do I get support in bundling my app as an AppImage?

We try to help upstream application authors as good as we can, please open an issue in this project. If you are not an upstream application author, then please contact the upstream application author(s) first before you open an issue here.

## For AppImage developers

### How can I contribute?

Curious about AppImage development? Want to contribute? We welcome pull requests addressing any of the open issues and/or other bugfixes and/or feature additions. In the case of complex feature additions, it is best to contact us first, before you spend much time. See our [list of issues](https://github.com/probonopd/AppImageKit/issues) and get in touch with us in `#AppImage` on `irc.freenode.net`.
