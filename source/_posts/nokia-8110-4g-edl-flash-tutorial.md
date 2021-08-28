---
title: 诺基亚 8110 4G EDL 包刷机教程
type: post
date: 2021-08-28 22:08:01
tags: 
---
# 诺基亚 8110 4G EDL 包刷机教程

## 适用范围

可用于诺基亚 8110 4G 的救砖、扩容、恢复原厂系统。

**注意！！！本教程仅提供 `TA-1059` 的 EDL 包， `TA-1048` 在得到该机型的 EDL 包之前不适用。**

## 准备

1. Windows 7 系统（推荐，且关闭了驱动程序签名验证），Windows 10 可使用以下 Batch 脚本尝试（同样需关闭驱动程序签名验证）
2. **USB 2.0 接口（必须）** USB 3.0 实测无法成功
3. QPST Tool，ADB
4. 工程线（可选）

本教程所有资源：

```
天翼云盘：
https://cloud.189.cn/t/UnIbum2aimYf (访问码:ozw4)

如需自行下载其他版本的 QPST Tool 前往：
https://qpsttool.com/
```

## 刷机

### 第一步：前期准备

首先，你需要安装驱动程序，驱动程序安装包名为 `Qualcomm USB Driver V1.0.exe` ，安装完毕后请重启系统，并在系统启动前按下 F8，选择 `禁用驱动程序签名强制` (Windows 7)，若为 Windows 10 请在 `设置 --> 更新和安全 --> 恢复 --> 高级启动 --> 立即重新启动` 后选择 `疑难解答 --> 高级选项 --> 启动设置 --> 重启`，之后按下 `F7 (即禁用驱动程序强制签名)`。

### 第二步：进入 EDL 模式

你有两种方式进入 EDL 模式：

1. 将手机彻底关机，最好抠出电池放电大约 10 秒后放回电池，在关机状态下按住上方向键和下方向键后按住开机键，此时开机的 KaiOS logo 会闪一下后黑屏，此时将 USB 线插入电脑，在设备管理器中的 `其他设备` 显示 `QHSUSB_DLOAD` 或在 `端口 (COM 和 LPT)` 显示 `Qualcomm HS-USB QDLoader 9008 (COMx)`（COMx 不固定，具体以 QFIL 为准）。

2. 在开机状态下，启动 USB 调试（在拨号盘上输入 ` *#*#33284#*#* `），显示甲虫图标后接入电脑，输入 `adb reboot edl`。

3. 购买 microUSB 工程线，在关机状态下插入电脑并按下工程线上的按钮，工程线样式如图。  
![edlcable.jpg](https://i.loli.net/2021/08/28/XbxU74GVyjIqNak.jpg)

### 第三步：安装 QPST Tool

安装 QPST Tool 之前请先安装 `Qualcomm USB Driver V1.0.exe` 此为高通 USB 驱动。安装完毕后可在设备管理器中看到 `Qualcomm HS-USB QDLoader 9008` （手机进入 EDL 模式时）。

驱动安装完毕后即可安装 QPST Tool。

### 第四步：刷机

安装完 QPST Tool 后在开始菜单找到 `QPST --> QFIL` 打开 QFIL 刷机工具，点击 `Select Port` 选择 `Qualcomm HS-USB QDLoader 9008 (COMx)`，在 `Select Build Type` 中选择 `Flat Build`，之后解压刷机包，在 `Select Programmer` 中选择解压后的刷机包内的 `prog_emmc_firehose_8909_ddr.mbn`后，在 `Select Flat Build` 下点击 `Load XML ...` ，会要求你选择两个 XML 文件，先选择刷机包内的 `rawprogram0.xml` 后选择 `patch0.xml`。最后点击 `Download` 即开始刷机。

刷机过程中最好不要动任何设备，`Status` 中返回 `Waiting for reset done....` 且进度条在长时间没有动静后即可拔下数据线，抠掉电池后装回开机。

若进度条很快走完，请一定要翻看 `Status` 是否返回有 `Download Fail!` 等信息，若有请检查驱动和线缆以及接口，一般不推荐在 Windows 10 下进行刷机，因为部分使用 USB2.0 接口的机型在 Windows 10 下有兼容性问题，推荐在 Windows 7 系统下进行刷机，也可尝试云盘内的 `fix-usb3.0.bat` 脚本（需以管理员权限执行）。 

### 第五步：开机

开机之前需先按住上方向键+开机键，进入 Recovery，选择 `Wipe data/factory reset` 按返回键确认，恢复出厂设置，选择 `Wipe cache` 清除 cache 分区。

最后选择 `Reboot system now` 后进入系统。

