## 如果您已经有deb或ppa，请使用pkg2appimage工具

有[多种方法](https://github.com/probonopd/AppImageKit/wiki/Creating-AppImages)生成AppImages。如果你已经有了二进制文件（无论是归档文件还是`.deb`格式亦或ppa），那么将这些二进制文件转换为AppImage的推荐方法是编写一个[.yml描述文件](https://github.com/AppImage/AppImages/tree/master/recipes)并使用[pkg2appimage](https://github.com/AppImage/AppImages/tree/master/pkg2appimage)运行它：

从`.yml`描述文件构建一个AppImage：

```
bash -ex ./pkg2appimage recipes/XXX.yml
```

`.yml`描述文件可以告诉pkg2appimage从哪里获取应用的各个组成部分，以及如何将它们转换为AppImage（除了已经包含在pkg2appimage中的统一步骤）。你可以学习一些[示例](https://github.com/AppImage/AppImages/tree/master/recipes)，观察它是如何工作的。

 代码库[AppImageKit](https://github.com/probonopd/appimagekit)包含工具`pkg2appimage`和一些生成__AppImages__（即便携式Linux应用程序）的“配方”(recipes)。你也可以前往[Bintray Page](https://bintray.com/probono/AppImages)下载已经生成好的AppImages。

__配方(recipes)__是指用于创建AppImages的`.yml`文件。

## Use pkg2appimage tool  if you already have a deb or ppa

There are [multiple ways](https://github.com/probonopd/AppImageKit/wiki/Creating-AppImages) to generate AppImages. If you already have existing binaries (either in archive or `.deb` format or a ppa) then the recommended way to convert these to an AppImage is to write a [.yml description file](https://github.com/AppImage/AppImages/tree/master/recipes) and run it with [pkg2appimage](https://github.com/AppImage/AppImages/tree/master/pkg2appimage):

To build an AppImage from a `.yml` description file:

```
bash -ex ./pkg2appimage recipes/XXX.yml
```

`.yml` description files tell pkg2appimage where to get the ingredients from, and how to convert them to an AppImage (besides the general steps already included in pkg2appimage). Study some [examples](https://github.com/AppImage/AppImages/tree/master/recipes) to see how it works.

This repository contains the `pkg2appimage` tool and some recipes to generate __AppImages__ (portable Linux apps) using [AppImageKit](https://github.com/probonopd/appimagekit). See the [Bintray page](https://bintray.com/probono/AppImages) tab for downloads of the generated AppImages.

__Recipes__ are the `.yml` files used to create the AppImages.

