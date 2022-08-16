---
title: Android 编译笔记 1
date: 2022-01-05
slug: android1
categories:
    - 有用的🌌
tags:
    - ArrowOS
    - Android
---

**recovery fix**

修改 BoardConfig.mk
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

**AOD 自适应亮度**

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
sepolicy_vndr 里面的 sepolicy.mk 最后一行的 xxxx 处修改为当前项目名称
```
-include device/xxxx/sepolicy/qcom/sepolicy.mk
```
**Import vendor/lawnchair/lawnchair.mk**

```
$(call inherit-product-if-exists, vendor/lawnchair/lawnchair.mk)
```
Remove existing launcher from the build

This can be done by adding package name of such launcher in overrides for Lawnchair package definition in Android.bp
