# 登入系统和5G/LTE网络配置

用户可以在终端界面进行基本的设置。

### 通过HDMI登入系统

Icrest3预装Ubuntu 18.04操作系统。连接带有HDMI接口的显示器，鼠标和键盘，开启Icrest3电源后，使用如下信息进行登录：

Account：nvidia

Password：nvidia

### 通过OTG cable登入系统

> ⚠️该方式仅支持M300RTK，如连接的无人机为M350 RTK、M30系列、M3E/T不能通过该方法登入系统。

1. 将Micro USB线连接Icrest3 OTG口和电脑，USB的host端连接电脑， device端连接Icrest3的OTG接口。
2. 电脑端会弹出L4T的盘符，同时设备管理器会有个CDC NCM的设备。
3. 使用 SSH进行连接，通过window自带的cmd终端工具，输入如下命令

```
ssh nvidia@192.168.55.1
```

> password: nvidia

<img src="../images/%E9%80%9A%E8%BF%87OTG%20cable%E7%99%BB%E5%85%A5%E7%B3%BB%E7%BB%9F%20.jpg" alt="通过OTG cable登入系统 " style="zoom: 25%;" />

### 通过以太网有线网络SSH登入系统

1. 通过接口扩展模块，将以太网连接至Icrest3
2.  获取Icrest3 IP地址
3. 确保HOST PC的IP和Icrest3 IP处于同一网段

4. 在HOST PC上使用SSH工具进行Icrest3 以太网连接，例如Icrest 3 IP地址为 192.168.x.x，剩下步骤可参照“OTG cable登入系统”方法进行SSH连接

### 通过DEBUG UART登入系统

1. 通过接口扩展模块，将DEBUG UART连接至HOST PC串口软件工具，选择相应的COM端口，并将串口软件工具波特率设置为115000
2. 输入用户名和密码：nvidia nvida

![通过DEBUG UART登入系统 ](../images/%E9%80%9A%E8%BF%87DEBUG%20UART%E7%99%BB%E5%85%A5%E7%B3%BB%E7%BB%9F%20.jpg)

### 通过WIFI登入系统

1. 通过DEBUG UART 登入系统
2. 输入如下命令，将Icrest3连接到Wi-Fi网络

```
sudo nmcli dev wifi connect "your WIFI SSID" password "your WIFI password"
```

3. 打印successfully成功连上

4. 成功连接WIFI后，剩下步骤可参考“通过以太网有线网络SSH登入系统”步骤

### 使用5G/LTE网络

Icrest3-5G出厂已安装好了5G驱动和自动拨号，插入Nano SIM1卡后，即可自动连接到互联网。5G模块会自动分配IP。亦可通过ifconfig检查是否配置正确。

![拨号成功](../images/%E6%8B%A8%E5%8F%B7%E6%88%90%E5%8A%9F.jpg)