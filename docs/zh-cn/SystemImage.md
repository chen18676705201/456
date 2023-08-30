# 系统镜像

Icrest3 支持恢复模式、还原及镜像备份。连接Icrest3至主机（host）并进入恢复模式，可进行系统还原及镜像备份。

## 准备工作

------

1. 准备一台Ubuntu 系统的计算机作为主机，并确保硬盘空间大于32 GB
2. 访问以下网址获取最新DJI 官方镜像文件。

https://www.dji.com/manifold-2/info#downloads 

## 进入恢复模式

------



1. 连接Icrest3 的OTG2.0 接口（Micro-B）至主机。
2. 按住Icrest3的RCV 按键，连接Icrest3 电源，松开RCV按键。
3. 在主机的终端界面中输入`$ lsusb`，若显示有NVIDIA 设备，则成功进入恢复模式。若未显示NVIDIA 设备，则检查连线及进入方式是否正确，然后重试。

## 镜像备份

------

用户可自行制作镜像备份用于系统还原。若使用DJI 官方镜像进行还原，则无需进行镜像备份。镜像备份仅备份eMMC 中的内容，SSD 中的数据将不会备份。

## 系统还原

------

### 使用Nvidia官方镜像

### 使用Icrest3官方镜像

### 使用自制镜像

1. 进入恢复模式。

2. 在主机的终端界面，进入DJI 官方镜像文件所在目录，然后输入以下命令，以解压官方镜像压缩包。 

   `$ sudo tar -zxvf manifold2G_image_Vx.x.x.x.tar.gz`

   “manifold2G_image_Vx.x.x.x.tar.gz”为压缩包文件名。

> ⚠️务必使用sudo 超级权限进行解压，否则将导致系统还原失败。