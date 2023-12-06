# MAVLINK协议对接

Created time: January 9, 2023 9:52 AM
Last edited time: January 30, 2023 4:44 PM
Tags: Guides

## 通用消息集

MAVLink公共消息集包含由MAVLink项目管理的标准定义。定义涵盖了被认为对大多数地面控制站和自动驾驶仪有用的功能。希望与mavlink兼容的系统尽可能使用这些定义(如果存在适当的消息)，而不是以自己的方言推出变体。

原始定义在common.xml中定义。

## 飞行控制

飞行控制功能能够控制无人机执行基础飞行动作，可通过Joystick 功能控制无人机执行复杂的飞行动作。

### 起飞

| Param (:Label) | Description |
| --- | --- |
| command | MAV_CMD_NAV_TAKEOFF（22） |
| 其他 | 空 |
| 输出 | COMMAND_ACK |

### 降落

| Param (:Label) | Description |
| --- | --- |
| command | MAV_CMD_NAV_LAND（21） |
| 其他 | 空 |
| 输出 | COMMAND_ACK |

### 返航

| Param (:Label) | Description |
| --- | --- |
| command | MAV_CMD_NAV_RETURN_TO_LAUNCH（20） |
| 其他 | 空 |
| 输出 | COMMAND_ACK |

### 指点飞行

将车辆重新定位到特定的 WGS84 全局位置。

| Param (:Label) | Description | Units |
| --- | --- | --- |
| command | MAV_CMD_DO_REPOSITION（192） |  |
| param5 | 纬度 |  |
| param6 | 经度 |  |
| param7 | 高度 | m |
| 其他 | 空 |  |

### 悬停

| Param (:Label) | Description |
| --- | --- |
| command | MAV_CMD_DO_REPOSITION（192） |
| 其他 | 空 |
| 输出 | COMMAND_ACK |

### 虚拟摇杆控制

MAVLINK_MSG_ID_MANUAL_CONTROL   69

| Param (:Label) | Type | Description |
| --- | --- | --- |
| x | int16_t | x轴，归一化到范围[-1000,1000]; 通常对应于操纵杆的前进(1000)-向后(-1000)移动; |
| y | int16_t | y轴，归一化到范围[-1000,1000]; 通常对应于操纵杆的左移(1000)-后移(-1000)移动; |
| z | int16_t | z轴，归一化到范围[0,1000]; 通常对应于操纵杆的上升(1000)-下降(0)移动; |
| r | int16_t | r轴，归一化到范围[-1000,1000]; 通常对应于机头旋转，正数为顺时针旋转，负数为逆时针旋转。 |
| 其他 |  | 空 |

### 设置系统模式

包括打开虚拟遥控、返航、降落、任务等模式设置

| Param (:Label) | Description | Values |
| --- | --- | --- |
| param1 | 模式 | MAV_MODE_FLAG |
| param2 | 自定义模式-这是系统特定的，请参阅个别自动驾驶仪规格的详细信息。 | %E5%8D%8F%E8%AE%AE%E5%AF%B9%E6%8E%A5%20b3fc62419a1a4fb2b49cb2a44d370965.md |
| param3 | 自定义子模式-这是系统特定的，请参阅个别自动驾驶仪规格的详细信息。 | https://www.notion.so/d0e695391af642d19ad441fc4d565362 |
| 其他 | 空 |  |

## 相机

用户能够通过命令来控制相机类负载设备执行指定的动作，获取负载设备中的信息和资源。

### 拍照

| Param (:Label) | Description |
| --- | --- |
| command | MAV_CMD_IMAGE_START_CAPTURE（2000） |
| 其他 | 空 |
| 输出 | COMMAND_ACK |

### 开始录像

| Param (:Label) | Description |
| --- | --- |
| command | MAV_CMD_VIDEO_START_CAPTURE（2500） |
| 其他 | 空 |
| 输出 | COMMAND_ACK |

### 停止录像

| Param (:Label) | Description |
| --- | --- |
| command | MAV_CMD_VIDEO_STOP_CAPTURE（2501） |
| 其他 | 空 |
| 输出 | COMMAND_ACK |

### 变焦

| Param (:Label) | Description |
| --- | --- |
| command | MAV_CMD_SET_CAMERA_ZOOM（531） |
| param2 | 变焦倍数 |
| 其他 | 空 |
| 输出 | COMMAND_ACK |

### 设置相机模式

| Param (:Label) | Description |  |
| --- | --- | --- |
| command | MAV_CMD_SET_CAMETA_MODE（530） |  |
| param2 | 相机模式 | %E5%8D%8F%E8%AE%AE%E5%AF%B9%E6%8E%A5%20b3fc62419a1a4fb2b49cb2a44d370965.md |
| 其他 | 空 |  |
| 输出 | COMMAND_ACK |  |

### 获取相机类型

相机类型将在COMMAND_ACK的result_param2参数中返回

| Param (:Label) | Description | Values |
| --- | --- | --- |
| command | MAV_CMD_CUSTOM_GET_CAMETA_TYPE（535） |  |
| 其他 | 空 |  |
| 输出 | COMMAND_ACK | %E5%8D%8F%E8%AE%AE%E5%AF%B9%E6%8E%A5%20b3fc62419a1a4fb2b49cb2a44d370965.md |

## 云台

用户可基于云台管理的接口，设计控制逻辑，从而实现指定的云台旋转对应角度的功能。

### 云台控制

| Param (:Label) | Description | Values | Unit |
| --- | --- | --- | --- |
| command | VEHICLE_CMD_DO_GIMBAL_MANAGER_PITCHYAW（1000） |  |  |
| Param1 | 控制云台的俯仰角 |  |  |
| Param2 | 控制云台的偏航角 |  |  |
| Param3 | 云台执行的时间 |  | second |
| Param5 | mode | %E5%8D%8F%E8%AE%AE%E5%AF%B9%E6%8E%A5%20b3fc62419a1a4fb2b49cb2a44d370965.md |  |
| 其他 | 空 |  |  |
| 输出 | COMMAND_ACK |  |  |

## 视频流

用户可通过设置RTMP的推流地址，实时去查看视频流画面。

### 设置推流地址

MAVLINK_MSG_ID_STRING_COMMAND_LONG 173

| Param (:Label) | Description |
| --- | --- |
| command | VIDEO_STREAM_RTMP_MODE（0） |
| param1 | 推流地址 |
| 其他 | 空 |
| 输出 | COMMAND_ACK |

### 设置视频流码率

| Param (:Label) | Description | Values |
| --- | --- | --- |
| command | MAV_CMD_SET_VIDEOSTREAM_BITRATE（2506） |  |
| param1 | 码率 | %E5%8D%8F%E8%AE%AE%E5%AF%B9%E6%8E%A5%20b3fc62419a1a4fb2b49cb2a44d370965.md |
| 其他 | 空 |  |
| 输出 | COMMAND_ACK |  |

### 获取视频流码率

码率将在COMMAND_ACK的result_param2参数中返回

| Param (:Label) | Description | Values |
| --- | --- | --- |
| command | MAV_CMD_GET_VIDEOSTREAM_BITRATE（2507） |  |
| 其他 | 空 |  |
| 输出 | COMMAND_ACK | %E5%8D%8F%E8%AE%AE%E5%AF%B9%E6%8E%A5%20b3fc62419a1a4fb2b49cb2a44d370965.md |

### 设置视频流分辨率

| Param (:Label) | Description | Values |
| --- | --- | --- |
| command | MAV_CMD_CUSTOM_SET_VIDEOSTREAM_RESOLUTION（2508） |  |
| param1 | 分辨率 | %E5%8D%8F%E8%AE%AE%E5%AF%B9%E6%8E%A5%20b3fc62419a1a4fb2b49cb2a44d370965.md |
| 其他 | 空 |  |
| 输出 | COMMAND_ACK |  |

### 获取视频流分辨率

分辨率将在COMMAND_ACK的result_param2参数中返回

| Param (:Label) | Description | Values |
| --- | --- | --- |
| command | MAV_CMD_CUSTOM_GET_VIDEOSTREAM_RESOLUTION（2509） |  |
| 其他 | 空 |  |
| 输出 | COMMAND_ACK | %E5%8D%8F%E8%AE%AE%E5%AF%B9%E6%8E%A5%20b3fc62419a1a4fb2b49cb2a44d370965.md |

### FPV视频流

| Param (:Label) | Description |
| --- | --- |
| command | MAV_CMD_CUSTOM_SET_FPV_CAMERA（412） |
| 其他 | 空 |
| 输出 | COMMAND_ACK |

### 云台相机视频流

| Param (:Label) | Description |
| --- | --- |
| command | MAV_CMD_CUSTOM_SET_MAIN_CAMERA（413） |
| 其他 | 空 |
| 输出 | COMMAND_ACK |

### 切换相机

| Param (:Label) | Description | Values |
| --- | --- | --- |
| command | MAV_CMD_CUSTOM_SET_CAMERA_SOURCE（414） |  |
| param1 | 相机源 | %E5%8D%8F%E8%AE%AE%E5%AF%B9%E6%8E%A5%20b3fc62419a1a4fb2b49cb2a44d370965.md |
| 其他 | 空 |  |
| 输出 | COMMAND_ACK |  |

### 获取相机源

相机源将在COMMAND_ACK的result_param2参数中返回

| Param (:Label) | Description | Values |
| --- | --- | --- |
| command | MAV_CMD_CUSTOM_GET_CAMERA_SOURCE（415） |  |
| 其他 | 空 |  |
| 输出 | COMMAND_ACK | %E5%8D%8F%E8%AE%AE%E5%AF%B9%E6%8E%A5%20b3fc62419a1a4fb2b49cb2a44d370965.md |

## 通用功能

### 获取飞机序列号

飞机序列号将在COMMAND_ACK的string_param1参数中返回

| Param (:Label) | Description |
| --- | --- |
| command | MAV_CMD_CUSTOM_GET_AIRCRAFT_SERIAL（502） |
| 其他 | 空 |
| 输出 | COMMAND_ACK2 |

## 状态信息

### 心跳包

MAVLINK_MSG_ID_HEARTBEAT（0）

| Field Name | Description | Type | Values |
| --- | --- | --- | --- |
| type | 车辆或部件类型。对于飞行控制器组件，飞行器类型(四旋翼、直升机等)。 | uint8_t | MAV_TYPE |
| autopilot | 自动驾驶类型/等级。 | uint8_t | MAV_AUTOPILOT |
| base_mode |  | uint8_t | MAV_MODE_FLAG |
| custom_mode | 飞行模式;                                                      main_mode: 前十六位的低八位                                                     sub_mode:前十六位的高八位  | uint32_t | %E5%8D%8F%E8%AE%AE%E5%AF%B9%E6%8E%A5%20b3fc62419a1a4fb2b49cb2a44d370965.md                           %E5%8D%8F%E8%AE%AE%E5%AF%B9%E6%8E%A5%20b3fc62419a1a4fb2b49cb2a44d370965.md |
| system_status | 系统状态标志。 | uint8_t | MAV_STATE |

### 系统状态

MAVLINK_MSG_ID_SYS_STATUS（1）

| Field Name | Description | Type | Units |
| --- | --- | --- | --- |
| voltage_battery | 电池电压 | uint16_t | % |
| current_battery | 电池电流 | int16_t | cA |
| battery_remaining | 电池剩余电量 | int8_t | % |

### GPS原始值

MAVLINK_MSG_ID_GPS_RAW_INT（24）

| Field Name | Description | Type | Units | Values |
| --- | --- | --- | --- | --- |
| time_usec | 时间戳(UNIX纪元时间或自系统引导以来的时间)。接收端可以通过检查数字的大小来推断时间戳格式(自1.1.1970年以来或自系统引导以来)。 | uint64_t | us |  |
| fix_type | GPS固定类型。 | uint8_t |  | GPS_FIX_TYPE |
| lat | 纬度(WGS84, EGM96椭球) | int32_t | degE7 |  |
| lon | 经度(WGS84, EGM96椭球) | int32_t | degE7 |  |
| alt | 高度(MSL)。正向上。请注意，除了WGS84高度之外，几乎所有GPS模块都提供MSL高度。 | int32_t | mm |  |
| eph | GPS HDOP水平位置稀释(无单位* 100)。如果未知，设置为:UINT16_MAX | uint16_t |  |  |
| epv | GPS VDOP垂直位置稀释(无单位* 100)。如果未知，设置为:UINT16_MAX | uint16_t |  |  |
| vel | GPS地面速度。如果未知，设置为:UINT16_MAX | uint16_t | cm/s |  |
| cog | 经过地面(不是航向，而是运动方向)，度数* 100,0.0 ..359.99度。如果未知，设置为:UINT16_MAX | uint16_t | cdeg |  |
| satellites_visible | 可见卫星数目。如果未知，设置为UINT8_MAX | uint8_t |  |  |
| alt_ellipsoid | 高度(高于WGS84, EGM96椭球)。正向上。 | int32_t | mm |  |
| h_acc | 位置不确定性. | uint32_t | mm |  |
| v_acc | 高度的不确定性。 | uint32_t | mm |  |
| vel_acc | 速度的不确定性。 | uint32_t | mm |  |
| hdg_acc | 航向/航迹不确定性 | uint32_t | degE5 |  |
| yaw | 偏航在土框架从北。如果GPS不提供偏航，请使用0。使用UINT16_MAX如果这个GPS被配置为提供偏航，目前无法提供它。北上使用36000。 | uint16_t | cdeg |  |

### 电源状态

MAVLINK_MSG_ID_POWER_STATUS（25）

| Field Name | Type | Units | Values | Description |
| --- | --- | --- | --- | --- |
| Vcc | uint16_t | mV |  | 电池电压 |
| Vservo | uint16_t | mV |  | 电池电压 |

### 飞行器的姿态

MAVLINK_MSG_ID_ATTITUDE（30）

| Field Name | Description | Type | Units |
| --- | --- | --- | --- |
| time_boot_ms | 时间戳(自系统引导以来的时间)。 | uint32_t | ms |
| roll | 横滚角 | float | rad |
| pitch | 俯仰角 | float | rad |
| yaw | 偏航角 | float | rad |
| rollspeed | 横滚角速度 | float | rad/s |
| pitchspeed | 俯仰角速度 | float | rad/s |
| yawspeed | 偏航角速度 | float | rad/s |

### 飞行器的全球位置

MAVLINK_MSG_ID_GLOBAL_POSITION_INT（33）

| Field Name | Description | Type | Units |
| --- | --- | --- | --- |
| time_boot_ms | 时间戳(自系统引导以来的时间)。 | uint32_t | ms |
| lat | 纬度 | int32_t | degE7 |
| lon | 经度 | int32_t | degE7 |
| alt | 海拔高度 (MSL)。 | int32_t | mm |
| relative_alt | 相对高度 | int32_t | mm |
| vx | 地面X速度(纬度，正北) | int16_t | cm/s |
| vy | 地面Y速度(经度，正东) | int16_t | cm/s |
| vz | 地面Z速度(高度，正向下) | int16_t | cm/s |
| hdg | 车辆航向(偏航角)，0.0..359.99度。如果未知，设置为:UINT16_MAX | uint16_t |  cdeg  |

### 云台信息

MAVLINK_MSG_ID_GIMBAL_STATUS （170）

| Field Name | Description | Type |
| --- | --- | --- |
| gimbal_pitch | 俯仰角 | float |
| gimbal_roll | 横滚角 | float |
| gimbal_yaw | 偏航角 | float |

### LTE信息

MAVLINK_MSG_ID_LTE_STATUS （171）

| Field Name | Description | Type |
| --- | --- | --- |
| lte_temperature | 温度 | int16_t |
| lte_rsrp | 信号接收功率 | int8_t |
| lte_sinr | 信号与干扰加噪声比 | int8_t |
| lte_network_delay | 网络延迟 | float |

### HMS信息

MAVLINK_MSG_ID_HMS_ALARM（175）

| Field Name | Description | Type |
| --- | --- | --- |
| alarmCode | 警告码 | uint32_t |
| alarmLevel | 警告等级 | int8_t |

### HOME点位置信息

MAVLINK_MSG_ID_HOME_POSITION（242）

| Field Name | Description | Type | Units |
| --- | --- | --- | --- |
| latitude | 纬度(WGS84) | int32_t | degE7 |
| longitude | 经度(WGS84) | int32_t | degE7 |
| altitude | 高度(MSL)。正向上。 | int32_t | mm |

### 其他系统状态

MAVLINK_MSG_ID_EXTENDED_SYS_STATE（245）

| Field Name | Description | Type | Values |
| --- | --- | --- | --- |
| landed_state | 着陆态。如果着陆状态未知，则设置为MAV_LANDED_STATE_UNDEFINED。 | uint8_t | MAV_LANDED_STATE |

## 枚举定义

### MAV_RESULT

COMMAND_ACK返回结果

| Value | Field Name | Description |
| --- | --- | --- |
| 0 | MAV_RESULT_ACCEPTED | 成功 |
| 1 | MAV_RESULT_TEMPORARILY_REJECTED | reserve |
| 2 | MAV_RESULT_DENIED | 无效 |
| 3 | MAV_RESULT_UNSUPPORTED | 不支持 |
| 4 | MAV_RESULT_FAILED | 失败 |
| 5 | MAV_RESULT_IN_PROGRESS | reserve |
| 6 | MAV_RESULT_CANCELLED | 取消 |

### PX4_CUSTOM_MAIN_MODE

工作模式

| Value | Field Name | Description |
| --- | --- | --- |
| 1 | PX4_CUSTOM_MAIN_MODE_MANUAL  | 手动模式 |
| 2 | PX4_CUSTOM_MAIN_MODE_ALTCTL |  |
| 3 | PX4_CUSTOM_MAIN_MODE_POSCTL | 位置控制模式 |
| 4 | PX4_CUSTOM_MAIN_MODE_AUTO | 自动模式 |
| 5 | PX4_CUSTOM_MAIN_MODE_ACRO | 特技模式 |
| 6 | PX4_CUSTOM_MAIN_MODE_STABILIZED | 自稳模式 |
| 7 | PX4_CUSTOM_MAIN_MODE_RATTITUDE_LEGACY |  |
| 8 | PX4_CUSTOM_MAIN_MODE_SIMPLE  |  |

### PX4_CUSTOM_SUB_MODE_AUTO

工作状态

| Value | Field Name | Description |
| --- | --- | --- |
| 1 | PX4_CUSTOM_SUB_MODE_AUTO_READY  | 准备模式 |
| 2 | PX4_CUSTOM_SUB_MODE_AUTO_TAKEOFF | 起飞模式 |
| 3 | PX4_CUSTOM_SUB_MODE_AUTO_LOITER | 留待模式(gps悬停模式) |
| 4 | PX4_CUSTOM_SUB_MODE_AUTO_MISSION | 任务模式 |
| 5 | PX4_CUSTOM_SUB_MODE_AUTO_RTL | 返航模式 |
| 6 | PX4_CUSTOM_SUB_MODE_AUTO_LAND | 降落模式 |
| 7 | PX4_CUSTOM_SUB_MODE_AUTO_RESERVED_DO_NOT_USE |  |
| 8 | PX4_CUSTOM_MAIN_MOPX4_CUSTOM_SUB_MODE_AUTO_FOLLOW_TARGETDE_SIMPLE  |  |
| 9 | PX4_CUSTOM_SUB_MODE_AUTO_PRECLAND |  |
| 10 | PX4_CUSTOM_SUB_MODE_AUTO_VTOL_TAKEOFF |  |

### VIDEOSTREAM_SOLUTION

视频流分辨率

| Value | Field Name | Description |
| --- | --- | --- |
| 0 | VIDEOSTREAM_RESOLUTION_1080P | 1920x1028 |
| 1 | VIDEOSTREAM_RESOLUTION_720P | 1280x720 |
| 2 | VIDEOSTREAM_RESOLUTION_VGA | 640x480 |

### VIDEOSTREAM_BITRATE

视频流码率

| Value | Field Name | Description |
| --- | --- | --- |
| 1 | VIDEOSTREAM_BITRATE_1Mbps  | 1Mbps |
| 2 | VIDEOSTREAM_BITRATE_2Mbps  | 2Mbps |
| 3 | VIDEOSTREAM_BITRATE_3Mbps  | 3Mbps |
| 4 | VIDEOSTREAM_BITRATE_4Mbps  | 4Mbps |
| 5 | VIDEOSTREAM_BITRATE_5Mbps | 5Mbps |
| 6 | VIDEOSTREAM_BITRATE_6Mbps  | 6Mbps |
| 7 | VIDEOSTREAM_BITRATE_7Mbps  | 7Mbps |
| 8 | VIDEOSTREAM_BITRATE_8Mbps  | 8Mbps |

### GIMBAL_ROTATION_MODE

云台模式

| Value | Field Name | Description |
| --- | --- | --- |
| 0 | GIMBAL_ROTATION_MODE_RELATIVE  | 相对控制 |
| 1 | GIMBAL_ROTATION_MODE_ABSOLUTE  | 绝对控制 |
| 2 | GIMBAL_ROTATION_MODE_SPEED     | 速度控制 |
| 3 | GIMBAL_ROTATION_MODE_RESET     | 复位 |

### MAV_CMD_CUSTOM

自定义Command Long命令

| Value | Field Name | Description |
| --- | --- | --- |
| 412 | MAV_CMD_CUSTOM_SWITCH_FPV_CAMERA | 切换FPV相机流 |
| 413 | MAV_CMD_CUSTOM_SWITCH_MAIN_CAMERA  | 切换主挂载相机流 |
| 414 | MAV_CMD_CUSTOM_SWITCH_CAMERA_SOURCE  | 切换相机 |
| 502 | MAV_CMD_CUSTOM_GET_CAMETA_TYPE  | 获取相机类型 |
| 535 | MAV_CMD_CUSTOM_GET_AIRCRAFT_SERIAL  | 获取飞行序列号 |
| 2506 | MAV_CMD_CUSTOM_SET_VIDEOSTREAM_BITRATE  | 设置视频流码率 |
| 2507 | MAV_CMD_CUSTOM_GET_VIDEOSTREAM_BITRATE  | 获取视频流码率 |
| 2508 | MAV_CMD_CUSTOM_SET_VIDEOSTREAM_RESOLUTION  | 设置视频流分辨率 |
| 2509 | MAV_CMD_CUSTOM_GET_VIDEOSTREAM_RESOLUTION  | 获取视频流分辨率 |

### MAV_STRING_CMD

自定义Command String命令

| Value | Field Name | Description |
| --- | --- | --- |
| 0 | VIDEO_STREAM_RTMP_MODE | 设置RTMP推流 |

### E_DjiLiveViewCameraSource

相机源

| Value | Field Name |
| --- | --- |
| 0 | DJI_LIVEVIEW_CAMERA_SOURCE_DEFAULT  |
| 1 | DJI_LIVEVIEW_CAMERA_SOURCE_H20_WIDE  |
| 2 | DJI_LIVEVIEW_CAMERA_SOURCE_H20_ZOOM  |
| 1 | DJI_LIVEVIEW_CAMERA_SOURCE_H20T_WIDE |
| 2 | DJI_LIVEVIEW_CAMERA_SOURCE_H20T_ZOOM  |
| 3 | DJI_LIVEVIEW_CAMERA_SOURCE_H20T_IR |
| 1 | DJI_LIVEVIEW_CAMERA_SOURCE_H20N_WIDE  |
| 2 | DJI_LIVEVIEW_CAMERA_SOURCE_H20N_ZOOM  |
| 3 | DJI_LIVEVIEW_CAMERA_SOURCE_H20N_IR  |
| 1 | DJI_LIVEVIEW_CAMERA_SOURCE_M30_ZOOM  |
| 2 | DJI_LIVEVIEW_CAMERA_SOURCE_M30_WIDE  |
| 1 | DJI_LIVEVIEW_CAMERA_SOURCE_M30T_ZOOM  |
| 2 | DJI_LIVEVIEW_CAMERA_SOURCE_M30T_WIDE  |
| 3 | DJI_LIVEVIEW_CAMERA_SOURCE_M30T_IR  |
| 1 | DJI_LIVEVIEW_CAMERA_SOURCE_M3E_VIS  |
| 1 | DJI_LIVEVIEW_CAMERA_SOURCE_M3T_VIS  |
| 2 | DJI_LIVEVIEW_CAMERA_SOURCE_M3T_IR  |

### E_DjiCameraType

相机类型

| Value | Field Name |
| --- | --- |
| 0 | DJI_CAMERA_TYPE_UNKNOWN  |
| 20 | DJI_CAMERA_TYPE_Z30  |
| 26 | DJI_CAMERA_TYPE_XT2  |
| 31 | DJI_CAMERA_TYPE_PSDK  |
| 41 | DJI_CAMERA_TYPE_XTS  |
| 42 | DJI_CAMERA_TYPE_H20  |
| 43 | DJI_CAMERA_TYPE_H20T  |
| 61 | DJI_CAMERA_TYPE_H20N  |
| 50 | DJI_CAMERA_TYPE_P1  |
| 51 | DJI_CAMERA_TYPE_L1 |
| 52 | DJI_CAMERA_TYPE_M30 |
| 53 | DJI_CAMERA_TYPE_M30T |
| 54 | DJI_CAMERA_TYPE_M3E |
| 55 | DJI_CAMERA_TYPE_M3T |

### E_DjiCameraManagerWorkMode

相机模式

| Value | Field Name |
| --- | --- |
| 0 | DJI_CAMERA_MANAGER_WORK_MODE_SHOOT_PHOTO |
| 1 | DJI_CAMERA_MANAGER_WORK_MODE_RECORD_VIDEO |
| 2 | DJI_CAMERA_MANAGER_WORK_MODE_PLAYBACK |
| 3 | DJI_CAMERA_MANAGER_WORK_MODE_MEDIA_DOWNLOAD |

<aside>
📢 其他枚举定义在conmand.h文件中

</aside>