---
title: Openwrt 编译笔记
date: 2022-08-14
slug: owt4
categories:
    - 有用的🌌
tags:
    - openwrt
    - 编译
    - 软路由
---

### **修改默认 IP**

package/base-files/files/bin/config_generate 路径下，搜索关键字 IP 修改后保存即可。

------

### **启用 Samba4 / 关闭 Samba3.6**

Extra packages 取消勾选 autosamba，相关选项就会自动取消。
Luci - App - 启用 luci-app-samba4 ，相关选项会自动启用。
Network - samba4-util 需要启用。

------

### **启用 IPV6**

Extra packages - ipv6helper 启用即可。

------

### **启用 NTFS 支持**

不要选择 kmod-fs-ntfs ，这个只是提供 NTFS 只读支持。
```
Utilities  ---> disc --->   <*> fdisk
Utilities  --->   <*> usbutils
Utilities —> Filesystem —>  <*> ntfs-3g
Utilities —> Filesystem —>  <*> ntfs-3g-util
Kernel modules —> Block Devices —>  <*>kmod-scsi-core
Kernel modules —> USB Support —>  <*> kmod-usb-core
Kernel modules —> USB Support —>  <*> kmod-usb-ohci
Kernel modules —> USB Support —>  <*> kmod-usb-uhci
Kernel modules —> USB Support —>  <*> kmod-usb-storage
Kernel modules —> USB Support —>  <*> kmod-usb-storage-extras
Kernel modules —> USB Support —>  <*> kmod-usb2    ##usb2.0
Kernel modules —> USB Support —>  <*> kmod-usb3    ##usb3.0
Kernel modules —> Filesystems —>  <*> kmod-fs-ext4
Kernel modules —> Filesystems —>  <*> kmod-fs-vfat
Kernel modules —> Filesystems —>  <*> kmod-fuse  
```

------

### **SSRP**

源码地址 https://github.com/fw876/helloworld

```
rm -rf package/helloworld
git clone --depth=1 https://github.com/fw876/helloworld.git package/helloworld
git -C package/helloworld pull
```

------

### **更多参考**

看完这篇，自定义 OpenWrt/LEDE 路由固件不求人
https://sspai.com/post/61463

Lean OpenWrt 编译使用小记
https://chenhe.me/post/lean-openwrt-compile/

NTFS格式优盘挂载
https://github.com/danshui-git/shuoming/blob/master/NTFS%E6%A0%BC%E5%BC%8F%E4%BC%98%E7%9B%98%E6%8C%82%E8%BD%BD

openwrt编译USB以及常用软件
https://secm.top/index.php/archives/16/

软路由编译及ipv6、多播设置流程
https://agong.me/2019/soft-router-compile-ipv6-mppddial-by-top-navigation.html
