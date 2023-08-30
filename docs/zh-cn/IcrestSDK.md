## 5G
### 配置文件config.ini说明

rtmpAddress = rtmp://1056428d9d253aa78    // RTMP推流地址

rtmp_on = true                                                  // RTMP推流的状态

5G_on = true                                                     //  5G的工作状态

pingAddress = 202.108.22.5                             //   用于ping外网的IP地址

system_id = 1                                                    //   用于mavlink的system_id

### **执行程序**

1. cd /home/nvidia/workspace3
2. sudo chmod 777 bin/*
3. sudo ./dji_sdk_demo_linux_cxx 192.168.xx.xx   #192.168.xx.xx为上位机的IP

### **查看云冠 ID**

cat /sys/firmware/devicetree/base/serial-number