## 创建AppImages

创建AppImage的一般工作流程涉及以下步骤：

1. __收集合适的二进制文件__ 如果应用程序已经编译完成，则可以使用现有的二进制文件（例如：包含在.tar .gz，deb或rpm包中的二进制文件）。请注意，二进制文件编译环境绝不能比你的编译环境发行版更新（如果你的工作环境是Ubuntu 9.10，你不应该使用在Ubuntu 10.04上编译的二进制文件）。使用上游发行版用于AppImages特殊构建可能是有利的，尽管这不是必需的。

2. __收集所有依赖的二进制文件__ 收集你的期望支持的 **基本** 操作系统所没有的那部分。例如，如果你期望支持Ubuntu，Fedora和openSUSE，那么你需要收集你的应用程序运行所需单Ubuntu，Fedora和openSUSE所需库和其他依赖项的合集。

3. __根据二进制文件创建一个可用的AppDir文件夹__ 文件夹名称一般命名为xxx.AppDir(xxx根据二进制文件或个人喜好而定)，打包AppImage前先执行文件夹内AppRun确保应用程序在你的工作环境是正常运行的。

4. __把AppDir文件夹转换成AppImage__ 这将你的xxx.AppDir文件夹内的内容压缩成一个自挂载和自执行的文件。

5. __测试你的AppImage__ 在所有期望支持的 **基础** 操作系统测试打包好的AppImage程序。这一步非常重要，各种发行版的都有细微差异。大多数情况下刚好AppImages可以在各种发行版上运行，但这种情况一般不会自动发生，还需要仔细的手动调整才会完美支持。

理论上可以手动完成上述步骤，但AppImageKit内提供的工具会减轻你的工作量。

现在，实际上有多种不同的方法来生成应用程序的AppImage：

**使用 Open Build Service**。建议将此方法用在开源项目上，因为它允许你利用现有的开放式构建服务基础架构，安全和许可证遵从性流程。请参阅 https://github.com/probonopd/AppImageKit/wiki/Using-Open-Build-Service 以了解如何使用它。

如果使用Open Build Service不是你的选择，还有其他方法可以生成AppImages：

1. **转换现有的二进制包**。如果你已经有最新的软件包，这个方法可能是最简单的，理想的情况是 earlier 或 trusty ppa  或者debian的oldstable源的包。在这种情况下，你只要可以编写一个小`.yml`文件就可以了，很多工作都是由AppImage自动完成的。 [这个脚本](https://raw.githubusercontent.com/probonopd/AppImages/master/recipes/meta/Recipe)可以运行.yml文件。 [见示例](https://github.com/probonopd/AppImages/tree/master/recipes/meta) **或**

2. **将你的Travis CI版本打包为AppImages**。 如果你已经在 Travis CI 上持续构建，这个选项可能是最简单的。在这种情况下，你可以编写一个小的脚本文件，很多工作都是由AppImage自动完成的。 [见示例](https://github.com/search?utf8=%E2%9C%93&q=%22Package+the+binaries+built+on+Travis-CI+as+an+AppImage%22&type=Code&ref=searchresults) **或**

3. **用linuxdeployqt运行Qt应用程序**。如果你有一个基于Qt的应用程序，这个选项可能是最简单的。在这种情况下，你可能已经在使用`windeployqt`和`macdeployqt`，现在可以使用`linuxdeployqt`与`-appimage`参数运行，很多工作都是由AppImage自动完成的。[参见示例](//github.com/coryo/amphetype2/blob/2d41de3b0c19ab9286672ff0d6a7c11eadc13d9c/.travis/deploy.sh) **或**

4. **使用 electron-builder**。如果你有一个基于Electron的应用程序，这个选项可能是最简单的。在这种情况下，将AppImage定义为Linux的目标（在最新版本的electron-builder中是默认的），很多工作都是由AppImage自动完成的的。 [见示例](https://github.com/search?utf8=%E2%9C%93&q=electron-builder+linux+target+appimage&type=Code&ref=searchresults)**或**

5. **手动创建一个AppDir** 再把它变成一个AppImage。先看下面的例子，再看关于将某些应用程序或应用程序类型绑定为AppImages的例子，或者从右侧的**页面**菜单中查看示例。

## 手动创建一个AppDir

实际上，你可能永远不会手动做这个。所以这主要是为了说明这个概念。

创建一个AppDir结构，看起来（至少）如下所示：

```
MyApp.AppDir/
MyApp.AppDir/AppRun
MyApp.AppDir/myapp.desktop
MyApp.AppDir/myapp.png
MyApp.AppDir/usr/bin/myapp
MyApp.AppDir/usr/lib/libfoo.so.0
```
AppRun 文件可以是脚本或可执行文件。它设置所需的环境变量，例如 `$ PATH` 并启动有效载荷应用程序。你可以编写你自己的，但是在大多数情况下最简单的（也是最不容易出差错的）是用这个版本库中的预编译的版本。

当然，如果你的应用程序不需要库，或者你的应用程序需要的所有库都已包含在你所针对的每个基本操作系统中，那么你可以省去库。

你的二进制文件 myapp 不得包含任何硬编码的路径，以防止其被重定位。你可以运行 `strings MyApp.AppDir/usr/bin/myapp | grep /usr` 。如果返回一些东西，那么你需要编程修改你的应用程序（例如，使用[binreloc](https://github.com/ximion/binreloc)，或者用`QString QCoreApplication::applicationDirPath()` ）。如果你不想更改应用程序的源代码和/或不想重新编译应用程序，也可以使用命令`sed -i -e 's|/usr|././|g' MyApp.AppDir/usr/bin/myapp`修补二进制文件。通常只要应用程序没有执行`chdir()`就没为题，因为这可能会使`././`不再指向`$APPDIR/usr`。你可以运行`strace -echdir -f ./AppRun`来查看应用程序是否调用了`chdir()`（99％的GUI应用程序没有）。

另请参阅 https://www.gnu.org/software/gnulib/manual/html_node/Supporting-Relocation.html 。长期以来，GNU软件包软件包无法重定位扽问题折磨了许多用户。relocatable-prog模块旨在简化GNU程序可重定位的过程。

__注意：__ 对于应用程序依赖的任何助手二进制文件和/或库也是如此。你可以用 `cd MyApp.AppDir/usr/ ; find . -type f -exec sed -i -e 's|/usr|././|g' {} \; ; cd -`，这会用`././`替换所有`/usr`。

myapp.desktop（至少）应该包含：

```
[Desktop Entry]
Name=MyApp
Exec=myapp
Icon=myapp
Type=Application
Categories=Utilities;
```

一定要选择一个[注册类别](https://standards.freedesktop.org/menu-spec/latest/apa.html)，并确保你的desktop文件用`desktop-file-validate your.desktop`检验了。

然后，在AppDir上运行`appimagetool`以将其变成AppImage。你可以从这个仓库的[Releases](https://github.com/probonopd/AppImageKit/releases)获取（这来自AppImage；没错，我们吃我们自己的狗食）。

## 创建可移植的AppImages

要在大多数系统上运行AppImage，需要满足以下条件：
1. AppImage需要包含基本系统缺失的所有库和其他依赖项
2. AppImage中包含的二进制文件不能在比目标系统更新的系统上编译
3. AppImage实际上应该在其打算运行的基础系统上进行测试

### 在足够老的基本系统上编译二进制文件

不应在最新的基础系统上，而是你的AppImage要运行的最老的基础系统上编译。某些核心库（如glibc）往往会频繁地破坏旧版本基本系统的兼容性，这意味着二进制文件可以运行在较新的版本上，而在比二进制文件编译的版本旧的版本上就运行不了了。

如果你遇到这样的错误

```
failed to initialize: /lib/tls/i686/cmov/libc.so.6: version `GLIBC_2.11' not found
```

那么二进制文件是在一个比你试图运行的系统更新的系统上编译的。你应该使用在旧系统上编译的二进制文件。不幸的是，复杂的是，发行版通常只在最新的系统上编译最新版本的应用程序，这意味着你将很难找到在旧版系统上运行一些比较前沿的软件。解决这个问题的方法是在不太新的基础系统上自己编译依赖关系，和/或使用[LibcWrapGenerator]（https://github.com/probonopd/AppImageKit/tree/master/LibcWrapGenerator）。

当为Subsurface制作AppImages时，我使用__CentOS 6__得到了很好的结果。这个发行版本不是最新的（CentOS的当前major版本减1），而[EPEL](https://fedoraproject.org/wiki/EPEL)和[devtools-2](http://people.centos.org/tru/devtools-2/)（Red Hat Developer Toolset 2的社区版本）中也有最新的Qt和现代编译器。当使用它进行编译时，我发现生成的二进制文件在各种系统上运行，包括 __debian oldstable__ （wheezy）。

### libstdc++.so.6

一些项目需要更新的C++标准来构建它们。为了保持glibc依赖性低，你可以
在较旧的发行版上构建一个更新的GCC版本，并使用它来编译该项目。不过这么一来这个项目就需要一个更新的`libstdc++.so.6`。但捆绑`libstdc++.so.6`在大多数情况下会破坏安装到系统中的发行版的兼容性。所以盲目捆绑库是不可靠的。虽然在少数情况下这主要是`libstdc++.so.6`的一个问题，但这也可能发生在`libgcc_s.so.1`中。这是因为这两个库都是GCC的一部分。你必须先知道主机系统的库版本，再在应用程序启动之前决定是否使用捆绑库。这可以用[AppImageKit-checkrt](https://github.com/darealshinji/AppImageKit-checkrt/)处理。它将在AppImage或AppDir中搜索`usr/optional/libstdc++/libstdc++.so.6`和`usr/optional/libgcc_s/libgcc_s.so.1`，找到之后会与系统的版本进行比较，必要时将路径追加到`LD_LIBRARY_PATH`前面。

### 测试

为了确保AppImage在预期的基础系统上运行，应该对它们进行彻底的测试。以下测试程序既高效又有效：获取以前版本的Ubuntu，Fedora和openSUSE Live CD并在那里测试你的AppImage。使用三个最大的发行版增加了AppImage在其他发行版上运行的机会。使用以前（当前减1）版本确保你的最终用户可能尚未升级到最新版本，但仍可以运行你的AppImage。使用Live CD的优点是，与安装的系统不同，你始终拥有一个可以轻松复制的出厂状态的系统。大多数开发人员只是在他们的主要工作系统上测试他们的软件，这些系统往往通过安装额外的软件包进行大量的定制。通过在Live CD上进行测试，你可以确定最终用户将获得尽可能最好的体验。

我 loop-mount Live CD的ISO，chroot进去，然后在那里运行AppImage。这样，我需要每个支持的基本系统（分发）大约700 MB，并且只需换一个ISO文件即可轻松升级到较新的版本。下面的脚本为 Ubuntu-like的（基于Casper的）和Fedora-like的（基于Dract的）Live ISO 自动完成这些：

```
sudo ./AppImageAssistant.AppDir/testappimage /path/to/elementary-0.2-20110926.iso ./AppImageAssistant.AppImage
```

## 常见的错误

请 __不要__ 将AppImage放入另一个档案，例如`.zip`或`.tar.gz`。尽管避免用户必须设置权限可能是很诱人的，但是这会破坏与可选的“appimaged”守护进程的桌面集成等等。此外，AppImage格式的优点是你永远不需要解压任何东西。此外，将AppImage封装成某种形式的存档，导致无法将AppImage添加到https://github.com/AppImage/AppImageHub 上的可用AppImage的中央目录中。

## 环境变量

默认情况下，AppRun在执行有效载荷应用程序之前设置一些变量，如`LD_LIBRARY_PATH`。虽然在大多数情况下这是足够的，但是如果有效载荷应用程序启动驻留在基本系统中的其他应用程序，也就是在AppImage之外，可能会导致问题。 KDevelop就是这样一个应用程序的例子。在这些情况下，[appimage-exec-wrapper](https://cgit.kde.org/scratch/brauch/appimage-exec-wrapper.git/)库可以与AppImage分发机制一起使用。将库放在AppImage的某处，并在启动应用程序之前指向`LD_PRELOAD`。每当应用程序通过`execv()`或`execve()`调用一个子进程时，这个包装程序就会拦截这个调用，看看子进程是否位于捆绑的AppDir之外。如果是这样的话，那么在启动该过程之前，包装器将试图撤销对环境变量所做的任何更改，因为你可能不打算使用，例如你先前为你的应用程序设置的`LD_LIBRARY_PATH`。另一方面，[linuxdeployqt](https://github.com/probonopd/linuxdeployqt)并没有设置`LD_LIBRARY_PATH`而是[在$ORIGIN设置库的RPATH和程序的依赖](https://nixos.org/patchelf.html)。

## 处理器架构

### 我可以建立armhf或arm64 AppImages吗？

__是__，你可以编译这些体系结构的AppImageKit工具。 [Open Build Service](https://github.com/AppImage/AppImageKit/wiki/Using-Open-Build-Service)支持。

### 我可以建立支持多构架（比如x86_64 + armhf）的AppImages？

__否__，每个体系结构都需要一个AppImage。 [FatELF](https://icculus.org/fatelf/)可以解决这个问题，但是不会合并到主线内核中，所以目前还不是一个选项。

## 分发AppImages

即使在开放源代码许可下，以源代码或二进制形式分发和/或使用代码也可能产生一定的法律责任，例如分发相应的源代码，为GPL许可的二进制文件构建指令，以及显示版权声明和免责声明。作为AppImage应用程序的作者，你有责任遵守你在AppImage中包含的任何第三方依赖关系的所有许可证，并确保他们的许可证和源代码在需要时一起提供与发行二进制文件。 AppImageKit本身是在宽松的MIT许可下发布的。

## Creating AppImages

The general workflow for creating an AppImage involves the following steps:
 1. __Gather suitable binaries__. If the application has already been compiled, you can use existing binaries (for example, contained in .tar.gz, deb, or rpm archives). Note that the binaries must not be compiled on newer distributions than the ones you are targeting. In other words, if you are targeting Ubuntu 9.10, you should not use binaries compiled on Ubuntu 10.04. For upstream projects, it might be advantageous to compile special builds for use in AppImages, although this is not required.
 2. __Gather suitable binaries of all dependencies__ that are not part of the base operating systems you are targeting. For example, if you are targeting Ubuntu, Fedora, and openSUSE, then you need to gather all libraries and other dependencies that your app requires to run that are not part of Ubuntu, Fedora, and openSUSE.
 3. __Create a working AppDir__ from your binaries. A working AppImage runs your app when you execute its AppRun file.
 4. __Turn your AppDir into an AppImage__. This compresses the contents of your AppDir into a single, self-mounting and self-executable file.
 5. __Test your AppImage__ on all base operating systems you are targeting. This is an important step which you should not skip. Subtle differences in distributions make this a must. While it is possible in most cases to create AppImages that run on various distributions, this does not come automatically, but requires careful hand-tuning.

While it would theoretically be possible to do all these steps by hand, AppImageKit contains tools that greatly simplify the tasks.

Now, on a more practical note, there are different ways to generate an AppImage of your application:

**Use the Open Build Service**. This option is recommended for open source projects because it allows you to leverage the existing Open Build Service infrastructure, security and license compliance processes. See https://github.com/probonopd/AppImageKit/wiki/Using-Open-Build-Service for how to use this.

If using the Open Build Service is not an option for you, there are other ways to generate AppImages:

1. **Convert existing binary packages**. This option might be the easiest if you already have up-to-date packages in place, ideally a ppa for trusty or earlier or a debian repository for oldstable. In this case, you can write a small `.yml` file and in many cases are done with the package to AppImage conversion. [This script](https://raw.githubusercontent.com/probonopd/AppImages/master/recipes/meta/Recipe) can run .yml files. [See examples](https://github.com/probonopd/AppImages/tree/master/recipes/meta) **OR**
2. **Bundle your Travis CI builds as AppImages**. This option might be the easiest if you already have continuous builds on Travis CI in place. In this case, you can write a small scriptfile and in many cases are done with the AppImage generation. [See examples](https://github.com/search?utf8=%E2%9C%93&q=%22Package+the+binaries+built+on+Travis-CI+as+an+AppImage%22&type=Code&ref=searchresults) **OR**
3. **Run linuxdeployqt on your Qt application**. This option might be the easiest if you have a Qt-based application. In this case, you are probably already using `windeployqt` and `macdeployqt` and now can use `linuxdeployqt` in the same fashion with the `-appimage` argument and in many cases are done with the AppImage generation [See example](https://github.com/coryo/amphetype2/blob/2d41de3b0c19ab9286672ff0d6a7c11eadc13d9c/.travis/deploy.sh) **OR**
4. **Use electron-builder**. This option might be the easiest if you have an Electron-based application. In this case, you define AppImage as a target for Linux (default in the latest version of electron-builder) and in many cases are done with the AppImage generation. [See examples](https://github.com/search?utf8=%E2%9C%93&q=electron-builder+linux+target+appimage&type=Code&ref=searchresults) **OR**
5. **Manually create an AppDir** and turn it into an AppImage. Start out with the example below, then check the examples on bundling certain applications or type of applications as AppImages from the right-hand side **"Pages"** menu.

## Manually creating an AppDir

In practice, you will probably never do this by hand. So this is mainly to illustrate the concept.

Create an AppDir structure that looks (as a minimum) like this:

```
MyApp.AppDir/
MyApp.AppDir/AppRun
MyApp.AppDir/myapp.desktop
MyApp.AppDir/myapp.png
MyApp.AppDir/usr/bin/myapp
MyApp.AppDir/usr/lib/libfoo.so.0
```

The `AppRun` file can be a script or executable. It sets up required environment variables such as `$PATH` and launches the payload application. You can write your own, but in most cases it is easiest (and most error-proof) to use a precompiled one from this repository.

Of course you can leave out the library if your app does not need one, or if all libraries your app needs are already contained in every base operating system you are targeting.

Your binary, myapp, must not contain any hardcoded paths that would prevent it from being relocateable. You can check this by running `strings MyApp.AppDir/usr/bin/myapp | grep /usr`. Should this return something, then you need to modify your app programmatically (e.g., by using relative paths, using [binreloc](https://github.com/ximion/binreloc), or using `QString QCoreApplication::applicationDirPath()`). If you prefer not to change the source code of your app and/or would not like to recompile your app, you can also patch the binary, for example using the command `sed -i -e 's|/usr|././|g' MyApp.AppDir/usr/bin/myapp`. This usually works as long as the application is not doing a `chdir()` which would break this workaround, because then `././` would not be pointing to `$APPDIR/usr` any more. You can run `strace -echdir -f ./AppRun` to see whether the application is doing a `chdir()` (99% of GUI applications don't).

Also see https://www.gnu.org/software/gnulib/manual/html_node/Supporting-Relocation.html It has been a pain for many users of GNU packages for a long time that packages are not relocatable. The relocatable-prog module aims to ease the process of making a GNU program relocatable.

__Note:__ The same is true for any helper binaries and/or libraries that your app depends on. You can do so with `cd MyApp.AppDir/usr/ ; find . -type f -exec sed -i -e 's|/usr|././|g' {} \; ; cd -` which replaces all occurences of `/usr` with `././`, which simply means "here".

myapp.desktop should contain (as a minimum):

```
[Desktop Entry]
Name=MyApp
Exec=myapp
Icon=myapp
Type=Application
Categories=Utilities;
```

Be sure to pick one of the [Registered Categories](https://standards.freedesktop.org/menu-spec/latest/apa.html), and be sure that your desktop file passes validation by using `desktop-file-validate your.desktop`.

Then, run `appimagetool` on the AppDir in order to turn it into an AppImage. You can get it from this repository's [Releases](https://github.com/probonopd/AppImageKit/releases) page (it comes as an AppImage itself; yes, we eat our own dogfood).

## Creating portable AppImages

For an AppImage to run on most systems, the following conditions need to be met:
 1. The AppImage needs to include all libraries and other dependencies that are not part of all of the base systems that the AppImage is intended to run on
 2. The binaries contained in the AppImage need to be compiled on a system not newer than the oldest base system that the AppImage is intended to run on
 3. The AppImage should actually be tested on the base systems that it is intended to run on

### Binaries compiled on old enough base system

The ingredients used in your AppImage should not be built on a more recent base system than the oldest base system your AppImage is intended to run on. Some core libaries, such as glibc, tend to break compatibility with older base systems quite frequently, which means that binaries will run on newer, but not on older base systems than the one the binaries were compiled on.

If you run into errors like this

```
failed to initialize: /lib/tls/i686/cmov/libc.so.6: version `GLIBC_2.11' not found
```

then the binary is compiled on a newer system than the one you are trying to run it on. You should use a binary that has been compiled on an older system. Unfortunately, the complication is that distributions usually compile the latest versions of applications only on the latest systems, which means that you will have a hard time finding binaries of bleeding-edge softwares that run on older systems. A way around this is to compile dependencies yourself on a not too recent base system, and/or to use [LibcWrapGenerator](https://github.com/probonopd/AppImageKit/tree/master/LibcWrapGenerator).

When producing AppImages for the Subsurface project, I have had very good results by using __CentOS 6__. This distribution is not too recent (current major CentOS version minus 1) while there are still the most recent Qt and modern compilers for it in the [EPEL](https://fedoraproject.org/wiki/EPEL) and [devtools-2](http://people.centos.org/tru/devtools-2/) (the community equvalent of the Red Hat Developer Toolset 2) repositories. When using it for compilation, I found the resulting binaries to run on a wide variety of systems, including __debian oldstable__ (wheezy).

### libstdc++.so.6

Some projects require newer C++ standards to build them. To keep the glibc dependency low you can
build a newer GCC version on an older distro and use it to compile the project. This project however will now require a newer version of the `libstdc++.so.6` library than available on that distro. Bundling `libstdc++.so.6` however will in most cases break compatibility with distros that have a newer library version installed into their system than the bundled one. So blindly bundling the library is not reliable. While this is primarily an issue with `libstdc++.so.6` in some rare cases this might also occur with `libgcc_s.so.1`. That's because both libraries are part of GCC. You would have to know the library version of the host system and decide whether to use a bundled library or not before the application is started. This is exactly what the patched AppRun binary from https://github.com/darealshinji/AppImageKit-checkrt/ does. It will search for `usr/optional/libstdc++/libstdc++.so.6` and `usr/optional/libgcc_s/libgcc_s.so.1` inside the AppImage or AppDir. If found it will compare their internal versions with the ones found on the system and prepend their paths to `LD_LIBRARY_PATH` if necessary.

### Testing

To ensure that the AppImage runs on the intended base systems, it should be thoroughly tested on each of them. The following testing procedure is both efficient and effective: Get the previous version of Ubuntu, Fedora, and openSUSE Live CDs and test your AppImage there. Using the three largest distributions increases the chances that your AppImage will run on other distributions as well. Using the previous (current minus one) version ensures that your end users who might not have upgraded to the latest version yet can still run your AppImage. Using Live CDs has the advantage that unlike installed systems, you always have a system that is in a factory-fresh condition that can be easily reproduced. Most developers just test their software on their main working systems, which tend to be heavily customized through the installation of additional packages. By testing on Live CDs, you can be sure that end users will get the best experience possible.

I use ISOs of Live CDs, loop-mount them, chroot into them, and run the AppImage there. This way, I need approximately 700 MB per supported base system (distribution) and can easily upgrade to newer versions by just exchanging one ISO file. The following script automates this for Ubuntu-like (Casper-based) and Fedora-like (Dract-based) Live ISOs:

```
sudo ./AppImageAssistant.AppDir/testappimage /path/to/elementary-0.2-20110926.iso ./AppImageAssistant.AppImage
```

## Common mistake

Please __DO NOT__ put an AppImage into another archive like a `.zip` or `.tar.gz`. While it may be tempting to avoid users having to set permission, this breaks desktop integration with the optional `appimaged` daemon, among other things. Besides, the beauty of the AppImage format is that you never need to unpack anything. Furthermore, packing an AppImage into some form of archive prevents the AppImage from being added to the central catalog of available AppImages at https://github.com/AppImage/AppImageHub.

## Environment variables

By default, AppRun sets some variables such as `LD_LIBRARY_PATH` before executing the payload application. While this is sufficient in most cases, it may lead to issues if the payload application launches other applications that reside in the base system, that is, outside of the AppImage. KDevelop is an example of such an application. In these cases, the [appimage-exec-wrapper](https://cgit.kde.org/scratch/brauch/appimage-exec-wrapper.git/) library can be used together with the AppImage distribution mechanism. Place the library somewhere in your AppImage and point `LD_PRELOAD` to it before launching your application. Whenever your application invokes a child process through `execv()` or `execve()`, this wrapper will intercept the call and see if the child process lies outside of the bundled appdir. If it does, the wrapper will attempt to undo any changes done to environment variables before launching the process, since you probably did not intend to launch it with e.g. the `LD_LIBRARY_PATH` you previously set for your application. [linuxdeployqt](https://github.com/probonopd/linuxdeployqt), on the other hand, does entirely without setting `LD_LIBRARY_PATH` by [setting the RPATH in libraries and executables relative to $ORIGIN](https://nixos.org/patchelf.html).

## Processor architectures

### Can I build armhf or arm64 AppImages?

__Yes__, you can compile the AppImageKit tools for those architectures. The [Open Build Service](https://github.com/AppImage/AppImageKit/wiki/Using-Open-Build-Service) has support for it.

### Can I build multiple-arch (say x86_64 + armhf) AppImages ?

__No__, you need one AppImage for each architecture. [FatELF](https://icculus.org/fatelf/) could solve this, but is not merged into the mainline kernel, so it is currently not an option.

## Distributing AppImages

Even under open source licenses, distributing and/or using code in source or binary form may create certain legal obligations, such as the distribution of the corresponding source code and build instructions for GPL licensed binaries, and displaying copyright statements and disclaimers. As the author of an application which you are distributing as an AppImage, you are responsible to obey all licenses for any third-party dependencies that you include in your AppImage, and ensure that their licenses and source code are made available, where required, together with the release binaries. AppImageKit itself is released under the permissive MIT license.
