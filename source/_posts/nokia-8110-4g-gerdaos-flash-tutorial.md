---
title: 诺基亚 8110 4G GerdaOS 刷机教程
type: post
date: 2021-09-27 09:08:01
tags: 
---

之前的教程由于年代久远，一些地方有进一步优化的空间，所以重新翻新。

## 导读

[GerdaOS](https://gerda.tech) 是一个基于诺基亚 8110 4G V13版本制作的第三方 KaiOS ROM，这个ROM支持截屏、文件管理、安装第三方软件、开发者选项、IMEI修改、TTL修改、LTE频段解锁等，并且加入了广告屏蔽等功能。

他们说：“Install what you want, not they.”意味着你可以最大限度地定制你的香蕉机，而不是受HMD，kaiOS甚至阿三的jio摆布。

目前这个系统处在alpha阶段，可能会有未知的风险。刷机有风险，操作需谨慎。请务必备份好您的所有数据。这会删除您的所有数据。这一操作将导致您失去来自 HMD 官方的 OTA 更新。openGiraffes Group 不对本内容产生的问题承担任何责任。

<!-- more -->

## 已知问题和注意事项

1. 刷入 GerdaOS 后，将无法连接 WebIDE，同时也将无法通过 WebIDE 安装软件，你只能使用 OmniSD/GerdaPkg 安装软件。
2. 刷入 GerdaOS 后，将无法使用 KaiStore（该第三方 ROM 已将 KaiStore 从固件中移除）

## 系统要求

 - 一部正品 Nokia 8110 4G 香蕉机。华强北勿扰。
 - 一台电脑。最好运行的是 Mac 或者 Linux 系统，Windows 也可。
 - 手机固件需要先升级到 V12 以上。
 - 一张 TF 卡。可用空间需至少 5GB。

以下教程均假设你所使用的系统为 Windows 10。

## 下载所需文件：

 - **adb 驱动**
 - **Android SDK Platform Tools** [最新版本下载地址](https://developer.android.google.cn/studio/releases/platform-tools)
 - **recovery_8110.img** Recovery镜像文件，重命名为recovery.img并存放在存储卡根目录下。
 - **gerda-install-221edb8.zip** GerdaOS刷机包。存放在存储卡根目录下。
 - **OneKeyOmniSD** OmniSD一键安装器。
 - **Wallace Toolbox** 华莱士工具箱
 - （可选，备用） Waterfox Classic（或同样使用老版本内核的基于 Firefox 的浏览器，如 Firefox 52.9 ESR，Palemoon 28.6.1），我们非常推荐使用该浏览器安装应用。

除部分可以被直接访问的地址外，所有文件已经以天翼网盘分享链接的形式提供：

```
https://cloud.189.cn/t/jYNNBrnANvui (访问码:tpo4)
```

## 安装需要的环境（老手可以跳过）

连上手机，打开 `ADBDriverInstaller.exe` 发现设备后点击 `Install` 即可安装驱动。

将 Android SDK Platform Tools 解压到任意的目录（路径最好不要有中文），并编辑环境变量（在 Windows 任务栏上的搜索中或在控制面板搜索 `环境变量` 可以找到），将 `platform-tools` 内的 adb 所在的路径添加到 `Path` 下（用户变量下的可重启 cmd 立即生效，系统变量下的需重启系统后生效）

## 重置提权

OneKeyOmniSD支持通过USB一键安装OmniSD程序，安装步骤大大简化。

**在进行下一步操作之前，请确认此操作会删除您手机上的所有数据，请务必提前做好备份。openGiraffes Group 不为此工具的不当使用造成的任何后果承担责任。**

安装方法：

 1. 下载该工具
 2. 电脑安装adb驱动
 3. 将您的 KaiOS 设备（如8110）通过 USB 连接电脑
 4. 在您的 KaiOS 设备桌面上通过拨号盘输入以下代码：`*#*#33284#*#*`
 5. 如出现小甲虫图标，即开启成功
 6. 运行本工具
 7. 如果提示部署成功，请在您的KaiOS设备的应用列表找到OmniSD
 8. 打开OmniSD，按下#号键并确认重置提权，**此操作会删除您设备上的所有数据，请做好备份**
 9. 重启进入系统，重复3-7步骤
 10. 安装完成。

## 临时root

 - 将 Wallace Toolbox 的安装包复制粘贴到 TF 卡的 `download` 文件夹，若无可手动创建。
 - 打开刚刚安装的 OmniSD，选择 Wallace Toolbox 安装包安装。
 - 安装完成后，启动 Wallace Toolbox。
 - 按1进行临时root，完成后关闭 Wallace Toolbox。

## 降级Recovery

此步骤执行之前，请确保recovery镜像已经存放在存储卡根目录下并重命名为recovery.img。

在终端/命令提示符中逐行复制：

```shell
adb shell

busybox dd if=/dev/block/bootdevice/by-name/recovery of=/sdcard/recovery-backup.img bs=2048
busybox dd if=/sdcard/recovery.img of=/dev/block/bootdevice/by-name/recovery

mount -o remount,rw /system
echo '#!/system/bin/sh' > /system/bin/install-recovery.sh
echo 'exit 0' >> /system/bin/install-recovery.sh
chown root:root /system/bin/install-recovery.sh
chmod 750 /system/bin/install-recovery.sh
sync
mount -o remount,ro /system
exit

adb reboot
```

此时手机重启后正常进入系统，recovery 降级完成。您可以重启至 recovery（命令 `adb reboot recovery` 或者关机状态下同时按住 电源键 + 上键），如果首行显示 “Gerda Recovery” 即为安装成功。

## 刷入刷机包

上下方向键选择，电源键或返回键确定。

 - 重启进入 recovery
 - 选择 Mount /system
 - 选择 Apply update from SD card
 - 选择 gerda-install-702d409.zip
 - 稍等一会，等待 recovery 菜单重新弹出
 - 选择 Wipe data/factory reset 双清
 - 选择 Wipe cache 清除缓存
 - 选 Yes
 - 选择 reboot system now 重启至系统。

至此，如果开机界面变为 Tux (Linux 企鹅) 和 GerdaOS 图标，即为刷机成功。您可以体验您的新系统了。您可以通过自带的文件管理器安装第三方软件了。
