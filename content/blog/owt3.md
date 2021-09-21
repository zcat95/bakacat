+++
date = "2021-09-21T16:00:00+04:00"
draft = false
title = "Openwrt 带离线下载功能的私有云搭建"

+++

### Docker 配置

1. ssh 登录到软路由，然后输入 

   ```
   /etc/docker-init
   ```

   自动分区剩余硬盘空间为 ext4

2. 回到 luci 的挂载点，点击生成配置

3. 选择新分区的 UUID ，点击修改

4. 启用挂载点，并选择挂载为 Docker 的 /opt 数据分区，然后点击高级设置，选择 ext4 格式

5. 启用你需要作为存储空间的挂载点，并选择挂载为 /mnt/xxx （视情况而定，也可以自定义为其他路径），然后点击高级设置，选择 ext4 格式，重启软路由。

6. 重启后进入 ssh 输入 

   ```
   /etc/docker-web
   ```

   这时将会自动下载安装启动好 Docker 的 Web 管理界面

7. 运行成功后，在 Web 输入你的路由器 IP:9999 端口，设置好管理员密码，选择 local Docker 即可进入管理页面

------

### Cloudreve 配置

1. 下载 cloudreve

   ```
   docker pull xavierniu/cloudreve
   ```

2. 创建相关文件夹

   ```
   mkdir -p ~/cloudreve/uploads \
       && mkdir -p ~/cloudreve/avatar \
       && touch ~/cloudreve/conf.ini \
       && touch ~/cloudreve/cloudreve.db
   ```

3. 启动 cloudreve

   ```
   docker run -d \
   >   --name cloudreve \
   >   -e PUID=$UID \
   >   -e PGID=$GID \
   >   -e TZ="Asia/Shanghai" \   #时区
   >   -p 5212:5212 \   #端口
   >   --restart=unless-stopped \
   >   -v /mnt/xxx:/cloudreve/uploads \   #文件存储路径为你的硬盘
   >   -v ~/cloudreve/conf.ini:/cloudreve/conf.ini \   #配置文件
   >   -v ~/cloudreve/cloudreve.db:/cloudreve/cloudreve.db \   #数据库文件
   >   -v ~/cloudreve/avatar:/cloudreve/avatar \   #头像路径
   >   -v /mnt/xxx/aria2-downloads:/downloads \   #aria2下载临时路径
   >   xavierniu/cloudreve
   ```

4. 获取密码

   ```
   docker logs -f cloudreve
   ```

5. 从你的路由器 IP:5212 进入主页，点击右上角进入管理面板 - 用户 - 选择一号用户后面的操作修改用户名、邮箱和密码

------

### Aria 2 配置

1. 下载 Aria2-pro

   ```
   docker pull p3terx/aria2-pro
   ```

2. 配置并启动，记住你自己填写的 token

   ```
   docker run -d \
       --name aria2-pro \
       --restart unless-stopped \
       --log-opt max-size=1m \
       --network host \
       -e PUID=$UID \
       -e PGID=$GID \
       -e RPC_SECRET=[token] \   #[token]自己填写
       -e RPC_PORT=6800 \
       -e LISTEN_PORT=6888 \
       -v $PWD/aria2-config:/config \
       -v /mnt/xxx/aria2-downloads:/downloads \   #下载目录也配置到你的硬盘内
       p3terx/aria2-pro
   ```

------

### 安装 AriaNg Ui

1. 自动安装并下载

   ```
   docker run -d \
       --name ariang \
       --restart unless-stopped \
       --log-opt max-size=1m \
       -p 6880:6880 \
       p3terx/ariang
   ```

2. 从你的路由器 IP:6880 进入 UI 界面，然后进入 AriaNg 设置，点击后面的 RPC ，输入你刚才的 token

------

### 配置离线下载

1. 在 Cloudreve 的管理面板中，点击参数设置 - 离线下载，RPC 服务器地址填写为 http://IP:6800 ，RPC Secret 填写刚刚的 token ，临时下载目录填写为 /downloads ，然后保存并测试连接，提示成功即可
2. 在 AriaNg 中，点击 Aria2 设置 - BitTorrent 设置 - BT 服务器地址，修改为你喜欢的 Tracker ，推荐使用 [XIU2/TrackersListCollection](https://github.com/XIU2/TrackersListCollection)

------

至此流程全部结束。