---
title: appimaged用法
permalink: /docs/appimaged-usage/
---

### appimaged用法

`appimaged`是一个可选的守护进程，用于监视AppImages的像`~/bin`和`~/Downloads`的位置，如果检测到某些守护进程，则将其注册到系统中，以便它们显示在菜单中，将它们的图标显示出来，关联MIME类型等等。如果它们被删除，它也从系统中再次注销AppImages。如果安装了[firejail](https://github.com/netblue30/firejail)，则会一同运行AppImage。

预编译版本可以在上次成功的Travis CI版本中找到，您可以通过以下方式获得：

```
wget "https://github.com/AppImage/AppImageKit/releases/download/continuous/appimaged-x86_64.AppImage"
chmod a+x appimaged-x86_64.AppImage
```

用法简而言之：

```
./appimaged-x86_64.AppImage --install
```

或者，如果您使用的是基于deb包管理的系统：

```
wget -c "https://github.com/AppImage/AppImageKit/releases/download/continuous/appimaged_1.0_amd64.deb"
sudo dpkg -i appimaged_*.deb
systemctl --user enable appimaged
systemctl --user start appimaged
```

它将从以下位置在您的系统中注册AppImages：
* $HOME/Downloads
* $HOME/.local/bin
* $HOME/bin
* /Applications
* /isodevice/Applications
* /isofrom/Applications
* /run/archiso/img_dev/Applications
* /opt
* /usr/local/bin

运行`appimaged -v`来输出详细信息。

详细用法：
```
用法：
  appimaged [选项...]

帮助选项：
  -h,--help 显示帮助选项

应用选项：
  -v,--verbose 输出详细信息
  -i,--install 将这个appimaged实例安装到$HOME
  -u,--uninstall 从$HOME中卸载一个appimaged实例
  --version 显示版本号

```

 __注意：__ 可能需要重新启动（或者`xkill`）dash命令行，nautilus，来识别第一次运行`appimaged`之前不存在的新目录。另外，在appimaged运行一次后应该注销并重新登录。

如果你的`$PATH`上有`AppImageUpdate`，那么它也可以做到这一点：

![截图从2016-10-15 16-37-05](https://cloud.githubusercontent.com/assets/2480569/19410850/0390fe9c-92f6-11e6-9882-3ca6d360a190.jpg)

从 https://github.com/AppImage/AppImageUpdate/releases/download/continuous/ 上下载AppImageUpdate 并放在你的`$ PATH`上：

```
sudo mv "Downloads/AppImageUpdate-*.AppImage" /usr/local/bin/AppImageUpdate
chmod a+x /usr/local/bin/AppImageUpdate
```
