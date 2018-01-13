## appimagetool的使用方法

`appimagetool`用于把现有的`AppDir`(目录)生成一个AppImage。一些更高级别的工具，如[`linuxdeployqt`](https://github.com/probonopd/linuxdeployqt)等也在使用它。预编译版本可以在[GitHub Releases](https://github.com/AppImage/AppImageKit/releases)上找到。

```
wget "https://github.com/AppImage/AppImageKit/releases/download/continuous/appimagetool-x86_64.AppImage"
chmod a+x appimagetool-x86_64.AppImage
```

简而言之，假设你已经有一个[AppDir](https://github.com/AppImage/AppImageSpec/blob/master/draft.md#appdir) ：

```
./appimagetool-x86_64.AppImage some.AppDir
```

详细用法：
```
用法：
  appimagetool [OPTION ...] SOURCE [DESTINATION] - 生成，提取和检查AppImages

帮助选项：
  -h，--help                 显示帮助选项

应用选项：
  -l, --list                 列出SOURCE AppImage中的文件
  -u, --updateinformation    嵌入更新信息STRING; 如果安装了 zsyncmake，则生成 zsync 文件
  --bintray-user             Bintray用户名
  --bintray-repo             Bintray存储库
  --version                  显示版本号
  -v, --verbose              产生详细的输出
  -s, --sign                 用 gpg2 签名
  -n, --no-appstream         不检查AppStream元数据
```

如果你想手动生成一个AppImage，你可以：

```
mksquashfs Your.AppDir Your.squashfs -root-owned -noappend
cat runtime >> Your.AppImage
cat Your.squashfs >> Your.AppImage
chmod a+x Your.AppImage
```

# appimagetool usage

`appimagetool` is used to generate an AppImage from an existing `AppDir`. Higher-level tools such as [`linuxdeployqt`](https://github.com/probonopd/linuxdeployqt) use it internally. A precompiled version can be found on [GitHub Releases](https://github.com/AppImage/AppImageKit/releases).

```
wget "https://github.com/AppImage/AppImageKit/releases/download/continuous/appimagetool-x86_64.AppImage"
chmod a+x appimagetool-x86_64.AppImage
```

Usage in a nutshell, assuming that you already have an [AppDir](https://github.com/AppImage/AppImageSpec/blob/master/draft.md#appdir) in place:

```
./appimagetool-x86_64.AppImage some.AppDir
```

Detailed usage:
```
Usage:
  appimagetool [OPTION...] SOURCE [DESTINATION] - Generate, extract, and inspect AppImages

Help Options:
  -h, --help                  Show help options

Application Options:
  -l, --list                  List files in SOURCE AppImage
  -u, --updateinformation     Embed update information STRING; if zsyncmake is installed, generate zsync file
  --bintray-user              Bintray user name
  --bintray-repo              Bintray repository
  --version                   Show version number
  -v, --verbose               Produce verbose output
  -s, --sign                  Sign with gpg2
  -n, --no-appstream          Do not check AppStream metadata
```

If you want to generate an AppImage manually, you can:

```
mksquashfs Your.AppDir Your.squashfs -root-owned -noappend
cat runtime >> Your.AppImage
cat Your.squashfs >> Your.AppImage
chmod a+x Your.AppImage
```