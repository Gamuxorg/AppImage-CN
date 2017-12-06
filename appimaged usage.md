## appimaged用法

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

运行`appimaged -v`来增加冗长度。

详细用法：
```
用法：
  appimaged [选项...]

帮助选项：
  -h,--help 显示帮助选项

应用选项：
  -v,--verbose 冗长
  -i,--install 将这个appimaged实例安装到$HOME
  -u,--uninstall 从$HOME中卸载一个appimaged实例
  --version 显示版本号

```

 __注意：__ 可能需要重新启动（或者`xkill`）dash命令行，nautilus，来识别第一次运行`appimaged`之前不存在的新目录。另外，在appimaged运行一次后它应该足以登出会话并重新登录。

如果你的`$PATH`上有`AppImageUpdate`，那么它也可以做到这一点：

![截图从2016-10-15 16-37-05](https://cloud.githubusercontent.com/assets/2480569/19410850/0390fe9c-92f6-11e6-9882-3ca6d360a190.jpg)

从 https://github.com/AppImage/AppImageUpdate/releases/download/continuous/ 上下载AppImageUpdate 并放在你的`$ PATH`上：

```
sudo mv "Downloads/AppImageUpdate-*.AppImage" /usr/local/bin/AppImageUpdate
chmod a+x /usr/local/bin/AppImageUpdate
```

## appimaged usage

`appimaged` is an optional daemon that watches locations like `~/bin` and `~/Downloads` for AppImages and if it detects some, registers them with the system, so that they show up in the menu, have their icons show up, MIME types associated, etc. It also unregisters AppImages again from the system if they are deleted. If [firejail](https://github.com/netblue30/firejail) is installed, it runs the AppImages with it.

A precompiled version can be found in the last successful Travis CI build, you can get it with:

```
wget "https://github.com/AppImage/AppImageKit/releases/download/continuous/appimaged-x86_64.AppImage"
chmod a+x appimaged-x86_64.AppImage
```

Usage in a nutshell:

```
./appimaged-x86_64.AppImage --install
```

Or, if you are on a deb-based system:

```
wget -c "https://github.com/AppImage/AppImageKit/releases/download/continuous/appimaged_1.0_amd64.deb"
sudo dpkg -i appimaged_*.deb
systemctl --user enable appimaged
systemctl --user start appimaged
```

It will register the AppImages in with your system from the following places:
* $HOME/Downloads
* $HOME/.local/bin
* $HOME/bin
* /Applications
* /isodevice/Applications
* /isofrom/Applications
* /run/archiso/img_dev/Applications
* /opt
* /usr/local/bin

Run `appimaged -v` for increased verbosity.

Detailed usage:
```
Usage:
  appimaged [OPTION...] 

Help Options:
  -h, --help          Show help options

Application Options:
  -v, --verbose       Be verbose
  -i, --install       Install this appimaged instance to $HOME
  -u, --uninstall     Uninstall an appimaged instance from $HOME
  --version           Show version number

```

__NOTE:__ It may be necessary to restart (or `xkill`) dash, nautilus, to recognize new directories that didn't exist prior to the first run of `appimaged`. Alternatively, it should be sufficient to log out of the session and log in again after having run appimaged once.

If you have `AppImageUpdate` on your `$PATH`, then it can also do this neat trick:

![screenshot from 2016-10-15 16-37-05](https://cloud.githubusercontent.com/assets/2480569/19410850/0390fe9c-92f6-11e6-9882-3ca6d360a190.jpg)

Download AppImageUpdate from https://github.com/AppImage/AppImageUpdate/releases/download/continuous/ and put on your `$PATH`:

```
sudo mv "Downloads/AppImageUpdate-*.AppImage" /usr/local/bin/AppImageUpdate
chmod a+x /usr/local/bin/AppImageUpdate
```
