# 系统配置
----

用户可以在终端界面进行基本的设置。

## 基本连接

------

### 通过HDMI登入系统

云冠2 预装 Ubuntu 18.04 操作系统。连接带有 HDMI 接口的显示器，鼠标和键盘，接入 云冠2 电源后，使用如下信息进行登录：

用户名：NVIDIA  密码：NVIDIA

### 通过OTG登入系统

将(Usb 转 Micro 线)连接 OTG 模块和电脑，USB的host端连接电脑， device端连接Icrest3的OTG2.0接口。

<img src="../images/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252Ffda9ebe7-7d37-420f-a075-a972e4a57b6d%252FUntitled.png" alt="img" style="zoom:50%;" />

电脑端会弹出盘符，同时设备管理器会有个串口设备。

<img src="../images/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F46ecd7fe-c441-48ab-a453-a2e9a7cb8f9f%252FUntitled.png" alt="img" style="zoom:50%;" />
使用 SSH进行连接。主机名：192.168.55.1，用户名： nvidia，密码：nvidia, 端口：22

<img src="../images/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F53ae48dc-3507-469f-9198-e777058b957f%252FUntitled.png" alt="img" style="zoom:50%;" />







## 网络配置

------

使用网线、WIFI或 5G网络接入网络，若所在网络自带 DHCP 服务，将会自动获取 IP 地址。若无 DHCP 服务，可通过以下命令手动配置：

**有线网络（接入扩展模块）**

```
$ sudo ifconfig eth1 xxx.xxx.xxx.xxx
$ ifconfig
```

**无线网络**

```
$ sudo ifconfig wlan0 xxx.xxx.xxx.xxx
$ ifconfig
```

“xxx.xxx.xxx.xxx”为所需使用的IP地址。亦可通过该命令检查是否配置正确。

**5G网络**

```
$ cd quectel-CM/
$ sudo ./quectel-CM
```

5G模块会自动分配IP。亦可通过ifconfig检查是否配置正确。
