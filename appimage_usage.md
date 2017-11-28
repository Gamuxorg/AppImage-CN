## AppImage的用法

运行一个AppImage挂载文件系统映像，并透明地运行包含的应用程序。所以AppImage的使用通常应该等于它包含的应用程序的使用。但是，这里有特殊的功能。如果您收到的AppImgae不支持这些选项，请让AppImage的作者使用最新的`appimagetool`（或`linuxdeployqt`）重新创建它。

### 命令行参数

如果使用这些特殊的命令行参数中的一个调用由最近版本的AppImageKit构建的AppImage，那么AppImage的行为将有所不同：

- `--appimage-help`打印帮助选项
- `--appimage-offset`打印嵌入文件系统映像开始的偏移量，然后退出。如果你想使用`mount -o loop，offset=...`命令来循环挂载文件系统镜像，这很有用
- `--appimage-extract`从嵌入的文件系统映像中提取内容，然后退出。如果您在FUSE不可用的系统上使用AppImage，这非常有用
- `--appimage-mount`挂载嵌入式文件系统映像并打印挂载点，然后等待直到它被杀死。如果您想检查AppImage的内容而不执行包含的有效载荷应用程序，这非常有用
- `--appimage-version`打印AppImageKit的版本，然后退出。如果您想提出问题，这很有用
- `--appimage-updateinformation”`打印嵌入到AppImage中的更新信息，然后退出。这对调试二进制增量更新非常有用
- `--appimage-signature`打印嵌入到AppImage中的数字签名，然后退出。这对调试二进制增量更新非常有用。如果您想验证嵌入式签名，则应使用属于AppImageKit一部分的“验证”命令行工具

### 特殊的目录

通常情况下，包含在AppImage中的应用程序将存储它的配置文件，无论它通常存储在哪里（最常见的是在$ HOME中的某处）。如果您调用由最近版本的AppImageKit构建的AppImage，并且具有这些特殊目录之一，那么配置文件将与AppImage一起存储。这对于便携式使用情况是有用的，例如，在USB记忆棒上携带AppImage以及其数据。

- 如果有一个和AppImage同名的目录加`.home`，那么在执行净荷应用程序之前，会自动将$ HOME设置为它
- 如果有一个和AppImage同名的目录加`.config`，那么在执行payload应用程序之前会自动设置$ XDG_CONFIG_HOME

## AppImage usage

Running an AppImage mounts the filesystem image and transparently runs the contained application. So the usage of an AppImage normally should equal the usage of the application contained in it. However, there is special functionality, as described here. If an AppImgae you have received does not support these options, ask the author of the AppImage to recreate it using the latest `appimagetool` (or `linuxdeployqt`).

### Command line arguments

If you invoke an AppImage built with a recent version of AppImageKit with one of these special command line arguments, then the AppImage will behave differently:

- `--appimage-help` prints the help options
- `--appimage-offset` prints the offset at which the embedded filesystem image starts, and then exits. This is useful in case you would like to loop-mount the filesystem image using the `mount -o loop,offset=...` command 
- `--appimage-extract` extracts the contents from the embedded filesystem image, then exits. This is useful if you are using an AppImage on a system on which FUSE is not available
- `--appimage-mount` mounts the embedded filesystem image and prints the mount point, then waits until it is killed. This is useful if you would like to inspect the contents of an AppImage without executing the contained payload application
- `--appimage-version` prints the version of AppImageKit, then exits. This is useful if you would like to file issues
- `--appimage-updateinformation` prints the update information embedded into the AppImage, then exits. This is useful for debugging binary delta updates
- `--appimage-signature` prints the digital signature embedded into the AppImage, then exits. This is useful for debugging binary delta updates. If you would like to validate the embedded signature, you should use the `validate` command line tool that is part of AppImageKit

### Special directories

Normally the application contained inside an AppImage will store its configuration files whereever it normally stores them (most frequently somewhere inside `$HOME`). If you invoke an AppImage built with a recent version of AppImageKit and have one of these special directories in place, then the configuration files will be stored alongside the AppImage. This can be useful for portable use cases, e.g., carrying an AppImage on a USB stick, along with its data.

- If there is a directory with the same name as the AppImage plus `.home`, then `$HOME` will automatically be set to it before executing the payload application
- If there is a directory with the same name as the AppImage plus `.config`, then `$XDG_CONFIG_HOME` will automatically be set to it before executing the payload application
