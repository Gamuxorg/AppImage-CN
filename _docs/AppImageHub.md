---
title: AppImageHub
permalink: /docs/appimagehub/
---

# AppImageHub

https://appimage.github.io/

## 这是...

* __可用的，经过审查的 AppImages 的众包目录__ ，包含第三方应用商店和软件中心可以使用的数据。给定一个 AppImage 的 URL，它检查 AppImage 并将其放入社区维护的目录中。这个想法是，所有的元数据都在 AppImage 中传播，所以只要向这个仓库添加一个 URL 就行了，其它的都包含在 AppImage 自身。

## 这不是...

* __appimage分发渠道__ 。 AppImageHub __不分发AppImages__或提供下载。它只是链接到各自作者的下载页面，从那里用户将能够下载AppImages。它也__不跟踪版本__，仅仅是诸如“release”、“beta”，“nightly”，“continuous”（由上游应用程序作者定义）等各种 channels。我们认为，试图跟踪中央存储库中的所有版本是徒劳的，因为它不能扩展。

## 用户故事

* “作为一个用户，我想有一个可用的AppImage的中央目录，我可以直接从应用程序作者那里下载，以便我知道什么是可用的。”
* “作为一名开发人员，我希望尽可能多地向我的用户提供我的应用程序，以便增加我的用户群。”
* “作为应用中心或应用商店开发者，我希望能够轻松获得关于可用 AppImages 的信息，以便我可以专注于构建我的应用中心或应用商店，而不必自己抓取 Internet 上的 AppImages。

## 如何将 AppImages 提交到目录

通过 [此链接](https://github.com/AppImage/AppImageHub/new/gh-pages/data) 创建一个新文件并发送合并请求。该文件应该包含一个超链接指向其 Github 仓库存放 AppImage 的 Release 页面，或者一个指向 AppImage 的链接，就这么简单就行了。然后发送一个合并请求到这个仓库。 Travis CI 将立即执行 AppImage 的自动检查，如果成功，您将在您的请求中看到 __绿色的__ 结果。如果您得到 __红色的__ 结果，请检查Travis CI构建的日志，并修复它。

### 提交您自己的 AppImage 的清单

作为一种格式，AppImage 并不会对生成 AppImages 的人施加限制。基本上，你想往 AppImage 里面放什么都可以。但是，对于 AppImageHub，还是需要附加规则的。提交给 AppImage 中心的 AppImages 会自动进行也可能手动审查。

* 必须能通过 URL 下载。我们的测试系统使用 `wget` 获取 AppImage。目前，我们无法从一个有身份验证或Cookie保护的位置获取 AppImages。对于商业应用，我们建议有一个通用可下载的演示/试用版本。如果您想将您的商业AppImage添加到目录中，而这个软件并且无法进行常规下载，请联系我们
* 必须能运行在 [仍受支持的最旧 Ubuntu LTS 版本](https://www.ubuntu.com/info/release-end-of-life) （目前是Ubuntu 14.04）上，而不需要安装额外的软件包。这是为了确保AppImage不仅可以在最新版本上运行，而且还可以在较旧的目标系统上运行，比如企业版（不限于Ubuntu）
* 必须能在我们的基于 Travis CI 的测试环境中执行
* 必须通过 [appdir-lint.sh](https://github.com/AppImage/AppImages/blob/master/appdir-lint.sh)
* 必须有一个通过 `desktop-file-validate` 的 desktop 文件
* 必须可以运行在无有效的互联网连接的环境下（还有至少要显示一些信息）
* 在`usr/share/metainfo`中应该有一个 [AppStream元信息文件](https://people.freedesktop.org/~hughsient/appdata/)。而且必须通过 `appstreamcli` 验证
* 应该显示一个有用的界面，而不是一些粗略的对话框，因为主窗口将用于主屏幕截图。您还可以使用 [AppStream元信息文件](https://people.freedesktop.org/~hughsient/appdata/) 提供自己的屏幕截图
* 不包含版本号的固定 URL 也必须可用。GitHub Release 或者 `openSUSE Build Service` 是满足要求的（你可以自由地选择推荐以外的其他服务）

## 如何使用

应用程序商店和软件中心可以使用此项目收集的元数据。请参阅 [AppImage生态系统](https://github.com/AppImage/AppImageKit/wiki/Ecosystem)。

目前我们正在 https://appimage.github.io/feed.json 上提供一个JSON提要。如果您想使用这些数据则需要更改，请通过i rc.freenode.net上 的 #AppImage 与我们联系，以便我们可以讨论最适合您需求的输出格式。 __请注意__，Release之前，数据输出格式尚未最终确定，如有更改，恕不另行通知。
