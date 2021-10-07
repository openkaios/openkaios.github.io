---
layout: post
title: 为未获得 OTA 或已被放弃更新的 KaiOS 设备手动导入 Let's Encrypt 新的根证书
date: 2021-10-07 21:29 
tags: 
---

Due to the expiration of Let's Encrypt SSL's old root certificate DST Root CA X3 on September 30, 2021, a large number of KaiOS devices are unable to use the service with this certificate deployed (browsing the website will prompt This Connection is Untrusted)

![](https://liaronce.magecorn.com/img/20211007202249.png)

This tutorial will show you how to manually import a new root certificate so that the affected websites can be accessed properly.

**This tutorial defaults to you having the required environment and having Wallace Toolbox installed.**

<!-- more -->

## Preliminary Preparation

**When viewing the content after this tutorial, please make sure your device supports getting ROOT privileges first, if not, please wait for the OEM to provide an OTA update, this tutorial does not apply to devices that cannot be ROOTed. ** 

1. Use Wallace Toolbox to get temporary ROOT permissions (press 1 on the dialpad), for non-US versions of Nokia 6300 4G and Nokia 8000 4G, see [This tutorial to get permanent ROOT permissions](https://sites.google.com/view/bananahackers/devices/nokia-8000-4g-nokia-6300-4g-2020) (**we do not provide any support for this**)

2. Input `*#*#33284#*#*` on keypad(For Nokia), plug the device into your computer and type `adb devices` in terminal to check if the device is returned.

## Applicate Script

Visit our modified toolkit address: [https://github.com/openGiraffes/b2g-certificates](https://github.com/openGiraffes/b2g-certificates) and download or use `git clone` to get it.

```bash
git clone https://github.com/openGiraffes/b2g-certificates
cd b2g-certificates
```

### Windows (Testing)

For Windows users, we have modified and ported a Batch script and prepared most of the required environment, just download and unzip it, but we have not included ADB in the toolkit, please install the ADB driver and provide your own Android Platform Tools, install the driver and set the environment variables as described in [this tutorial](https://sites.google.com/view/bananahackers/install-omnisd).

Once you have the toolkit, double click `add-certificates-to-phone.bat` to import the certificate to your phone, the phone will reboot after execution and you can try to access websites and applications that use Let's Encrypt SSL.

Since we are only doing a rough port, there may be problems, if you have any, you can give feedback here: [https://github.com/openGiraffes/b2g-certificates/issues](https://github.com/openGiraffes/b2g-certificates/issues)

### Linux

For Linux users, execute the following commands as needed.

```bash
# Debian/Ubuntu
sudo apt-get install libnss3-tools adb wget
# RHEL/CentOS/Fedora
sudo dnf install nss-tools wget #adb needs to be installed by yourself
# ArchLinux
sudo pacman -S nss android-tools wget

chmod +x ./add-certificates-to-phone.sh
./add-certificates-to-phone.sh

# Windows Subsystem for Linux (WSL)
chmod +x ./add-certificates-to-phone-wsl.sh
./add-certificates-to-phone-wsl.sh
```

**Note: For users using WSL, please make sure that Android Platform Tools (Windows version) is added to the environment variable `Path`, as the current version does not allow WSL to access the device**
