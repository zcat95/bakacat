---
title: 单独升级 Xray Core
date: 2021-10-20
slug: xraycore-update
categories:
    - 有用的🌌
tags:
    - openwrt
    - 软路由
    - 配置
    - Xray
---

1. 下载对应的 Xray-linux-*.zip，解压出可执行文件 xray

2. FTP 连接 OpenWrt，替换 /usr/bin/xray

3. SSH 连接 OpenWrt，执行 chmod +x /usr/bin/xray