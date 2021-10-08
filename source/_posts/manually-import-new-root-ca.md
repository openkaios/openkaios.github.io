---
layout: post
title: 为没有获得 OTA 或已被放弃更新的 KaiOS 设备手动导入 Let's Encrypt 新的根证书
date: 2021-10-07 21:29 
tags: 
---

由于 Let's Encrypt SSL 的旧版根证书 DST Root CA X3 已于 2021 年 9 月 30 日过期，导致大量 KaiOS 设备无法正常使用部署了该证书的服务（浏览网站时会提示 This Connection is Untrusted）

![](https://liaronce.magecorn.com/img/20211007202249.png)

本教程将展示如何手动导入新的根证书令受影响的网站正常访问。

**本教程均默认你拥有所需的环境且已经安装 Wallace Toolbox。**

<!-- more -->

## 前期准备

**在查看本教程之后的内容时，请先确认你的设备是否支持获取 ROOT 权限，若不能获取 ROOT 权限，请等待 OEM 提供 OTA 更新，本教程不适用于无法 ROOT 的设备。**

1. 使用 Wallace Toolbox 获取临时 ROOT 权限（拨号盘按 1），对于非美版的诺基亚 6300 4G 和 8000 4G，请参阅[此教程获取永久 ROOT 权限](https://sites.google.com/view/bananahackers/devices/nokia-8000-4g-nokia-6300-4g-2020)（需自备梯子，**且我们不对此提供任何支持**）

2. 拨号盘输入 `*#*#33284#*#*`，将设备接入到电脑，在终端输入 `adb devices` 检查是否返回设备。

## 应用脚本

访问我们已经修改好的工具包地址：[https://github.com/openGiraffes/b2g-certificates](https://github.com/openGiraffes/b2g-certificates)，下载或使用 `git clone` 获取它。

```bash
git clone https://github.com/openGiraffes/b2g-certificates
cd b2g-certificates
```

### Windows (Testing)

对于 Windows 用户，我们已经修改并移植好了一个 Batch 脚本，且准备好了大部分所需的环境，直接下载解压即可，但我们并未将 ADB 包含到工具包，请自行安装 ADB 驱动且自备 Android Platform Tools，安装驱动以及设置环境变量可参考[此教程](https://opengiraffes.top/install-apps-with-webide-for-kaios2)。

获取到工具包后双击 `add-certificates-to-phone.bat` 然后按照提示输入路径（例如对于诺基亚手机用户，输入 `/data/b2g/mozilla`)，即可将证书导入到手机，执行后手机将会重启，之后可尝试访问使用 Let's Encrypt SSL 的网站和应用。

由于我们仅仅只是粗略的进行移植，可能会出现问题，若有问题可在此处反馈：[https://github.com/openGiraffes/b2g-certificates/issues](https://github.com/openGiraffes/b2g-certificates/issues)

### Linux

对于 Linux 用户，请按需执行以下命令：

```bash
# Debian/Ubuntu
sudo apt-get install libnss3-tools adb wget
# RHEL/CentOS/Fedora
sudo dnf install nss-tools wget #adb 需自行安装
# ArchLinux
sudo pacman -S nss android-tools wget

chmod +x ./add-certificates-to-phone.sh
./add-certificates-to-phone.sh  -d # 对诺基亚用户

# Windows Subsystem for Linux (WSL)
chmod +x ./add-certificates-to-phone-wsl.sh
./add-certificates-to-phone-wsl.sh -d # 对诺基亚用户
```

**注意：对于使用 WSL 的用户，请确保 Android Platform Tools （Windows 版本）已加入到环境变量 `Path` 中，原因是目前的版本无法令 WSL 访问设备**

