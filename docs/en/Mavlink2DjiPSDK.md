>  IcrestSDK采用Mavlink通信协议，开发者可以直接通过QGC直接控制大疆无人机。
>
> 5G--> VPN-->QGC, 视频RTMP
>
> WIFI --> QGC
>
> 1.4G MESH--> QGC
mavlink官方网站：[Microservices · MAVLink Developer Guide]


一、飞行控制

起飞
Comand: MAV_CMD_NAV_TAKEOFF  =  22

降落
Comand: MAV_CMD_NAV_LAND  =  21

返航
Comand:MAV_CMD_NAV_RETURN_TO_LAUNCH  =  20

悬停或者指点飞行
Comand:MAV_CMD_DO_REPOSITION  =  192
Param5:纬度
Param6:经度
Param7:海拔高度

虚拟控制
MAVLINK_MSG_ID_MANUAL_CONTROL 69
x：控制方向前进/后退;  设置遥控指数【-1000, 1000】; 速度范围在【-10,10】之间
y：控制方向左移/右移  设置遥控指数【-1000, 1000】; 速度范围在【-10,10】之间
z：控制方向上升/下降   设置遥控指数【0, 1000】; 速度范围在【-10,10】之间
r：无人机机头旋转，正数为顺时针，负数为逆时针 设置遥控指数【-1000,1000】; 速度范围在【-10,10】之间


设置系统模式
Comand:MAV_CMD_DO_SET_MODE  =  176
Param1:Mode模式
Param2:Custom Mode自定义模式
Param3:Custom Submode自定义子模式
例如：
设置虚拟遥感模式
Comand:MAV_CMD_DO_SET_MODE  =  176
Param1：221
Param2：PX4_CUSTOM_MAIN_MODE_MANUAL
Param3：0

设置返航模式
Comand:MAV_CMD_DO_SET_MODE  =  176
Param1：221
Param2：PX4_CUSTOM_MAIN_MODE_AUTO
Param3：PX4_CUSTOM_SUB_MODE_AUTO_RTL



Mode
{
typedef enum VEHICLE_MODE_FLAG {
VEHICLE_MODE_FLAG_CUSTOM_MODE_ENABLED = 1, /* 0b00000001 Reserved for future use. | */
VEHICLE_MODE_FLAG_TEST_ENABLED = 2, /* 0b00000010 system has a test mode enabled. This flag is intended for temporary system tests and should not be used for stable implementations. | */
VEHICLE_MODE_FLAG_AUTO_ENABLED = 4, /* 0b00000100 autonomous mode enabled, system finds its own goal positions. Guided flag can be set or not, depends on the actual implementation. | */
VEHICLE_MODE_FLAG_GUIDED_ENABLED = 8, /* 0b00001000 guided mode enabled, system flies MISSIONs / mission items. | */
VEHICLE_MODE_FLAG_STABILIZE_ENABLED = 16, /* 0b00010000 system stabilizes electronically its attitude (and optionally position). It needs however further control inputs to move around. | */
VEHICLE_MODE_FLAG_HIL_ENABLED = 32, /* 0b00100000 hardware in the loop simulation. All motors / actuators are blocked, but internal software is full operational. | */
VEHICLE_MODE_FLAG_MANUAL_INPUT_ENABLED = 64, /* 0b01000000 remote control input is enabled. | */
VEHICLE_MODE_FLAG_SAFETY_ARMED = 128, /* 0b10000000 MAV safety set to armed. Motors are enabled / running / can start. Ready to fly. Additional note: this flag is to be ignore when sent in the command MAV_CMD_DO_SET_MODE and MAV_CMD_COMPONENT_ARM_DISARM shall be used instead. The flag can still be used to report the armed state. | */
VEHICLE_MODE_FLAG_ENUM_END = 129, /*  | */
} VEHICLE_MODE_FLAG;

}

Custom Mode
{
enum PX4_CUSTOM_MAIN_MODE {
PX4_CUSTOM_MAIN_MODE_MANUAL = 1,
PX4_CUSTOM_MAIN_MODE_ALTCTL,
PX4_CUSTOM_MAIN_MODE_POSCTL,
PX4_CUSTOM_MAIN_MODE_AUTO,
PX4_CUSTOM_MAIN_MODE_ACRO,
PX4_CUSTOM_MAIN_MODE_OFFBOARD,
PX4_CUSTOM_MAIN_MODE_STABILIZED,
PX4_CUSTOM_MAIN_MODE_RATTITUDE_LEGACY,
PX4_CUSTOM_MAIN_MODE_SIMPLE /* unused, but reserved for future use */
};
}
Custom Submode
{
enum PX4_CUSTOM_SUB_MODE_AUTO {
PX4_CUSTOM_SUB_MODE_AUTO_READY = 1,
PX4_CUSTOM_SUB_MODE_AUTO_TAKEOFF,
PX4_CUSTOM_SUB_MODE_AUTO_LOITER,
PX4_CUSTOM_SUB_MODE_AUTO_MISSION,
PX4_CUSTOM_SUB_MODE_AUTO_RTL,
PX4_CUSTOM_SUB_MODE_AUTO_LAND,
PX4_CUSTOM_SUB_MODE_AUTO_RESERVED_DO_NOT_USE, // was PX4_CUSTOM_SUB_MODE_AUTO_RTGS, deleted 2020-03-05
PX4_CUSTOM_SUB_MODE_AUTO_FOLLOW_TARGET,
PX4_CUSTOM_SUB_MODE_AUTO_PRECLAND,
PX4_CUSTOM_SUB_MODE_AUTO_VTOL_TAKEOFF
};
}

二、云台

Comand: VEHICLE_CMD_DO_GIMBAL_MANAGER_PITCHYAW  =  1000
Param1:pitch
Param2:yaw
Param3:time
Param5:mode

mode
{
GIMBAL_ROTATION_MODE_RELATIVE = 0,
GIMBAL_ROTATION_MODE_ABSOLUTE = 1,
GIMBAL_ROTATION_MODE_SPEED    = 2,
GIMBAL_ROTATION_MODE_RESET    = 3,
}

三、相机

2.1 开始拍照
Comand: MAV_CMD_IMAGE_START_CAPTURE  =  2000

2.2 开始录像
Comand: MAV_CMD_VIDEO_START_CAPTURE  =  2500

2.3 停止录像
Comand: MAV_CMD_VIDEO_STOP_CAPTURE  =  2501

2.4 设置变焦
Comand: MAV_CMD_SET_CAMERA_ZOOM  =  531
Param2:变焦倍数

2.4 获取相机类型
Comand: MAV_CMD_CUSTOM_GET_CAMETA_TYPE  =  535
响应ACK:
result_param2:相机类型

camera_type
{
DJI_CAMERA_TYPE_UNKNOWN = 0, /*!< Camera type is unknown. */
DJI_CAMERA_TYPE_Z30 = 20, /*!< Camera type is Z30. */
DJI_CAMERA_TYPE_XT2 = 26, /*!< Camera type is XT2. */
DJI_CAMERA_TYPE_PSDK = 31, /*!< Camera type is third party camera based on Payload SDK. */
DJI_CAMERA_TYPE_XTS = 41, /*!< Camera type is XT S. */
DJI_CAMERA_TYPE_H20 = 42, /*!< Camera type is H20. */
DJI_CAMERA_TYPE_H20T = 43, /*!< Camera type is H20T. */
DJI_CAMERA_TYPE_H20N = 61, /*!< Camera type is H20N. */
DJI_CAMERA_TYPE_P1 = 50, /*!< Camera type is P1. */
DJI_CAMERA_TYPE_L1, /*!< Camera type is L1. */
DJI_CAMERA_TYPE_M30, /*!< Camera type is M30. */
DJI_CAMERA_TYPE_M30T, /*!< Camera type is M30T. */
DJI_CAMERA_TYPE_M3E, /*!< Camera type is M3E. */
DJI_CAMERA_TYPE_M3T, /*!< Camera type is M3T. */

}

四、视频流

设置流地址
MAVLINK_MSG_ID_STRING_COMMAND_LONG 173
Comand:VIDEO_STREAM_RTMP_MODE  =  0
Param1:rtmp_url

设置码率
Command: MAV_CMD_SET_VIDEOSTREAM_BITRATE = 2506
Param1:bitrate


设置分辨率
Command: MAV_CMD_CUSTOM_SET_VIDEOSTREAM_RESOLUTION = 2507
Param1:resolution

切换FPV视频流
Command:MAV_CMD_CUSTOM_SWITCH_FPV_CAMERA  412

切换主相机视频流
Command:MAV_CMD_CUSTOM_SWITCH_MAIN_CAMERA  413

切换相机源
Command:MAV_CMD_CUSTOM_SWITCH_CAMERA_SOURCE = 414
Param1:source
bitrate
{
VIDEOSTREAM_BITRATE_1Mbps = 1,  
VIDEOSTREAM_BITRATE_2Mbps = 2,
VIDEOSTREAM_BITRATE_3Mbps = 3,
VIDEOSTREAM_BITRATE_4Mbps = 4,
VIDEOSTREAM_BITRATE_5Mbps = 5,
VIDEOSTREAM_BITRATE_6Mbps = 6,  
VIDEOSTREAM_BITRATE_7Mbps = 7,  
VIDEOSTREAM_BITRATE_8Mbps = 8,  
}


resolution
{
VIDEOSTREAM_RESOLUTION_1080P = 0, /* videostream switch to 1080p(1920x1028). | */
VIDEOSTREAM_RESOLUTION_720P = 1, /* videostream switch to 720p(1280x720). | */
VIDEOSTREAM_RESOLUTION_VGA = 2, /* videostream switch to VGA(640x480). | */

}

source
{
DJI_LIVEVIEW_CAMERA_SOURCE_DEFAULT = 0,
DJI_LIVEVIEW_CAMERA_SOURCE_H20_WIDE = 1,
DJI_LIVEVIEW_CAMERA_SOURCE_H20_ZOOM = 2,
DJI_LIVEVIEW_CAMERA_SOURCE_H20T_WIDE = 1,
DJI_LIVEVIEW_CAMERA_SOURCE_H20T_ZOOM = 2,
DJI_LIVEVIEW_CAMERA_SOURCE_H20T_IR = 3,
DJI_LIVEVIEW_CAMERA_SOURCE_H20N_WIDE = 1,
DJI_LIVEVIEW_CAMERA_SOURCE_H20N_ZOOM = 2,
DJI_LIVEVIEW_CAMERA_SOURCE_H20N_IR = 3,
DJI_LIVEVIEW_CAMERA_SOURCE_M30_ZOOM = 1,
DJI_LIVEVIEW_CAMERA_SOURCE_M30_WIDE = 2,
DJI_LIVEVIEW_CAMERA_SOURCE_M30T_ZOOM = 1,
DJI_LIVEVIEW_CAMERA_SOURCE_M30T_WIDE = 2,
DJI_LIVEVIEW_CAMERA_SOURCE_M30T_IR = 3,
DJI_LIVEVIEW_CAMERA_SOURCE_M3E_VIS = 1,
DJI_LIVEVIEW_CAMERA_SOURCE_M3T_VIS = 1,
DJI_LIVEVIEW_CAMERA_SOURCE_M3T_IR = 2,

}

五、通用

获取飞机序列号
Comand: MAV_CMD_CUSTOM_GET_AIRCRAFT_SERIAL  =  502
响应ACK:
string_param1:序列号








六、状态信息
6.1 单个电池的状态信息
MAVLINK_MSG_ID_SINGLE_BATTERY_STATUS   169

6.2 心跳包
MAVLINK_MSG_ID_HEARTBEAT   0

6.3 飞行器的姿态信息
MAVLINK_MSG_ID_ATTITUDE 30

6.4 系统的状态信息
MAVLINK_MSG_ID_SYS_STATUS 1

6.5 全球位置信息
MAVLINK_MSG_ID_GLOBAL_POSITION_INT 33

6.6 拓展系统功能
MAVLINK_MSG_ID_EXTENDED_SYS_STATE 245

6.7 Home点信息
MAVLINK_MSG_ID_HOME_POSITION 242

6.8 云台信息
MAVLINK_MSG_ID_GIMBAL_STATUS 170

6.9 LTE信息
MAVLINK_MSG_ID_LTE_STATUS 171

