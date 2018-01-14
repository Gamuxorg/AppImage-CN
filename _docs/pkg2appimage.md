---
title: 从deb包或ppa构建AppImage
permalink: /docs/pkg2appimage/
---

## 从deb包或ppa构建AppImage

有 [多种方式](https://github.com/probonopd/AppImageKit/wiki/Creating-AppImages) 生成AppImage。如果你已经有了二进制文件（无论是程序压缩包还是 `.deb` 格式亦或 ppa），那么将这些二进制文件转换为 AppImage 的推荐方法是编写一个 [.yml描述文件](https://github.com/AppImage/AppImages/tree/master/recipes) 并使用 [pkg2appimage](https://github.com/AppImage/AppImages/tree/master/pkg2appimage) 运行它：

从 `.yml` 描述文件构建一个 AppImage：

```
bash -ex ./pkg2appimage recipes/XXX.yml
```

`.yml` 描述文件可以告诉 pkg2appimage 从哪里获取应用的各个组成部分，以及如何将它们转换为 AppImage（除了已经包含在 pkg2appimage 中的统一步骤）。你可以学习一些 [示例](https://github.com/AppImage/AppImages/tree/master/recipes)，观察它是如何工作的。

 代码库 [AppImageKit](https://github.com/probonopd/appimagekit) 包含工具 `pkg2appimage` 和一些生成 __AppImage__（即便携式Linux应用程序）的“配方”(recipes)。你也可以前往 [Bintray Page](https://bintray.com/probono/AppImages) 下载已经生成好的AppImage。

__配方(recipes)__ 是指用于创建AppImage的 `.yml` 文件。
