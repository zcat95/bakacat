---
title: Openwrt 解决 NTFS 挂载问题
date: 2022-08-16
slug: owt5
categories:
    - 有用的🌌
tags:
    - openwrt
    - NTFS
    - 软路由
---

**相关依赖调整**

只保留 ntfs-3g 和 ntfs-3g-utils

**重启后无法自动挂载**

系统 - 启动项 - 本地启动脚本添加
```
mount -t ntfs-3g /dev/sdb1 /mnt/sdb1
```

**挂载出现错误**

错误如下
```
$MFTMirr does not match $MFT (record 0).
Failed to mount '/dev/sdb1': Input/output error
NTFS is either inconsistent, or there is a hardware fault, or it's a
SoftRAID/FakeRAID hardware. In the first case run chkdsk /f on Windows
then reboot into Windows twice. The usage of the /f parameter is very
important! If the device is a SoftRAID/FakeRAID then first activate
it and mount a different device under the /dev/mapper/ directory, (e.g.
/dev/mapper/nvidia_eahaabcc1). Please see the 'dmraid' documentation
for more details.
```

使用此命令修复，即可重新挂载
```
ntfsfix /dev/sdb1
```

