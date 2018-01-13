# linuxdeployqt

`linuxdeployqt`是一个在Linux下为Qt应用部署的工具，将应用程序作为输入，并将复制的自包含的Qt库和插件和应用程序捆成一束。这可以使用[fpm](https://github.com/probonopd/linuxdeployqt/issues/9)将跨平台的deb和rpm包转化为[AppImage](http://appimage.org/)。

## 与macdeployqt的差异
这个工具在概念上基于Qt工具包中的的[Mac部署工具](http://doc.qt.io/qt-5/osx-deployment.html)，macdeployqt，但是在逻辑上进行了略微的调整，加入了Linux所需的其他工具。

* 代替macOS的`.app`包，这会为Linux生成一个[AppDir](http://rox.sourceforge.net/desktop/AppDirs.html)
* 代替macOS的`.dmg`映像文件，这会产生一个Linux的[AppImage](http://appimage.org/)，与dmg非常相似，但会直接执行包含的应用程序，而不是只打开一个桌面窗口，选择应用的启动位置（MacOS用户应该明白讲的是什么，译者注）

## 已知的问题

__可能还不能完全正常工作.__ 有关已知问题，参见[GitHub问题](https://github.com/probonopd/linuxdeployqt/issues)。请仔细使用，以输出最详细日志模式运行，提交issues并pull requests。我们对您的帮助表示感谢。

## 安装

请从[Releases](https://github.com/probonopd/linuxdeployqt/releases)页面下载 __linuxdeployqt-x86_64.AppImage__ 并且运行`chmod a + x`。如果您想从源码编译生成 `linuxdeployqt`，请参阅[BUILDING.md](https://github.com/probonopd/linuxdeployqt/blob/master/BUILDING.md)。

## 用法

```
用法：linuxdeployqt app-binary [options]

参数：
   -verbose = <0-3>：0 = 无输出，1 = 错误/警告（默认），2 = 正常，3 = debug
   -no-plugins：忽略部署插件
   -appimage：创建一个AppImage
   -no-strip：不在二进制文件上运行“strip”
   -bundle-non-qt-libs：集成非核心、非Qt的库
   -executable = <path>：让给定的可执行文件也使用已部署的库
   -qmldir = <path>：扫描QML导入用来从给定目录中捆绑，使用Qt的qmlimportscanner
   -always-overwrite：即使目标文件存在，也可以复制文件
   -qmake = <path>：使用qmake的可执行文件
   -no-translations：忽略部署翻译文件

linuxdeployqt将一个self-contained(自包含)应用程序作为标的并使之生效
通过在Qt库和插件中复制应用程序使用。
```

#### 最简单的例子

鉴于AppImage应该提供一个快捷方式，`linuxdeployqt`可以使用它来确定构建的参数。

`linuxdeployqt ./path/to/appdir/usr/share/application_name.desktop`

_desktop_ 指定要运行的可执行文件（带有`EXEC =`），应用程序的名称和图标。
请参阅[桌面文件规范](https://specifications.freedesktop.org/desktop-entry-spec/desktop-entry-spec-latest.html)。

有关更详细的示例，请参阅下面的“使用Linuxdeployqt与Travis CI”。

#### 检查库包含

在Qt Creator中打开并构建您的应用程序。使用命令行运行它，并用`ldd`来检查它，以确保从正确的位置的正确的库被加载，因为`linuxdeployqt`将在内部使用`ldd`来确定从哪里将库复制到bundle中。

#### QMake配置

__重要：__ 默认情况下，`linuxdeployqt`部署$PATH指向的qmake指向Qt实例，所以确保它是正确的。在运行`linuxdeployqt`工具之前，验证qmake是否找到了正确的Qt实例：

```
qmake -v

QMake版本3.0
在 /tmp/.mount_QtCreator-5.7.0-x86_64/5.7/gcc_64/lib 中使用5.7.0的Qt版本
```

如果这并没有显示你想绑定的Qt实例的正确路径，那么调整你的`$PATH`来找到正确的`qmake`。

或者，使用`-qmake`命令行选项将工具直接指向要使用的qmake可执行文件。

#### 删除不必要的文件

在运行linuxdeployqt之前，删除不希望从构建目录分发的不需要的文件或许是明智的做法。这些可能会在构建过程中自动生成。你可以像这样删除它们：

```
find $HOME /build - * - * _ Qt_ * \（-name“moc_ *” - or - “* .o” - or - “qrc_ *” - or - name“Makefile *” - or - name “* .a“\）-exec rm {} \;
```

另外，你可以使用`$DESTDIR`。

##使用Linuxdeployqt与Travis CI

`linuxdeployqt`的常见用例是在`make`命令后面的Travis CI上使用它。以下示例说明如何在Travis CI中使用`linuxdeployqt`。创建一个类似于这个文件的`.travis.yml`文件（一定要定制它，例如，把'APPNAME`改成你的应用程序的名字，因为它是在`.desktop`文件的Name = ）：

```
language: cpp
compiler: gcc
sudo: require
dist: trusty

before_install:
  - sudo add-apt-repository ppa:beineri/opt-qt592-trusty -y
  - sudo apt-get update -qq

install:
  - sudo apt-get -y install qt59base
  - source /opt/qt*/bin/qt*-env.sh

script:
  - qmake CONFIG+=release PREFIX=/usr
  - make -j$(nproc)
  - make INSTALL_ROOT=appdir -j$(nproc) install ; find appdir/
  - wget -c -q "https://github.com/probonopd/linuxdeployqt/releases/download/continuous/linuxdeployqt-continuous-x86_64.AppImage"
  - chmod a+x linuxdeployqt-continuous-x86_64.AppImage
  - unset QTDIR; unset QT_PLUGIN_PATH ; unset LD_LIBRARY_PATH
  - export VERSION=$(git rev-parse --short HEAD) # linuxdeployqt uses this for naming the file
  - ./linuxdeployqt-continuous-x86_64.AppImage appdir/usr/share/applications/*.desktop -bundle-non-qt-libs
  - ./linuxdeployqt-continuous-x86_64.AppImage appdir/usr/share/applications/*.desktop -appimage

after_success:
  - find ./appdir -executable -type f -exec ldd {} \; | grep " => /usr" | cut -d " " -f 2-3 | sort | uniq
  - # curl --upload-file ./APPNAME*.AppImage https://transfer.sh/APPNAME-git.$(git rev-parse --short HEAD)-x86_64.AppImage
  - wget -c https://github.com/probonopd/uploadtool/raw/master/upload.sh
  - bash upload.sh ./APPNAME*.AppImage*
  
branches:
  except:
    - # Do not build tags that we create when we upload to GitHub Releases
    - /^(?i:continuous)/
```

当你保存你的更改时，Travis CI会为你创建并上传一个AppImage。不过，可能一些微调仍然是必需的。

为了达到这个目的，如果你还没有这样做，你需要为你的版本库启用[Travis CI](https://travis-ci.org/getting_started)。

默认情况下，Qt Creator生成的qmake`.pro`文件不支持`make install`。在这种情况下，你会得到这样的错误:

```
make：Nothing to be done for `install'.
find: `appdir/': No such file or directory
```

如果`qmake`不允许`make install`或者无法安装桌面文件和图标，那么你需要改变`.pro`文件，类似于https://github.com/probonopd/FeedTheMonkey/blob/main/FeedTheMonkey.pro。

```
  - make INSTALL_ROOT=appdir install ; find appdir/
```

__CMake__ 需要`DESTDIR`来代替：

```
  - cmake . -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr
  - make -j$(nproc)
  - make DESTDIR=appdir -j$(nproc) install ; find appdir/
```

__autotools__（“find...”将会花费大量的时间）也要`DESTDIR`，如果使用一个绝对的链接，我们可以用readlink链接它：

```
  - ./configure --prefix=/usr
  - make -j$(nproc)
  - make install DESTDIR=$(readlink -f appdir) ; find appdir/
```

注意，如果你遇到

```
qmake PREFIX=/usr CONFIG+=use_qt_paths
```

在这里，`CONFIG + = use_qt_paths`需要被删除，否则在使用beineri ppa的时候，它会在`/opt/qt58`的Qt安装路径下安装所有的东西。

不同之处在于你正在构建Qt库，应该将Qt库安装到Qt驻留在你的系统的同一个位置，从`linuxdeployqt`选择它。

### 在GitHub上发送合并请求

`linuxdeployqt`非常适合上游应用程序项目，它们可以快速地以二进制形式向Linux用户发布其软件，而且没有太多的开销。如果您希望看到特定的应用程序使用`linuxdeployqt`，那么发送合并请求可能是上游应用程序项目考虑的一个选项。对于合并请求，您可以使用以下模板文本，但一定要将其定制到相关项目中。

```
这个PR合并后，将在每个`git push`上的[Travis CI]（https://travis-ci.org/）上编译这个应用程序，并上传一个[AppImage]（http://appimage.org/）到你的GitHub发布页面。

提供[AppImage]（http://appimage.org/）将具有以下优点：
- 作为AppImage打包的应用程序可以在许多发行版上运行（包括Ubuntu，Fedora，openSUSE，CentOS，basicOS，Linux Mint等）
- 一个应用程序=一个文件=用户可以非常简单的使用：只需下载一个AppImage文件，[使其可执行]（http://discourse.appimage.org/t/how-to-make-an-appimage-executable/80） ，然后运行
- 不需要解包或安装
- 不需要root权限
- 没有系统库改变
- 开箱即用，无需安装运行时(run time)
- 可选择与桌面集成`appimaged`
- 可选择二进制增量更新，例如，用于连续构建（仅下载二进制比较）使用AppImageUpdate
- 可以选择GPG2 - 签署你的AppImages（在文件内）
- 在Live ISOs上工作
- 双启动多个发行版时可以使用相同的AppImages
- 可以展示在AppImage的[AppImageHub]（https://appimage.github.io/apps）主目录中
- 可以使用“--appimage-extract”参数作为自解压压缩归档

[这里是一个概述]（https://appimage.github.io/apps）已经分发上游提供的官方AppImages项目。

__PLEASE注意：__为了这个工作，你需要为你的仓库启用Travis CI（这里描述）（https://travis-ci.org/getting_started）__prior合并this__，如果你还没有这样做的话。此外，您需要在Travis CI中设置“GITHUB_TOKEN”才能正常工作;请参阅https://github.com/probonopd/uploadtool。
如果您希望在项目历史记录中只看到一个合并请求条目，请启用[此GitHub功能]（https://help.github.com/articles/configuring-commit-squashing-for-pull-requests /）在您的回购。它允许您在合并时压缩（合并）提交。

如果您有任何问题，可以和AppImage开发人员在IRC:irc.freenode.net上的#AppImage频道与其交流。
```
## 使用linuxdeployqt的项目

这些项目已经在使用[Travis CI](http://travis-ci.org/)和linuxdeployqt来提供AppImage的构建：
- https://github.com/probonopd/ImageMagick
- https://github.com/Subsurface-divelog/subsurface/
- https://github.com/jimevins/glabels-qt
- https://travis-ci.org/NeoTheFox/RepRaptor
- https://github.com/electronpass/electronpass-desktop
- https://github.com/lirios/browser
- https://github.com/jeena/FeedTheMonkey
- https://github.com/labsquare/fastQt/
- https://github.com/sqlitebrowser/sqlitebrowser/
- https://github.com/neuro-sys/tumblr-downloader-client
- https://github.com/LongSoft/UEFITool
- https://github.com/dannagle/PacketSender
- https://github.com/nuttyartist/notes
- https://github.com/leozide/leocad/
- https://github.com/Blinkinlabs/PatternPaint
- https://github.com/fathomssen/redtimer
- https://github.com/coryo/amphetype2
- https://github.com/chkmue/MyQtTravisTemplateProject
- https://github.com/chkmue/qttravisCI_1
- https://github.com/eteran/edb-debugger
- https://github.com/crapp/labpowerqt/
- https://github.com/probonopd/linuxdeployqt/显然;-)
- https://github.com/xdgurl/xdgurl
- https://github.com/QNapi/qnapi

该项目已经在定制的Jenkins工作流程中使用了linuxdeployqt：
- https://github.com/appimage-packages/

这些项目已经在使用linuxdeployqt：
- 用于Linux的Autodesk EAGLE http://www.autodesk.com/products/eagle/free-download
- https://github.com/bjorn/tiled/
- https://github.com/evpo/EncryptPad
- https://github.com/grahamrow/Muview2
- https://github.com/freemountain/quark/
- https://github.com/Mr0815/geraetepruefung/

GitLab上的这个项目使用linuxdeployqt：

- https://gitlab.com/rpdev/opentodolist

这些可以使用linuxdeployqt成功捆绑：

- https://github.com/probonopd/tiled/blob/patch-1/.travis.yml
- https://gitlab.com/rpdev/opentodolist/issues/96


## 贡献

一个非常好的贡献方式就是是将合并请求发送到您希望使用linuxdeployqt的应用程序项目，如上所述。也欢迎您为linuxdeployqt开发本身作出贡献。请在[forum](http://discourse.appimage.org/t/linuxdeployqt-new-linux-deployment-tool-for-qt/57)或使用GitHub问题和Pull Requests进行讨论。

## 联系

开发者在irc.freenode.net的#AppImage频道

# linuxdeployqt

This Linux Deployment Tool for Qt, `linuxdeployqt`, takes an application as input and makes it self-contained by copying in the Qt libraries and plugins that the application uses into a bundle. This can optionally be put into an [AppImage](http://appimage.org/), and, using [fpm](https://github.com/probonopd/linuxdeployqt/issues/9), into cross-distro deb and rpm packages.

## Differences to macdeployqt
This tool is conceptually based on the [Mac Deployment Tool](http://doc.qt.io/qt-5/osx-deployment.html), `macdeployqt` in the tools applications of the Qt Toolkit, but has been changed to a slightly different logic and other tools needed for Linux.

* Instead of an `.app` bundle for macOS, this produces an [AppDir](http://rox.sourceforge.net/desktop/AppDirs.html) for Linux
* Instead of a `.dmg` disk image for macOS, this produces an [AppImage](http://appimage.org/) for Linux which is quite similar to a dmg but executes the contained application rather than just opening a window on the desktop from where the application can be launched

## Known issues

__This may not be fully working yet.__ See [GitHub Issues](https://github.com/probonopd/linuxdeployqt/issues) for known issues. Use with care, run with maximum verbosity, submit issues and pull requests. Help is appreciated.

## Installation

Please download __linuxdeployqt-x86_64.AppImage__ from the [Releases](https://github.com/probonopd/linuxdeployqt/releases) page and `chmod a+x` it. If you would like to build `linuxdeployqt` from source instead, see [BUILDING.md](https://github.com/probonopd/linuxdeployqt/blob/master/BUILDING.md).

## Usage

```
Usage: linuxdeployqt app-binary [options]

Options:
   -verbose=<0-3>      : 0 = no output, 1 = error/warning (default), 2 = normal, 3 = debug
   -no-plugins         : Skip plugin deployment
   -appimage           : Create an AppImage
   -no-strip           : Don't run 'strip' on the binaries
   -bundle-non-qt-libs : Also bundle non-core, non-Qt libraries
   -executable=<path>  : Let the given executable use the deployed libraries too
   -qmldir=<path>      : Scan for QML imports to bundle from the given directory, determined by Qt's qmlimportscanner
   -always-overwrite   : Copy files even if the target file exists
   -qmake=<path>       : The qmake executable to use
   -no-translations    : Skip deployment of translations

linuxdeployqt takes an application as input and makes it
self-contained by copying in the Qt libraries and plugins that
the application uses.
```

#### Simplest example

Given that a desktop file should be provided with an AppImage, `linuxdeployqt` can use that to determine the parameters of the build.

`linuxdeployqt ./path/to/appdir/usr/share/application_name.desktop`

Where the _desktop_ file specifies the executable to be run (with `EXEC=`), the name of the applications and an icon.
See [desktop file specification](https://specifications.freedesktop.org/desktop-entry-spec/desktop-entry-spec-latest.html).

For a more detailed example, see "Using linuxdeployqt with Travis CI" below.

#### Checking library inclusion

Open in Qt Creator and build your application. Run it from the command line and inspect it with `ldd` to make sure the correct libraries from the correct locations are getting loaded, as `linuxdeployqt` will use `ldd` internally to determine from where to copy libraries into the bundle.

#### QMake configuration

__Important:__ By default, `linuxdeployqt` deploys the Qt instance that qmake on the $PATH points to, so make sure that it is the correct one. Verify that qmake finds the correct Qt instance like this before running the `linuxdeployqt` tool:

```
qmake -v

QMake version 3.0
Using Qt version 5.7.0 in /tmp/.mount_QtCreator-5.7.0-x86_64/5.7/gcc_64/lib
```

If this does not show the correct path to your Qt instance that you want to be bundled, then adjust your `$PATH` to find the correct `qmake`.

Alternatively, use the `-qmake` command line option to point the tool directly to the qmake executable to be used.

#### Remove unecessary files

Before running linuxdeployqt it may be wise to delete unneeded files that you do not wish to distribute from the build directory. These may be autogenerated during the build. You can delete them like so:

```
find $HOME/build-*-*_Qt_* \( -name "moc_*" -or -name "*.o" -or -name "qrc_*" -or -name "Makefile*" -or -name "*.a" \) -exec rm {} \;
```

Alternatively, you could use `$DESTDIR`.

## Using linuxdeployqt with Travis CI

A common use case for `linuxdeployqt` is to use it on Travis CI after the `make` command. The following example illustrates how to use `linuxdeployqt` with Travis CI. Create a `.travis.yml` file similar to this one (be sure to customize it, e.g., change `APPNAME` to the name of your application as it is spelled in the `Name=` entry of the `.desktop` file):

```
language: cpp
compiler: gcc
sudo: require
dist: trusty

before_install:
  - sudo add-apt-repository ppa:beineri/opt-qt592-trusty -y
  - sudo apt-get update -qq

install:
  - sudo apt-get -y install qt59base
  - source /opt/qt*/bin/qt*-env.sh

script:
  - qmake CONFIG+=release PREFIX=/usr
  - make -j$(nproc)
  - make INSTALL_ROOT=appdir -j$(nproc) install ; find appdir/
  - wget -c -q "https://github.com/probonopd/linuxdeployqt/releases/download/continuous/linuxdeployqt-continuous-x86_64.AppImage"
  - chmod a+x linuxdeployqt-continuous-x86_64.AppImage
  - unset QTDIR; unset QT_PLUGIN_PATH ; unset LD_LIBRARY_PATH
  - export VERSION=$(git rev-parse --short HEAD) # linuxdeployqt uses this for naming the file
  - ./linuxdeployqt-continuous-x86_64.AppImage appdir/usr/share/applications/*.desktop -bundle-non-qt-libs
  - ./linuxdeployqt-continuous-x86_64.AppImage appdir/usr/share/applications/*.desktop -appimage

after_success:
  - find ./appdir -executable -type f -exec ldd {} \; | grep " => /usr" | cut -d " " -f 2-3 | sort | uniq
  - # curl --upload-file ./APPNAME*.AppImage https://transfer.sh/APPNAME-git.$(git rev-parse --short HEAD)-x86_64.AppImage
  - wget -c https://github.com/probonopd/uploadtool/raw/master/upload.sh
  - bash upload.sh ./APPNAME*.AppImage*
  
branches:
  except:
    - # Do not build tags that we create when we upload to GitHub Releases
    - /^(?i:continuous)/
```

When you save your change, then Travis CI should build and upload an AppImage for you. More likely than not, some fine-tuning will still be required.

For this to work, you need to enable Travis CI for your repository as [described here](https://travis-ci.org/getting_started) __prior to merging this__, if you haven't already done so.

By default, qmake `.pro` files generated by Qt Creator unfortunately don't support `make install` out of the box. In this case you will get

```
make: Nothing to be done for `install'.
find: `appdir/': No such file or directory
```

If `qmake` does not allow for `make install` or does not install the desktop file and icon, then you need to change your `.pro` file it similar to https://github.com/probonopd/FeedTheMonkey/blob/master/FeedTheMonkey.pro.

```
  - make INSTALL_ROOT=appdir install ; find appdir/
```

__CMake__ wants `DESTDIR` instead:

```
  - cmake . -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr
  - make -j$(nproc)
  - make DESTDIR=appdir -j$(nproc) install ; find appdir/
```

__autotools__ (the dinosaur that spends precious minutes "checking...") wants `DESTDIR` too but insists on an absolute link which we can feed it using readlink:

```
  - ./configure --prefix=/usr
  - make -j$(nproc)
  - make install DESTDIR=$(readlink -f appdir) ; find appdir/
```

Caution if you encounter

```
qmake PREFIX=/usr CONFIG+=use_qt_paths
```

Here, `CONFIG+=use_qt_paths` needs to be removed, otherwise it will install everything under the Qt installation paths in `/opt/qt58` when using the beineri ppa.

The exception is that you are building Qt libraries that _should_ be installed to the same location where Qt resides on your system, from where it will be picked up by `linuxdeployqt`.

### Sending Pull Requests on GitHub

`linuxdeployqt` is great for upstream application projects that want to release their software in binary form to Linux users quickly and without much overhead. If you would like to see a particular application use `linuxdeployqt`, then sending a Pull Request may be an option to get the upstream application project to consider it. You can use the following template text for Pull Requests but make sure to customize it to the project in question.

```
This PR, when merged, will compile this application on [Travis CI](https://travis-ci.org/) upon each `git push`, and upload an [AppImage](http://appimage.org/) to your GitHub Releases page.

Providing an [AppImage](http://appimage.org/) would have, among others, these advantages:
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

__PLEASE NOTE:__ For this to work, you need to enable Travis CI for your repository as [described here](https://travis-ci.org/getting_started) __prior to merging this__, if you haven't already done so. Also, You need to set up `GITHUB_TOKEN` in Travis CI for this to work; please see https://github.com/probonopd/uploadtool.
If you would like to see only one entry for the Pull Request in your project's history, then please enable [this GitHub functionality](https://help.github.com/articles/configuring-commit-squashing-for-pull-requests/) on your repo. It allows you to squash (combine) the commits when merging.

If you have questions, AppImage developers are on #AppImage on irc.freenode.net.
```
## Projects using linuxdeployqt

These projects are already using [Travis CI](http://travis-ci.org/) and linuxdeployqt to provide AppImages of their builds:
- https://github.com/probonopd/ImageMagick
- https://github.com/Subsurface-divelog/subsurface/
- https://github.com/jimevins/glabels-qt
- https://travis-ci.org/NeoTheFox/RepRaptor
- https://github.com/electronpass/electronpass-desktop
- https://github.com/lirios/browser
- https://github.com/jeena/FeedTheMonkey
- https://github.com/labsquare/fastQt/
- https://github.com/sqlitebrowser/sqlitebrowser/
- https://github.com/neuro-sys/tumblr-downloader-client
- https://github.com/LongSoft/UEFITool
- https://github.com/dannagle/PacketSender
- https://github.com/nuttyartist/notes
- https://github.com/leozide/leocad/
- https://github.com/Blinkinlabs/PatternPaint
- https://github.com/fathomssen/redtimer
- https://github.com/coryo/amphetype2
- https://github.com/chkmue/MyQtTravisTemplateProject
- https://github.com/chkmue/qttravisCI_1
- https://github.com/eteran/edb-debugger
- https://github.com/crapp/labpowerqt/
- https://github.com/probonopd/linuxdeployqt/ obviously ;-)
- https://github.com/xdgurl/xdgurl
- https://github.com/QNapi/qnapi

This project is already using linuxdeployqt in a custom Jenkins workflow:
- https://github.com/appimage-packages/

These projects are already using linuxdeployqt:
- Autodesk EAGLE for Linux http://www.autodesk.com/products/eagle/free-download
- https://github.com/bjorn/tiled/
- https://github.com/evpo/EncryptPad
- https://github.com/grahamrow/Muview2
- https://github.com/freemountain/quark/
- https://github.com/Mr0815/geraetepruefung/

This project on GitLab uses linuxdeployqt:

- https://gitlab.com/rpdev/opentodolist

These can be bundled successfully using linuxdeployqt:

- https://github.com/probonopd/tiled/blob/patch-1/.travis.yml
- https://gitlab.com/rpdev/opentodolist/issues/96


## Contributing

One great way to contribute is to send Pull Requests to the application projects you'd like to see use linuxdeployqt, as described above. You are also welcome to contribute to linuxdeployqt development itself. Please discuss in the [forum](http://discourse.appimage.org/t/linuxdeployqt-new-linux-deployment-tool-for-qt/57) or using GitHub issues and Pull Requests.

## Contact

The developers are in the channel #AppImage on irc.freenode.net
