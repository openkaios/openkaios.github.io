---
layout: post
title: 使用 WebIDE 安装以及调试第三方应用程序 (KaiOS 2.5.x)
date: 2021-09-30 21:48
tags: 
---

本教程将展示使用 Waterfox Classic 安装为 KaiOS 2.5.x 设计的应用，同时我们也非常推荐使用它来进行安装和调试应用。

考虑到可能有一些用户是需要使用它安装第三方应用，且并无开发知识，我们也会尽量将该教程变得通俗易懂，同时尽力简化操作步骤，并使用图片。

**本教程本身不会对手机产生任何损害，但请注意，你必须信任你所安装的应用是安全、无任何威胁的，WebIDE不会验证应用的安全性，openGiraffes Group 不对因使用 WebIDE 安装恶意应用程序所产生的问题承担任何责任。**

<!-- more -->

## 前期准备

 - Waterfox Classic 浏览器
 - adb 驱动，macOS 和 Linux 可忽略 （可从此处下载：https://cloud.189.cn/t/jYNNBrnANvui (访问码:tpo4)，文件名: `ADBDriverInstaller.exe`）
 - Android SDK Platform Tools [最新版本下载地址](https://developer.android.google.cn/studio/releases/platform-tools)，macOS 和 Linux 发行版可通过其对应的包管理器（如 `Homebrew`(macOS)、`Pacman`(Arch Linux)、`apt`(Debian/Ubuntu)、`Yum/Dnf`(RedHat)）安装，包名为 `android-platform-tools`，Linux 发行版需自行寻找。
 - 一部运行 KaiOS 2.5.x 或 Firefox OS 2.x 的手机以及一根 microUSB 数据线

**注意：本教程以下步骤均假设你所使用的系统为 Windows 10。且手机为 HMD 公司所发布的诺基亚 KaiOS 2.5.x 设备：8110 4G (2.5.1)/2720 Flip (2.5.2.1)/800 Tough (2.5.2.1)/6300 4G (2.5.4.1)/8000 4G (2.5.4.1)**

## 准备开发环境

连上手机，在手机拨号盘输入 `*#*#33284#*#*` 后，打开 `ADBDriverInstaller.exe` 发现设备后点击 `Install` 即可安装驱动。

从 [Waterfox 官网](https://www.waterfox.net/download/) 找到 Waterfox Classic，选择你所运行的平台下载安装，截至目前的版本（2021.09）该浏览器的 WebIDE 均保留且可以正常使用，与 Firefox 52.9/59.0 和 Palemoon 28.6.1 相比取消了 WebIDE 难以使用的代码编辑器，同时 ADB Helper 可以正常使用，你可以选择你所喜欢的代码编辑器甚至专业 IDE 来编写应用（如 Visual Studio Code、WebStorm、HBuilder X 等）。

![](https://liaronce.magecorn.com/img/Snipaste_2021-09-30_22-03-04.png)

为了让 Waterfox Classic 的 ADB Helper 可以正常使用，同时为了以后方便通过 cmd 使用，我们推荐将 Android SDK Platform Tools 解压到任意的目录（路径最好不要有中文），并编辑环境变量（在 Windows 任务栏上的搜索中或在控制面板搜索 `环境变量` 可以找到），将 `platform-tools` 内的 adb 所在的路径添加到 `Path` 下（用户变量下的可重启 cmd 立即生效，系统变量下的需重启系统后生效）。

![](https://liaronce.magecorn.com/img/Snipaste_2021-09-30_23-15-10.png)

设置成功后可在 cmd 中确认 adb 是否可被调用，输入 `adb version`，输出以下内容说明环境变量配置成功：

![](https://liaronce.magecorn.com/img/Snipaste_2021-09-30_23-35-40.png)

## 准备 WebIDE

启动 Waterfox Classic，首次启动可能默认语言为英语，可在右上角的汉堡菜单中点击 `Options`，在 `Locale Select` 下找到 `Chinese Simplified - 中文 (简体)` 并按照提示重启浏览器后即可生效。

![](https://liaronce.magecorn.com/img/Snipaste_2021-09-30_23-48-43.png)

同样是在汉堡菜单，点击 `开发者`，找到 WebIDE，点击后即可进入 WebIDE 界面，你也可以通过快捷键 `Shift + F8` 快速进入：

![](https://liaronce.magecorn.com/img/Snipaste_2021-09-30_23-49-51.png)

在 WebIDE 窗口下的菜单栏里找到 `项目 --> 管理额外组件`，安装 `ADB Helper 附加组件` 和 `工具适配器扩展`，这两个扩展有可能在启动 WebIDE 后就已经自动安装完毕，需检查。

![](https://liaronce.magecorn.com/img/Snipaste_2021-10-01_00-00-37.png)

![](https://liaronce.magecorn.com/img/Snipaste_2021-10-01_00-01-02.png)

## 准备手机

确认你的手机进入了 Debug 模式（即手机状态栏有一个背后为 `T` 的甲虫图标），若无可在拨号盘输入 `*#*#33284#*#*` 后进入，为保证可以正常连接，请**彻底关闭**任何手机助手以及任何安卓模拟器软件（因为这些软件不同版本的 ADB 会与你所配置的版本冲突）：

![](https://liaronce.magecorn.com/img/2021-09-30-23-51-53.png)

确认手机已经接入电脑，现在你可以在右侧面板的 `USB 设备` 中找到你的手机，点击后即可连接，注意此时有可能会断连，再次点击即可：

![](https://liaronce.magecorn.com/img/Snipaste_2021-10-01_00-02-49.png)

连接成功后会在右上角此处显示一个蓝色的手机图标和设备名称：

![](https://liaronce.magecorn.com/img/Snipaste_2021-10-01_00-04-34.png)

### 如果无法通过 USB 设备直接连接

如果因为系统配置等我们无法预料的原因导致无法在 WebIDE 中通过 `USB 设备` 直接连接，你可以使用 KaiOS Technologies 公司建议的方式：

在 cmd 中，输入以下命令：

```bash
adb forward tcp:6000 localfilesystem:/data/local/debugger-socket
```

此时该命令会返回 TCP 端口 (`6000`)。

此时在右侧面板的 `其他` 选择 `远程运行环境`，在弹出的窗口输入 `localhost:6000`，点击确定后即可连接成功，效果等同于直接连接，区别是右上角则会显示 `远程运行环境`。

![](https://liaronce.magecorn.com/img/Snipaste_2021-10-01_00-42-14.png)

## 安装应用

对于在网络上找到的应用程序包（zip 格式的压缩包），请将其解压，若为商店包或为 OmniSD 包（压缩包内有 `application.zip` 和 `metadata.json`），请进一步解压 `application.zip` 直到可以看到 `manifest.webapp` 文件，这是该应用程序的清单文件。

![](https://liaronce.magecorn.com/img/Snipaste_2021-10-01_00-17-24.png)

在左侧面板点击 `打开打包式应用...` 双击含有 `manifest.webapp` 文件的目录，点击 `选择文件夹` 后即可显示应用程序的基本信息。

（若上面的运行按钮（即等边三角形图标）可以正常点击，可无视警告，不影响后续安装）：

![](https://liaronce.magecorn.com/img/Snipaste_2021-10-01_00-17-45.png)

![](https://liaronce.magecorn.com/img/Snipaste_2021-10-01_00-17-52.png)

点击运行按钮，稍等片刻后会直接在手机上安装并运行该应用程序：

![](https://liaronce.magecorn.com/img/Snipaste_2021-10-01_00-23-10.png)

普通用户在此可以止步，因为此时应用已经安装完毕，并有 `下载完成` 的推送通知：

![](https://liaronce.magecorn.com/img/2021-10-01-00-22-53.png)

**注意：通过 WebIDE 安装的应用程序均不支持自动更新，若有更新请通过以上步骤手动通过 WebIDE 更新**

## 进阶：调试应用

若为开发者，以下三个按钮会对调试有所帮助：

![](https://liaronce.magecorn.com/img/Snipaste_2021-10-01_00-30-44.png)

从左至右分别为：刷新应用（安装并运行），停止应用，调试应用

调试应用即在 WebIDE 底部弹出 Firefox 的开发者工具，你可以在这里对应用进行调试：

![](https://liaronce.magecorn.com/img/Snipaste_2021-10-01_00-29-24.png)

**注意：存储选项卡不可用，原因未知，点击后会导致应用崩溃停止，且之后会因为开发者工具会记忆上一次的选项卡，导致无法进行调试**