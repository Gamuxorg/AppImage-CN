---
title: 创建AppImage
permalink: /docs/creating-appimages/
---
[原文链接](https://github.com/AppImage/AppImageKit/wiki/Creating-AppImages)
## 关于创建AppImage应用方面的介绍

#### 创建AppImage的一般工作流程涉及以下步骤：

1. __收集合适的二进制文件__ 如果应用程序是已编译好的，则可以使用现有的二进制文件（例如：包含在.tar.gz，deb或rpm包中的二进制文件）。但请注意，二进制文件的编译环境绝不能比你现有的更新（如果你的工作环境是Ubuntu 9.10，你不应该使用Ubuntu 10.04或更新系统上编译的二进制文件）。对于上游项目(指应用程序开发方，译者注)，为AppImage专门编译一份二进制是有利(于构建)的，尽管这不是必需的。

2. __收集所有合适的依赖项二进制文件__ 收集你期望支持的 **基础** 操作系统环境(在运行应用程序时)所缺的所有依赖。例如，如果你期望支持Ubuntu，Fedora和openSUSE，那么你需要收集你的应用程序运行所缺在Ubuntu，Fedora和openSUSE的依赖库(.so文件，译者注)和其他依赖项的合集。

3. __根据二进制文件创建一个可用的AppDir文件夹__ 文件夹名称一般命名为xxx.AppDir(xxx根据二进制文件或个人喜好而定)，打包AppImage前先执行文件夹内AppRun确保应用程序在你的工作环境是正常运行的。

4. __把AppDir文件夹转换成AppImage__ 将你的xxx.AppDir文件夹内的内容压缩成单一的自挂载和自执行AppImage文件。

5. __测试你的AppImage__ 在所有期望支持的 **基础** 操作系统环境测试打包好的AppImage程序。这一步非常重要，每个发行版都有细微差异。要确保大多数情况下AppImage应用都可以在各种发行版上运行，需要仔细的手动调整才会完美支持。

理论上可以手动完成上述步骤，但AppImageKit内提供的工具会减轻你的工作量。

现在，实践中我们发现有多种不同的方法来生成AppImage格式应用程序：

1. 使用`Open Build Service(OBS)`
2. 转换已有的二进制包(.deb,.rpm ...)
3. 使用`Travis CI`
4. 适用于QT程序的实用工具`linuxdeployqt`
5. 使用`electron-builder`
6. 手工创建 AppDir

## 1. Open Build Service (OBS)

建议将此方法用于开源项目，因为它允许你利用现有的Open Build Service基础设施、安全性和协议许可流程。请参阅 https://github.com/probonopd/AppImageKit/wiki/Using-Open-Build-Service 了解如何使用它。

## 2. 转换现有的二进制包

如果你已经有最新的软件包，这个方法可能是最简单的，理想的情况是 earlier 或 trusty ppa  或者debian的oldstable源的包。在这种情况下，你只要可以编写一个简单的`.yml`文件就可以了，很多工作都是由AppImage构建脚本自动完成的。 [这个脚本](https://raw.githubusercontent.com/probonopd/AppImages/master/recipes/meta/Recipe)可以运行.yml文件。 [参见示例](https://github.com/probonopd/AppImages/tree/master/recipes/meta)

## 3. 将你的Travis CI版本打包为AppImage应用

如果你已经在 Travis CI 上进行过持续构建工作，这个选项可能是最简单的。在这种情况下，你可以编写一个小的脚本文件，很多工作都是由AppImage构建脚本自动完成的。 [参见示例](https://github.com/search?utf8=%E2%9C%93&q=%22Package+the+binaries+built+on+Travis-CI+as+an+AppImage%22&type=Code&ref=searchresults)

## 4. 适用于QT程序的实用工具linuxdeployqt

如果你的QT程序是采用`cmake``qmake`或者`make`进行源码编译，或者是一个Qt-based型应用程序，这个选项可能是最简单的。在后者的情况下(qt-based)，之前你可能已经在使用`windeployqt`和`macdeployqt`工具了，现在可以用同样的风格来使用`linuxdeployqt`配合`-appimage`参数运行，很多工作都是由AppImage自动完成的。[参见示例](//github.com/coryo/amphetype2/blob/2d41de3b0c19ab9286672ff0d6a7c11eadc13d9c/.travis/deploy.sh)

## 5. 使用 electron-builder

如果你有一个基于Electron技术开发的应用程序，这个选项可能是最简单的。在这种情况下，将AppImage定义为Linux的目标（在最新版本的electron-builder中是默认的），很多工作都是由AppImage自动完成的的。 [参见示例](https://github.com/search?utf8=%E2%9C%93&q=electron-builder+linux+target+appimage&type=Code&ref=searchresults)

## 6. 手动创建AppDir

手动创建一个AppDir，然后把它转化成一个AppImage应用。先看下面的例子，再看关于将某些应用程序或应用程序类型绑定为AppImages的例子，或者从右侧的**页面**菜单中查看示例。

#### 手动创建一个AppDir

实际上，一般情况下可能永远不会手动做这个。所以这主要是为了说明这个概念。

创建一个AppDir结构，看起来（至少）如下所示：

```
MyApp.AppDir/
MyApp.AppDir/AppRun
MyApp.AppDir/myapp.desktop
MyApp.AppDir/myapp.png
MyApp.AppDir/usr/bin/myapp
MyApp.AppDir/usr/lib/libfoo.so.0
```
`AppRun` 文件可以是脚本或可执行文件。它用来设置所需的环境变量，例如 `$PATH` 并启动实际的应用程序。你可以编写你自己的脚本，但是在大多数情况下最简单的（也是最不容易出差错的）是用版本库中的预编译的版本。

当然，如果你的应用程序不需要依赖库，或者你的应用程序需要的所有依赖库都已包含在你所针对的每个基本操作系统中，那么你可以省略收集依赖库。

#### 禁止硬编码路径

程序的二进制文件 `myapp` 不能包含任何硬编码的路径，以防止其被重定位。你可以运行 `strings MyApp.AppDir/usr/bin/myapp | grep /usr` 。如果返回一些信息，那么你需要修改你的应用程序代码（例如，使用[binreloc](https://github.com/ximion/binreloc)，或者用`QString QCoreApplication::applicationDirPath()` ）。如果你不想更改应用程序的源代码或不想重新编译应用程序，也可以使用命令`sed -i -e 's|/usr|././|g' MyApp.AppDir/usr/bin/myapp`为二进制文件打补丁。通常只要应用程序没有执行`chdir()`就没问题，因为这可能会使`././`不再指向`$APPDIR/usr`。你可以运行`strace -echdir -f ./AppRun`来查看应用程序是否调用了`chdir()`（99％的GUI应用程序都没有）。

另请参阅 https://www.gnu.org/software/gnulib/manual/html_node/Supporting-Relocation.html 。长期以来，GNU软件包无法重定位的问题折磨了许多用户。relocatable-prog模块旨在简化GNU程序可重定位的工作。

__注意：__ 对于应用程序依赖的任何辅助二进制文件和其依赖库也是如此。你可以用 `cd MyApp.AppDir/usr/ ; find . -type f -exec sed -i -e 's|/usr|././|g' {} \; ; cd -`，这会用`././`替换所有`/usr`。

myapp.desktop（至少）应该包含：

```
[Desktop Entry]
Name=MyApp
Exec=myapp
Icon=myapp
Type=Application
Categories=Utilities;
```

一定要选择一个[注册类别](https://standards.freedesktop.org/menu-spec/latest/apa.html)，并确保你的desktop文件用`desktop-file-validate myapp.desktop`检验通过。

然后，在AppDir上运行`appimagetool`以将其转换为AppImage。你可以从此仓库的[Releases](https://github.com/probonopd/AppImageKit/releases)获取（appimagetool.AppImage，这来自AppImage项目。没错，我们吃自己的狗粮）。

## 创建可移植的AppImages

要在大多数系统上运行AppImage应用，需要满足以下条件：
1. AppImage应用需要包含基本系统环境缺失的所有库和其他依赖项
2. AppImage应用中包含的二进制文件不能在比目标系统更新的系统上编译
3. AppImage应用实际上应该在其打算运行的基础系统上进行测试

#### 在足够老的基本系统上编译二进制文件

不应在最新的基础系统上，而是你的AppImage要运行的最老的基础系统上编译。某些核心库（如glibc）往往会破坏旧版本基本系统的兼容性，这意味着二进制文件可以运行在较新的版本上，而在比二进制文件编译的版本旧的版本上就运行不了了。

如果你遇到这样的错误

```
failed to initialize: /lib/tls/i686/cmov/libc.so.6: version `GLIBC_2.11' not found
```

那么二进制文件是在一个比你试图运行的系统更新的系统上编译的。你应该使用在旧系统上编译的二进制文件。不幸和复杂的是，发行版通常只在最新的系统上编译最新版本的应用程序，这意味着你将很难找到在旧版系统上找到一些比较新的软件或软件的新版本。解决这个问题的方法是在不太新的基础系统上自己编译依赖关系，或使用[LibcWrapGenerator](https://github.com/probonopd/AppImageKit/tree/master/LibcWrapGenerator)。

当为Subsurface项目制作AppImage应用时，我使用 __CentOS 6__ 得到了很好的结果。这个发行版本不是最新的（CentOS的当前major版本减1），而[EPEL](https://fedoraproject.org/wiki/EPEL)和[devtools-2](http://people.centos.org/tru/devtools-2/)（Red Hat Developer Toolset 2的社区版本）中也有最新的Qt和现代编译器。当使用它进行编译时，我发现生成的二进制文件可在各种系统上运行，包括 __debian oldstable__ （wheezy）。

一定要查看[https://github.com/probonopd/AppImages](https://github.com/probonopd/AppImages)这里的内容，里面详细描述了probonopd(AppImage项目发起人)是如何通过`travis-ci`, `docker`,`docker-hub`,和`bintray`构建和托管AppImage应用的，一切都在云上完成。特别需要注意的是`Subsurface`和`Scribus`这个项目的细节。

链接[ https://github.com/probonopd/AppImageKit/wiki/Docker-Hub-Travis-CI-Workflow]( https://github.com/probonopd/AppImageKit/wiki/Docker-Hub-Travis-CI-Workflow) 详细介绍了如何通过`GitHub 仓库`, `Docker Hub`,和`Travis CI`组建一个全自动，连续的AppImage应用构建工作流。

你可能在考虑是否可以静态地链接某些外部库,请参考Debian项目如下所做的[https://lintian.debian.org/tags/embedded-library.html](https://lintian.debian.org/tags/embedded-library.html)

##### libstdc++.so.6

一些项目需要更新的C++标准来构建它们。为了保持glibc依赖性低，你可以在较旧的发行版上构建一个更新的GCC版本，并使用它来编译该项目。不过这么一来这个项目就需要一个更新的`libstdc++.so.6`。但集成`libstdc++.so.6`在大多数情况下会破坏安装到系统中的发行版的兼容性。所以盲目集成依赖库是不可靠的。虽然在少数情况下这主要是`libstdc++.so.6`的一个问题，但这也可能发生在`libgcc_s.so.1`中。这是因为这两个库都是GCC的一部分。你必须先知道主机系统的库版本，再在应用程序启动之前决定是否集成依赖库。这可以用[AppImageKit-checkrt](https://github.com/darealshinji/AppImageKit-checkrt/)处理。它将在AppImage或AppDir中搜索`usr/optional/libstdc++/libstdc++.so.6`和`usr/optional/libgcc_s/libgcc_s.so.1`，找到之后会与系统的版本进行比较，必要时将路径追加到`LD_LIBRARY_PATH`前面。

##### 测试

为了确保AppImage在预期的基础系统环境上运行，应该对它们进行彻底的测试。以下测试程序高效又有效：获取之前版本（当前版本的上一个版本，译者注）的Ubuntu，Fedora和openSUSE Live CD并在那里测试你的AppImage。在三个最大的发行版测试增加了AppImage在其他发行版上运行的可能性。使用之前的版本确保你的用户可能尚未升级到最新版本，但仍可以运行你的AppImage。使用Live CD的优点是，与安装的系统不同，你始终拥有一个可以轻松复制的纯净状态的系统。大多数开发人员只是在他们的主要工作系统上测试他们的软件，这些系统往往通过安装额外的软件包进行了大量的定制。通过在Live CD上进行测试，你可以确定最终用户将获得尽可能最好的体验。

我通常使用Live CD的ISO，挂载并chroot进入，然后在那里运行AppImage。这样，对于每一个需要支持的基本系统（发行版）只需占用大约700 MB空间，并且只需换一个ISO文件即可轻松升级到较新的版本。下面的脚本为 Ubuntu-like（基于Casper的）和Fedora-like的（基于Dract的）Live ISO 自动完成这些：

```
sudo ./AppImageAssistant.AppDir/testappimage /path/to/elementary-0.2-20110926.iso ./AppImageAssistant.AppImage
```

#### 常见的错误

请 __不要__ 将AppImage归档压缩到另一个文件内，例如`.zip`或`.tar.gz`。尽管这免去了需要用户设置运行权限这步操作，但是这会破坏与可选的“appimaged”守护进程的桌面集成等等。此外，AppImage格式的优点是你永远不需要解压任何东西。此外，将AppImage封装成某种形式的压缩包，将导致无法将你的AppImage应用添加到AppImage项目的[可用应用目录](https://github.com/AppImage/AppImageHub) 。

#### 环境变量

通常情况下，AppRun在执行应用程序之前需设置一些变量，如`LD_LIBRARY_PATH`。虽然在大多数情况下这就足够了，但是如果应用程序需调用在基本系统中的其他应用程序，也就是在AppImage之外，可能会导致问题。 KDevelop就是这样一个应用程序的例子。在这些情况下，[appimage-exec-wrapper](https://cgit.kde.org/scratch/brauch/appimage-exec-wrapper.git/)库可以与AppImage分发机制一起使用。将库放在AppImage的某处，并在启动应用程序之前指向`LD_PRELOAD`。每当应用程序通过`execv()`或`execve()`调用一个子进程时，这个包装程序就会拦截这个调用，看看子进程是否位于捆绑的AppDir之外。如果是这样的话，那么在启动该过程之前，包装器将试图撤销对环境变量所做的任何更改，因为你可能不打算使用，例如你先前为你的应用程序设置的`LD_LIBRARY_PATH`。另一方面，[linuxdeployqt](https://github.com/probonopd/linuxdeployqt)并没有设置`LD_LIBRARY_PATH`而是[在$ORIGIN设置库的RPATH和程序的依赖](https://nixos.org/patchelf.html)。

#### 处理器架构

#### ![question-logo](https://raw.githubusercontent.com/encharm/Font-Awesome-SVG-PNG/master/black/png/48/question-circle.png)是否可以构建适用于armhf或arm64架构的AppImage应用？

__是的__，你可以编译适用于这些体系架构的AppImageKit工具。 [Open Build Service](https://github.com/AppImage/AppImageKit/wiki/Using-Open-Build-Service) 已经支持这点。

#### ![question-logo](https://raw.githubusercontent.com/encharm/Font-Awesome-SVG-PNG/master/black/png/48/question-circle.png)是否可以建立支持多构架（比如x86_64 + armhf）的AppImage应用？

__不行__，每个体系结构都需要一个独立的AppImage结构。[FatELF](https://icculus.org/fatelf/)可以解决这个问题，但是不会合并到主线内核中，所以目前还不是一个可用选项。

#### 分发AppImage应用

我们认同你把AppImage应用下载链接放在下载页里作为Linux程序的解决方案，并且和windows的exe，macos的dmg并列，如下图：
![download page example](https://user-images.githubusercontent.com/2480569/35162112-287bff54-fd3a-11e7-8893-139638af600c.png)

对于开源项目，我们建议通过[GitHub Release](https://help.github.com/articles/creating-releases/)的方式发布你的AppImage应用。

或者你也可以把应用添加到[AppImage应用中心](https://appimage.github.io/apps/)，,一个来源多样的可用AppImage应用集合。

即使在开放源代码许可下，以源代码或二进制形式分发或使用代码也会产生一定的法律义务，例如发布相应的源代码，为GPL许可的二进制文件编写说明，以及显示版权声明和免责声明。作为AppImage应用程序的作者，你有责任遵守你在AppImage中包含的任何第三方依赖关系的所有许可证，并确保他们的许可证和源代码在需要时一起提供与发行二进制文件。 AppImageKit本身是在宽松的MIT许可下发布的。
