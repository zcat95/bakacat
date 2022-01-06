---
title: ArrowOS 12 编译笔记
date: 2022-01-05
slug: android1
categories:
    - 有用的🌌
tags:
    - ArrowOS
    - Android
---

**recovery fix**

修改BoardConfig.mk
```
TARGET_OTA_ASSERT_DEVICE := haydn,haydnin
```

**bluetooth**

使用膏通的蓝牙 vendor
```
android_vendor_qcom_opensource_system_bt

android_vendor_qcom_opensource_bluetooth-commonsys-intf

android_vendor_qcom_opensource_bluetooth_ext

android_vendor_qcom_opensource_packages_apps_Bluetooth
```

**AOD自适应亮度**

引入 Pixel Experience 的设备设置
```
PixelExperience/android_package_resource_devicesettings
```

**Sepolicy**

使用膏通的 Sepolicy ，注意使用正确分支
```
https://github.com/PixelExperience/device_qcom_sepolicy
https://github.com/PixelExperience/device_qcom_sepolicy_vndr
```

目录结构应该为
```
device/
   qcom-LA.UM.10.3.r1-xxxxx-sdm845.0/
     sepolicy/
     sepolicy_vndr/
```