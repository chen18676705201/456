# 运行实例程序

------

## DJI PSDK

将Icrest3正确接入无人机后，按照开发调试步骤接好接好调试线，上电无人机。

- 如飞机为M300RTK，通过以下链接下载geoai配置脚本，将Icrest3设置为M300RTK模式后，在HOST 主机终端输入`ls /dev/tty*`，如显示下列设备，则表明Icrest3能检测到无人机

  http://gofile.me/79fsr/6LMONhwxT

|                       | Jetson NX版本 | Jetson Nano版本 |
| --------------------- | ------------- | --------------- |
| 无人机OSDK/E-Port串口 | ttyTHS1       | ttyTHS2         |
| 无人机OSDK/E-Port USB | ACM0          | ACM0            |



- 如飞机为M30系列、M350RTK，通过以下链接下载geoai配置脚本，将Icrest3设置为M30模式后，在HOST 主机终端输入`ifconfig`，如显示下列设备，则表明Icrest3能检测到无人机

  http://gofile.me/79fsr/6LMONhwxT

|                       | Jetson NX版本 | Jetson Nano版本 |
| --------------------- | ------------- | --------------- |
| 无人机PSDK/E-Port串口 | ttyTHS1       | ttyTHS2         |
| 无人机PSDK/E-PortUSB  | L4T           | L4T             |

以上步骤完成后，在[DJI 开发者网站](https://developer.dji.com/doc/payload-sdk-tutorial/en/)下载最新PSDK软件包运行即可
