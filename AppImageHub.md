# AppImageHub

https://appimage.github.io/

##  这是...

* __可用的，经过审查的AppImages__的众包目录，包含第三方应用商店和软件中心可以使用的数据。给定一个AppImage的URL，它检查AppImage并将其放入社区维护的目录中。这个想法是，所有的元数据都在AppImage中传播，所以除了向这个仓库添加一个URL之外，没有其他的信息会被询问，因为它自带了AppImage。

## 这不是...

* __A分配___。 AppImageHub __不分发AppImages__或提供下载。它只是链接到各自作者的下载页面，从那里用户将能够下载AppImages。它也不跟踪版本___，仅仅是诸如“释放”，“测试”，“每晚”，“连续”（由上游应用程序作者定义）的频道。我们认为，试图跟踪中央存储库中的所有版本是徒劳的，因为它不能扩展。

##用户故事

* “作为一个用户，我想有一个可用的AppImage的中央目录，我可以直接从应用程序作者下载，以便我知道什么是可用的。
* “作为一名开发人员，我希望尽可能多地向我的应用程序提供我的应用程序，以增加我的用户群。”
* “作为应用中心或应用商店开发者，我希望能够轻松获得关于可用AppImages的信息，以便我可以专注于构建我的应用中心或应用商店，而不必自己抓取Internet上的AppImages。

##如何将AppImages提交到目录

使用[此链接]（https://github.com/AppImage/AppImageHub/new/gh-pages/data）创建一个新文件并发送合并请求。该文件应该包含一行代码，该代码链接到在其“发行版”页面上承载AppImages的GitHub存储库。另外，一个链接到AppImage。没有其他的。然后发送一个合并请求到这个仓库。 Travis CI将立即执行AppImage的自动检查，如果成功，您将在您的请求中看到__green__结果。如果您得到__red__结果，请检查Travis CI构建的日志，并修复它。

###提交您自己的AppImage的清单

作为一种格式，AppImage的设计方式不会对生成AppImages的人施加限制。基本上，你可以随心所欲地放入一个AppImage。但是，对于AppImageHub，还需要附加规则。提交给AppImage中心的AppImages会自动进行手动审查。

* 必须从URL下载。我们的测试sysgtem使用`wget`获取AppImage。目前，我们无法从身份验证和/或Cookie保护的位置获取AppImages。对于商业应用，我们建议有一个通常可下载的演示/试用版本。如果您想将您的商业AppImage添加到目录中，请联系我们，并且无法进行常规下载
* 必须运行在最旧版本的Ubuntu LTS版本上（https://www.ubuntu.com/info/release-end-of-life）（目前是Ubuntu 14.04），而不需要安装额外的软件包。针对最老的仍然支持的LTS是为了确保AppImage不仅可以在最新版本上运行，而且还可以在较旧的目标系统上运行，比如企业版（不限于Ubuntu）
* 必须在我们的基于Travis CI的测试环境中执行
* 必须通过[appdir-lint.sh]（https://github.com/AppImage/AppImages/blob/master/appdir-lint.sh）
* 必须有一个通过`desktop-file-validate`的桌面文件
* 必须运行没有活动的互联网连接（至少显示一些信息）
* 在`usr / share / metainfo`中应该有一个[AppStream元信息文件]（https://people.freedesktop.org/~hughsient/appdata/）。如果是这样，必须通过`appstreamcli`验证
* 应该显示一个有用的屏幕，而不是一些粗略的对话框，因为主窗口将用于主屏幕截图。请注意，您可以使用[AppStream元信息文件]（https://people.freedesktop.org/~hughsient/appdata/）提供自己的屏幕截图
* 应该在不包含版本号的常量URL下可用。另外，应该可以在GitHub发布或者openSUSE构建服务上使用（你可以自由地推荐其他的服务）

## 如何使用

应用程序商店和软件中心可以使用此项目收集的元数据。请参阅[AppImage生态系统]（https://github.com/AppImage/AppImageKit/wiki/Ecosystem）。

目前我们正在https://appimage.github.io/feed.json上提供一个JSON提要。如果您想使用这些数据，但需要更改，请通过irc.freenode.net上的#AppImage与我们联系，以便我们可以讨论最适合您需求的输出格式。 __PLEASE NOTE__数据输出格式尚未最终确定，如有更改，恕不另行通知，直到我们发布

# AppImageHub

https://appimage.github.io/

## This is...

* __A crowd-sourced directory of available, reviewed AppImages__ with data that 3rd party app stores and software centers can use. Given an URL to an AppImage, it inspects the AppImage and puts it into a community-maintained catalog. The idea is that all the metadata travels inside the AppImage, so besides adding an URL to this repository no additional information is asked, since it comes with the AppImage itself.

## This is NOT...

* __A distribution__. AppImageHub __does not distribute AppImages__ or provide them for download. It simply links to the respective author's download pages, from where users will be able to download AppImages. It also __does not keep track of versions__, only of channels such as "release", "beta", "nightly", "continuous" (as defined by the upstream application authors). We think that trying to keep track of all versions in a central repository is futile, since it does not scale.

## User stories

* "As a user, I would like to have a central catalog of available AppImages that I can download directly from the application authors, so that I know what is available."
* "As a developer, I would like to make my application known to as many potential users as possible in order to increase my user base."
* "As an app center or app store developer, I would like to get information about the available AppImages easily, so that I can focus on building my app center or app store rather than having to crawl the Internet for AppImages myself."

## How to submit AppImages to the catalog

Create a new file using [this link](https://github.com/AppImage/AppImageHub/new/gh-pages/data) and send a Pull Request. The file should contain one line with a link to the GitHub repository that hosts AppImages on its Releases page. Alternatively, a link to the AppImage. Nothing else. Then send a Pull Request to this repository. Travis CI will instantly perform an automated review of the AppImage, and in case it succeeds, you will see a __green__ result in your pull request. If you get a __red__ result, check the log of the Travis CI build, and fix it.

### Checklist for submitting your own AppImage

As a format, AppImage is designed in a way that does not impose restrictions on the person generating AppImages. Basically you are free to put inside an AppImage whatever you want. For AppImageHub, however, additional rules apply. AppImages submitted to AppImage hub undergo automatic and possibly additional manual review.

* Must be downloadable from an URL. Our testing sysgtem fetches the AppImage using `wget`. Currently we cannot get AppImages from locations behind authentication and/or cookie-protected locations. For commercial applications we recommend to have a generally downloadable demo/trial version. Please contact us if you would like to add your commercial AppImage to the directory and it is not available for general download
* Must run on the [oldest still-supported Ubuntu LTS release](https://www.ubuntu.com/info/release-end-of-life) (currently Ubuntu 14.04) without the installation of additional packages. Targeting the oldest still-supported LTS is to ensure that the AppImage will run not only on the very latest, but also on older target systems, such as enterprise distributions (not limited to Ubuntu)
* Must execute in our Travis CI based testing environment
* Must pass [appdir-lint.sh](https://github.com/AppImage/AppImages/blob/master/appdir-lint.sh)
* Must have a desktop file that passes `desktop-file-validate`
* Must run without active Internet connection (and at least show some information)
* Should have an [AppStream metainfo file](https://people.freedesktop.org/~hughsient/appdata/) in `usr/share/metainfo`. If it does, must pass `appstreamcli` validation
* Should show a useful screen rather than some crude dialog box since the main window will be used for the main screenshots. Note that you can provide your own screenshots by using an [AppStream metainfo file](https://people.freedesktop.org/~hughsient/appdata/)
* Should be available under a constant URL that does not contain the version number. Alternatively, should be available on GitHub Releases or the openSUSE Build Service (you are free to suggest additional serices like these)

## How to use

App stores and software centers can consume the metadata collected by this project. See [AppImage ecosystem](https://github.com/AppImage/AppImageKit/wiki/Ecosystem). 

Currently we are providing a JSON feed at https://appimage.github.io/feed.json. If you would like to use this data but need changes, please contact us on #AppImage at irc.freenode.net, so that we can discuss an output format that would serve your needs best. __PLEASE NOTE__ that the data output format is not finalized yet and is subject to change any time without prior notice, until we release 