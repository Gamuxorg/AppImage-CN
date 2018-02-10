---
title: 总结
permalink: /docs/wiki/
---

[原文链接](https://github.com/AppImage/AppImageKit/wiki)
# AppImageKit Wiki

每个人都可以在 https://github.com/AppImage/AppImageKit 编辑wiki

## 对于用户而言

### ![question-logo](https://raw.githubusercontent.com/encharm/Font-Awesome-SVG-PNG/master/black/png/48/question-circle.png)AppImage是什么？

一个AppImage应用即是一个可下载的Linux文件，其中包含一个应用程序和应用程序运行需要的所有内容（例如，库，图标，字体，转换层等），而这些内容并不一定已经存在于每个目标系统之中。

### ![question-logo](https://raw.githubusercontent.com/encharm/Font-Awesome-SVG-PNG/master/black/png/48/question-circle.png)如何运行一个AppImage应用？

[添加可执行的权限](http://discourse.appimage.org/t/how-to-make-an-appimage-executable/80)并双击它运行。

### ![question-logo](https://raw.githubusercontent.com/encharm/Font-Awesome-SVG-PNG/master/black/png/48/question-circle.png)如何将AppImage应用集成到系统里？

使用可选的`appimaged`守护进程，可以轻松地将AppImage应用与系统集成。守护进程将为AppImage应用创建菜单，注册MIME类型，图标，所有一切都可以自动完成。你可以从这个仓库下载它，再次重申这是非必选项。

### ![question-logo](https://raw.githubusercontent.com/encharm/Font-Awesome-SVG-PNG/master/black/png/48/question-circle.png)哪里可以下载AppImage应用？
前往[AppImageHub](https://appimage.github.io/apps/)查看所有收录的AppImage应用。

### ![question-logo](https://raw.githubusercontent.com/encharm/Font-Awesome-SVG-PNG/master/black/png/48/question-circle.png)AppImage应用安放在哪里比较合适？
如果你不想把它们放在`$HOME/Downloads`中，那么  `$HOME/.local/bin` 和 `$HOME/bin` 是很好的选择：
* 在 CentOS/RHEL 和 Fedora 上：脚本`$HOME/.bash_profile`在登录时运行，这个脚本将`$HOME/.local/bin:$HOME/bin`添加到路径中。
* 在Ubuntu上：脚本`$HOME/.profile`在登录时运行，这个脚本将`PATH="$HOME/bin:$HOME/.local/bin"`添加到路径中。

此外，存储任何其他位置也是可以，例如U盘，网络位置或光盘，但是这样AppImage应用的路径不在环境变量中，意味着不能简单地在终端输入应用名来运行，而必须使用完整的路径。

### ![question-logo](https://raw.githubusercontent.com/encharm/Font-Awesome-SVG-PNG/master/black/png/48/question-circle.png)我想要的应用没有AppImage打包版本怎么办？
如果您想运行的应用没有AppImage版本，请您向该应用的作者请求，例如您可以使用应用的问题反馈功能进行提议。举个例子，如果您想要Mozilla Firefox的AppImage版本，请在 https://bugzilla.mozilla.org/show_bug.cgi?id=1249971 上留言。向上游作者请求AppImage的人越多，则提供AppImage版本的可能性就越大。

### ![question-logo](https://raw.githubusercontent.com/encharm/Font-Awesome-SVG-PNG/master/black/png/48/question-circle.png)我在哪里可以得到相关技术支持？

请访问[官方论坛](http://discourse.appimage.org/) 。论坛支持Google或GitHub帐户登录，无需注册。

### 对于应用程序开发者而言

### ![question-logo](https://raw.githubusercontent.com/encharm/Font-Awesome-SVG-PNG/master/black/png/48/question-circle.png)为什么要将我的应用程序打包成AppImage？

通过将您的应用程序打包成AppImage，您可以像Windows(exe)和MacOS(dmg)所使用技术方式类似，为Linux用户提供官方的、便捷和简单的软件下载和安装方式，您作为应用程序作者直接掌握端到端的用户体验，而无需任何中介（打包）横亘在开发者您和最终用户之间。只需打包成AppImage格式，就可以连结大多数Linux发行版的用户。您可以随时提供新的下载链接，甚至可以为持续构建的项目提供快速不停的更新。

比如，制作AppImage有如下好处：
- 只需打包一次即可运行在大多数主流Linux发行版
- 开箱即用，运行时无需安装
- 不需要root权限
- 一个应用程序 ＝ 一个文件 ＝ 极度便利的用户检验
- 可选（！）的`appimaged`用于将AppImage集成入桌面环境
- 二进制增量更新，例如，使用AppImageUpdate用于连续构建时升级应用（只下载二进制增量数据）
- 可以以GPG2的形式签署您的AppImages（文件内）

### ![question-logo](https://raw.githubusercontent.com/encharm/Font-Awesome-SVG-PNG/master/black/png/48/question-circle.png)如何将我的应用程序打包为AppImage？

有下面几种方式生成您的AppImage应用：

1.转换现有的二进制包，或者
2.将您的`Travis CI builds`打包成AppImage应用，或者
3.使用[linuxdeployqt](https://github.com/probonopd/linuxdeployqt/)工具转换您的Qt应用程序，或者
4.使用`electron-builder`，或者
5.自己编写

更多详情和示例，请参见 https://github.com/probonopd/AppImageKit/wiki/Creating-AppImages

### ![question-logo](https://raw.githubusercontent.com/encharm/Font-Awesome-SVG-PNG/master/black/png/48/question-circle.png)打包自己的AppImage应用过程中如何得到官方帮助？

我们尽力帮助上游应用程序作者，请应用作者通过在AppImage项目中新增一个[issue](https://github.com/AppImage/AppImageKit/issues)的方式获得支持。如果您不是上游应用程序作者，在发起issue之前请先联系上游应用程序作者。

### 对于AppImage开发者而言

#### ![question-logo](https://raw.githubusercontent.com/encharm/Font-Awesome-SVG-PNG/master/black/png/48/question-circle.png)如何参与AppImage项目做出贡献？

对AppImage开发感到好奇？想做出自己的贡献？我们欢迎通过`pull requests`方式来解决任何公开问题和（或）其他错误修正和（或）功能的添加。在添加复杂功能的情况前，最好先与官方沟通联系以免徒耗时间。请参阅我们的[问题列表](https://github.com/probonopd/AppImageKit/issues)，并在`irc.freenode.net`上的`＃AppImage`频道中与我们联系。
