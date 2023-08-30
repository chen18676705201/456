# ICREST_SDK_接口文档

2022-06-27
----

* 视频推流
* 



Video Streaming

头文件 :   #include "encode.hpp"

接口介绍：
void *consumer_appsink(void *arg)
功能：解码器线程
参数：
arg：传入的参数


void *consumer_rtmpsink(void *arg)
功能：编码rtmp推流线程
参数：
arg：传入的参数

void *consumer_udpsink(void *arg);
功能：编码udp拉流线程
参数：
arg：传入的参数

void liveView(uint8_t* buf264, int bufLen, void* userData)
功能：接收到新的 h264 帧时在回调线程中调用的回调函数
参数：
buf264：接收H264数据的缓冲区
bufLen：H264数据的长度
userData：用户可以在回调函数内部操作的空指针

void getImageInterface(T_Image *image)
功能：获取RGB数据
参数：
image：解码得到的YUV数据

void RegisterRgbImageCallback(ImageCallback imageCallback)
功能：注册获取RGB数据回调函数
参数：
imageCallback：回调函数
ImageCallback回调函数
typedef void (*ImageCallback)(T_Image *image);

void liveViewRtmp(uint8_t* buf, int bufLen, void* userData)
功能：将流推到流媒体服务器
参数：
buf：一帧YUV数据
bufLen：YUV数据的长度
userData：用户可以在回调函数内部操作的空指针

void RestartGstPipelineForReset()
功能：重启解码器

void RestartGstRtmpPipeline()
功能：重启编码推流

void setRtmpAddress(char *address)
功能：设置rtmp地址
参数：
address：RTMP地址

void setResolution(int width,int height)
功能：设置推流分辨率
参数：
width：推流的宽度
height：推流的高度
(width, height)的三种分辨率
(1920, 1440)
(1280, 720)
(640, 480)

void setControlRate(int controlRate)
功能：设置码率的模式
参数：
controlRate：码率模式
码率模式
1: 自动码率      
2: 固定码率

void setQualityLevel(int qualityLevel)
功能：设置图像质量等级
参数：
qualityLevel：图像质量等级
图像质量等级
0: UltraFastPreset for high perf
1: FastPreset
2: MediumPreset
3: SlowPreset

void setBitrate(int bitrate)
功能：设置码率
参数：
bitrate：设置的码率
码率
Range: 0 - 4294967295 Default: 2000000

void setVbvSize(int vbvSize)
功能：设置虚拟缓冲区的大小
参数：
vbvSize：缓冲区大小
缓冲区大小范围
Range: 0 - 30 Default: 10


void set_m_IsGetImageTaskLive(bool is_live)
功能：设置开启或或停止获取帧数据线程运行
参数：
is_live：是否启动的标志


/*
*实现推流
*/
cn::TheadOfImage imageDisplayThead;
PayloadIndexType m_payloadIndex = PAYLOAD_INDEX_0;
CameraModule::WorkMode m_workMode = CameraModule::WORK_MODE_UNKNOWN;

setRtmpAddress("rtmp://push.live.geoai.com/live/inesttest0427?sign=1666597877592-cb2d3787ec8664d2c7403262901ba474 live=1"); // 设置推流地址
setResolution(1280, 720);    // 设置分辨率
setControlRate(2);          // 设置固定分辨率模式
setQualityLevel(0);          // 设置图像质量
setBitrate(2 * 1e6);         // 设置码率为2M
setVbvSize(10);             // 设置虚拟缓冲区

RegisterRgbImageCallback(getImageInterface); // 注册获取RGB帧数据

checkCameraVersion(m_payloadIndex); // 检测相机挂载号
if(m_payloadIndex != PAYLOAD_INDEX_CNT)
{
cameraStreamInit(m_payloadIndex, m_workMode); // 初始化相机和获取H264流
}

imageDisplayThead.startShowImages();  // 对RGB帧数据进行YOLO识别， 将识别结果的RGB帧数据转换为YUV数据，调用liveViewRtmp接口进行推流

if (rc2 = pthread_create(&cid, NULL, consumer_appsink, NULL))   // 创建解码器线程
cout << "consumer thread creation failed: " << rc2 << endl;


if (rc2 = pthread_create(&rtmp_cid, NULL, consumer_rtmpsink, NULL))  // 创建启动编码rtmp推流线程
cout << "consumer thread creation failed: " << rc2 << endl;

OsdkOsal_TaskCreate(&m_getImageTaskHandle, GetNewFrameTask, 4096, NULL);  // 获取解码后的帧数据












CN_IMSHOWTHREAD

头文件 :   #include "flight_control.hpp"

接口介绍：
int TheadOfImage::addImage(String windowName, Mat image, int width, int height, int idx)
功能：添加RGB帧数据到队列中
参数：
windowName：显示的窗口名字
image：Mat图像
width：宽
height：高
idx：写入的位置，可以用于替换之前的图像。默认添加一个新的图像
返回值：当前的图像的索引

void TheadOfImage::showImages(void)
功能：处理RGB帧数据的线程












Flight Control

头文件 :   #include "flight_control.hpp"

接口介绍：
ErrorCode::ErrorCodeType openJoystickCtrl()
功能：获取虚拟摇杆控制权
返回值：ErrorCode::ErrorCodeType error code

ErrorCode::ErrorCodeType releaseJoystickCtrl()
功能：释放虚拟摇杆控制权
返回值：ErrorCode::ErrorCodeType error code


ErrorCode::ErrorCodeType takeOffCtrl()
功能：起飞
返回值：0或-1


ErrorCode::ErrorCodeType landingCtrl(int ensure)
功能：降落或取消降落
参数：
ensure：是否启动降落
返回值：0或-1


ErrorCode::ErrorCodeType goHomeCtrl(int ensure)
功能：返航或取消返航
参数：
ensure：是否启动返航
返回值：0或-1

void joystickCtrl(float pitch_angle_cd, float roll_angle_cd, float throttle_in, float yaw_rate_cd)
功能：虚拟摇杆控制
参数：
pitch_angle_cd：【俯仰】杆，用于控制无人机往机头方向前进/后退，正数为前进，负数为后退。使用虚拟摇杆控制时，最大速度为 15m/s，最小速度为-15m/s。
roll_angle_cd：【横滚】杆，用于控制无人机相对机头方向向左平移/向右平移，正数为向右平移，负数为向左平移。使用虚拟摇杆控制时，最大速度为 15m/s，最小速度为-15m/s。
throttle_in：【油门】杆，用于控制无人机升降，正数为上升，负数为下降。使用虚拟摇杆控制时，最大垂直速度为 4m/s。最小垂直速度为-4m/s。
yaw_rate_cd：【偏航】杆，用于控制无人机机头旋转，正数为顺时针，负数为逆时针。使用虚拟摇杆控制时，最大速度为 100°/s，最小速度为-100°/s。

/*
*实现飞行控制
*/
openJoystickCtrl(); // 打开虚拟摇杆
takeOffCtrl(); // 起飞
joystickCtrl(2, 0, 0, 0); // 设置速度为2， 向前飞行
joystickCtrl(0, 2, 0, 0); // 设置速度为2， 向右飞行
joystickCtrl(0, 0, 2, 0); // 设置速度为2， 向上飞行
joystickCtrl(0, 0, 0, 10); // 设置10°， 顺时针旋转
releaseJoystickCtrl(); // 释放虚拟摇杆







Gimbal Control
头文件 :   #include "gimbal_control.hpp"

接口介绍：

ErrorCode::ErrorCodeType GimbalManagerSyncInit()
功能：云台控制初始化
返回值： ErrorCode::ErrorCodeType  error code


ErrorCode::ErrorCodeType GimbalManagerSyncCtrl(int mode, float pitch, float yaw, float time=0.5f)
功能：云台绝对角度和相对角度控制
参数：
mode：0 - relative；1 - absolute
pitch：俯仰角度
yaw：横滚角度
time：完成的时间
返回值： ErrorCode::ErrorCodeType  error code

ErrorCode::ErrorCodeType GimbalManagerResetSyncControl()
功能：云台回中
返回值： ErrorCode::ErrorCodeType  error code

/*
*实现云台控制
*/
GimbalManagerSyncInit(); // 云台初始化
GimbalManagerSyncCtrl(0, 20, 0); // 相对模式，俯仰轴向上20°
GimbalManagerSyncCtrl(1, 20, 0); // 绝对模式，俯仰轴移动到20°




Camera Control

头文件 :   #include "camera_control.hpp"

接口介绍：
ErrorCode::ErrorCodeType CameraManagerSyncInit()
功能：相机控制初始化
返回值： ErrorCode::ErrorCodeType  error code

ErrorCode::ErrorCodeType startShootSinglePhoto(PayloadIndexType index)
功能：开始拍照
参数：
index：负载挂位
返回值： ErrorCode::ErrorCodeType  error code

ErrorCode::ErrorCodeType startRecordVideo(PayloadIndexType index)
功能：开始录像
参数：
index：负载挂位
返回值： ErrorCode::ErrorCodeType  error code

ErrorCode::ErrorCodeType stopRecordVideo(PayloadIndexType index)
功能：停止录像
参数：
index：负载挂位
返回值： ErrorCode::ErrorCodeType  error code


ErrorCode::ErrorCodeType setCameraZoom(PayloadIndexType index, float factor)
功能：相机变焦
参数：
index：负载挂位
factor：变焦的倍数
返回值： ErrorCode::ErrorCodeType  error code

ErrorCode::ErrorCodeType setCameraFocusPoint(PayloadIndexType index, float x, float y)
功能：相机聚焦
参数：
index：负载挂位
x：聚焦的位置（0-1.0）
y：聚焦的位置（0-1.0）
返回值： ErrorCode::ErrorCodeType  error code

ErrorCode::ErrorCodeType setCameraType(int type)
功能：设置相机类型
参数：
type：相机类型
返回值： ErrorCode::ErrorCodeType  error code
相机类型枚举
OSDK_CAMERA_SOURCE_DEF = 0,         // 默认当前类型
OSDK_CAMERA_SOURCE_H20_WIDE = 1,    // H20广角
OSDK_CAMERA_SOURCE_H20T_WIDE = 1,   // H20T广角
OSDK_CAMERA_SOURCE_H20_ZOOM = 2,    // H20变焦
OSDK_CAMERA_SOURCE_H20T_ZOOM = 2,   // H20T变焦
OSDK_CAMERA_SOURCE_H20T_IR = 3      // H20T红外

ErrorCode::ErrorCodeType getCameraMode(CameraModule::WorkMode &cameraMode)
功能：获取相机模式
参数：
cameraMode：相机模式
返回值： ErrorCode::ErrorCodeType  error code
相机模式枚举
CAMERA_WORKMODE_SHOOT_PHOTO = 0,    // 拍照
CAMERA_WORKMODE_RECORD_VIDEO = 1,   // 录像
CAMERA_WORKMODE_PLAYBACK = 7,       // 回放
CAMERA_WORKMODE_UNKNOWN = 0xFF,

std::string getCameraName()
功能：获取相机名称
返回值： 相机名称

void setVideoStreamResolution(int solutionMode)
功能：设置视频流的分辨率
参数：
solutionMode：视频流分辨率枚举
视频流分辨率枚举
VIDEOSTREAM_RESOLUTION_1080P = 0, /* videostream switch to 1080p(1920x1028). | */
VIDEOSTREAM_RESOLUTION_720P = 1, /* videostream switch to 720p(1280x720). | */
VIDEOSTREAM_RESOLUTION_VGA = 2, /* videostream switch to VGA(640x480). | */

int getVideoStreamResolution()
功能：获取视频流的分辨率
返回值：获取视频流的分辨率
视频流分辨率枚举
VIDEOSTREAM_RESOLUTION_1080P = 0, /* videostream switch to 1080p(1920x1028). | */
VIDEOSTREAM_RESOLUTION_720P = 1, /* videostream switch to 720p(1280x720). | */
VIDEOSTREAM_RESOLUTION_VGA = 2, /* videostream switch to VGA(640x480). | */

void setVideoStreamBitrate(int bitrate)
功能：设置视频流的码率
参数：
bitrate：码率的枚举
码率的枚举
VIDEOSTREAM_BITRATE_1Mbps = 1, /* videostream bitrate switch to 1Mbps. | */
VIDEOSTREAM_BITRATE_2Mbps = 2, /* videostream bitrate switch to 2Mbps. | */
VIDEOSTREAM_BITRATE_3Mbps = 3, /* videostream bitrate switch to 3Mbps. | */
VIDEOSTREAM_BITRATE_4Mbps = 4, /* videostream bitrate switch to 4Mbps. | */
VIDEOSTREAM_BITRATE_5Mbps = 5, /* videostream bitrate switch to 5Mbps. | */
VIDEOSTREAM_BITRATE_6Mbps = 6, /* videostream bitrate switch to 6Mbps. | */
VIDEOSTREAM_BITRATE_7Mbps = 7, /* videostream bitrate switch to 7Mbps. | */
VIDEOSTREAM_BITRATE_8Mbps = 8, /* videostream bitrate switch to 8Mbps. | */

int getVideoStreamBitrate()
功能：获取视频流的码率
返回值：获取视频流的码率
码率的枚举
VIDEOSTREAM_BITRATE_1Mbps = 1, /* videostream bitrate switch to 1Mbps. | */
VIDEOSTREAM_BITRATE_2Mbps = 2, /* videostream bitrate switch to 2Mbps. | */
VIDEOSTREAM_BITRATE_3Mbps = 3, /* videostream bitrate switch to 3Mbps. | */
VIDEOSTREAM_BITRATE_4Mbps = 4, /* videostream bitrate switch to 4Mbps. | */
VIDEOSTREAM_BITRATE_5Mbps = 5, /* videostream bitrate switch to 5Mbps. | */
VIDEOSTREAM_BITRATE_6Mbps = 6, /* videostream bitrate switch to 6Mbps. | */
VIDEOSTREAM_BITRATE_7Mbps = 7, /* videostream bitrate switch to 7Mbps. | */
VIDEOSTREAM_BITRATE_8Mbps = 8, /* videostream bitrate switch to 8Mbps. | */

/*
*实现拍照，录像
*/
CameraManagerSyncInit();
startShootSinglePhoto(0); // 主挂载开始拍照
startRecordVideo(0); // 主挂载开始录像
stopRecordVideo(0); // 主挂载停止录像










WAYPOINT CONTROL

头文件 :   #include "waypoint_v2.hpp"

接口介绍：
void WaypointV2MissionSample(Vehicle *vehicle)
功能：航点任务的注册回调函数

ErrorCode::ErrorCodeType uploadWaypointMission(int timeout)
功能：上传航点任务
参数：
timeout：超时时间
返回值： ErrorCode::ErrorCodeType  error code

ErrorCode::ErrorCodeType uploadWapointActions( int timeout)
功能：上传航点动作
参数：
timeout：超时时间
返回值： ErrorCode::ErrorCodeType  error code

ErrorCode::ErrorCodeType uploadWapointActions( int timeout)
功能：上传航点动作
参数：
timeout：超时时间
返回值： ErrorCode::ErrorCodeType  error code

ErrorCode::ErrorCodeType downloadWayPoint(std::vector<WaypointV2> &mission)
功能：下载航点动作
参数：
mission：航点任务的集合
返回值： ErrorCode::ErrorCodeType  error code

ErrorCode::ErrorCodeType startWayPoint()
功能：启动航点任务
返回值： ErrorCode::ErrorCodeType  error code

ErrorCode::ErrorCodeType pauseWayPoint()
功能：暂停航点任务
返回值： ErrorCode::ErrorCodeType  error code

ErrorCode::ErrorCodeType resumeWayPoint()
功能：继续航点任务
返回值： ErrorCode::ErrorCodeType  error code

ErrorCode::ErrorCodeType stopWayPoint()
功能：停止航点任务
返回值： ErrorCode::ErrorCodeType  error code

ErrorCode::ErrorCodeType stopWayPoint()
功能：停止航点任务
返回值： ErrorCode::ErrorCodeType  error code

ErrorCode::ErrorCodeType waypointV2_init(missionUpload *data)
功能：航点任务初始化设置和上传任务和动作
参数：
data：航点任务的数据集
返回值： ErrorCode::ErrorCodeType  error code
missionUpload结构体
typedef struct missionUpload
{
char missionID[50]; // 航线任务ID
char name[20];        // 航线任务名称
int headingMode;    //  航线航向模式
int gotoFirstWaypointMode; // 航线前往第一个航点的行动模式
int flightPathMode;        // 航线任务路径模式

    int finishAction;        // 航线任务完成的行动模式
    float autoFlightSpeed;  // 航线自动飞行速度
    int relativeAltitude;    // 航线高度
    int useHomeSeaLevelInRtkUnable; // 默认为0
    
    missionData missionList[255]; // 航点任务的列表
    
    int mission_num;
}missionUpload;

typedef struct missionData
{
double wayPointLatitude;  // 航点纬度
double wayPointLongitude; // 航点经度
double wayPointAltitude;    // 航点高度
double wayPointSpeed;    // 航点速度
double heading;            // 机头朝向，当headingMode 为 1 模式时无效
}missionData;


航向模式

值
模式
描述
1
AUTO（自动）
航向始终与飞行方向一致。
2
USING_INITIAL_DIRECTION（使用初始化角度）
固定使用无人机到达第一个航点时的航向。在到达第一个航点前，航向可以随时控制，到达第一个航点后，航向将被锁定。
3
USING_WAYPOINT_HEADING（使用航点航向角）
在两个航点之间飞行时，航向朝向下一个航点的heading值，飞行过程中机头会逐渐从当前航点角度转动至下一个航点角度（边飞边转动）。
4
TOWARD_POINT_OF_INTEREST（围绕兴趣点）
航向一直朝向兴趣点。

前往第一个航点的行动模式列表

值
模式
描述
1
SAFELY（安全）
如果起飞高度低于航点高度，飞机将上升到和第一个航点相同的高度，再前往该航点
2
POINT_TO_POINT（点到点）
从当前点直线前往第一个任务航点

任务路径模式列表

值
模式
描述
1
NORMAL（直线飞行）
航点之间直线移动。
2
CURVED（曲线飞行）
航点处协调转弯形成曲线，转弯角度半径取决于cornerRadiusInMeters参数，具体逻辑参考无人机曲线飞行方法，该模式下无人机到达航点不会停留，也不执行航点动作


任务完成的行动模式列表

值
模式
描述
0
NO_ACTION（无动作）
完成任务后悬停。此时，飞机可由遥控器控制。
1
GO_HOME（自动返航）
任务完成后以给无人机预设的返航高度返航。（注意在距离home点30m以内返航可能直接降落，最有一个航点应合理规划，不同无人机机型有不同的返航高度）
2
AUTO_LAND（自动降落）
无人机在最后一个航点自动降落。

ErrorCode::ErrorCodeType initMissionSetting(missionUpload *data, int timeout)
功能：初始化航点任务设置
参数：
data：航线和航点的数据集
timeout：超时时间
返回值： ErrorCode::ErrorCodeType  error code

void setWaypointV2Defaults(WaypointV2& waypointV2)
功能：将waypointV2设置默认值
参数：
waypointV2：航点任务结构体

std::vector<WaypointV2> generatePolygonWaypoints(missionUpload *data)
功能：将航点数据集生成航点任务容器
参数：
data：航点数据集
返回值：航点任务容器

std::vector<DJIWaypointV2Action> generateWaypointActions(uint16_t actionNum)
功能：生成航点动作容器
参数：
actionNum：航点动作的个数
返回值：航点动作数组

ErrorCode::ErrorCodeType uploadWayPoint(std::vector<mavlink_waypoint_t> waypoint_mission_data)
功能：上传接收到MAVLINK协议中的航点任务和动作
参数：
waypoint_mission_data：Mavlink定义的航点任务数据
返回值： ErrorCode::ErrorCodeType  error code
Mavlink定义的航点任务结构体
typedef struct __mavlink_waypoint_t {
float param1; /*<  param1*/
float param2; /*<  param2*/
float param3; /*<  param3*/
float param4; /*<  param4*/
int32_t x; /*<  local: x position in meters * 1e4, global: latitude in degrees * 10^7*/
int32_t y; /*<  y position: local: x position in meters * 1e4, global: longitude in degrees *10^7*/
float z; /*<  z position: global: altitude in meters (relative or absolute, depending on frame*/
uint16_t seq; /*<  seq*/
uint16_t waypoint_index; /*<  waypoint_index*/
uint16_t command; /*<  command*/
uint8_t speed; /*<  speed*/
} mavlink_waypoint_t;

/*
*实现航点任务飞行
*/
// 清空航点相关的状态     
current_waypoint_index = -1; // 航点的下标设置为-1
waypointFinishFlag = false;
memset(&route_data, 0, sizeof(mavlink_command_long_t)); // 航线的属性
waypoint_data.clear(); // 航点包括动作的队列
waypoint_mission_data.clear(); // 航点任务的队列


// 模拟经纬度， 选择自己地点的经纬度
int simulationLatitude = 231447337; // 23.1447337 * 1e7
int simulationlongitude = 1132913788;// 113.2913788 * 1e7




// 添加航线属性
route_data.param1 = 3; // 航点个数
route_data.param2 = 1; // 机头方向
route_data.param3 = 1; // 上升到第一个航点的高度再飞行
route_data.param4 = 1; // 航点之间直线移动
route_data.param5 = 1; // 完成任务后返航
route_data.param6 = 2; // 航线速度




// 添加航点动作任务
// 第1个航点---一个航点两个动作，waypoint_index为第一个航点下标，seq自增
mavlink_waypoint_t  waypoint_data_one;
memset(&waypoint_data_one, 0, sizeof(mavlink_waypoint_t));
waypoint_data_one.x = simulationLatitude;
waypoint_data_one.y = simulationlongitude;
waypoint_data_one.z = 10 * 1 ;
waypoint_data_one.speed = 10;
waypoint_data_one.command = WP_ACTION_TYPE_GIMBAL_PITCH_YAW; // 控制云台-10
waypoint_data_one.param1 = -10;
waypoint_data_one.seq = 0;
waypoint_data_one.waypoint_index = 0;
waypoint_data.push_back(waypoint_data_one);




memset(&waypoint_data_one, 0, sizeof(mavlink_waypoint_t));
waypoint_data_one.x = simulationLatitude;
waypoint_data_one.y = simulationlongitude;
waypoint_data_one.z = 10 * 1 ;
waypoint_data_one.speed = 10;
waypoint_data_one.command = WP_ACTION_TYPE_SIMPLE_SHOT; // 拍照
waypoint_data_one.seq = 1;
waypoint_data_one.waypoint_index = 0;
waypoint_data.push_back(waypoint_data_one);




// 第2个航点---一个航点三个动作，waypoint_index为第三个航点下标，seq自增
mavlink_waypoint_t  waypoint_data_two;
memset(&waypoint_data_two, 0, sizeof(mavlink_waypoint_t));
waypoint_data_two.x = simulationLatitude;
waypoint_data_two.y = simulationlongitude;
waypoint_data_two.z = 10 * 2 ;
waypoint_data_two.speed = 10;
waypoint_data_two.command =  WP_ACTION_TYPE_VIDEO_START;  // 开始录像
waypoint_data_two.seq = 2;
waypoint_data_two.waypoint_index = 1;
waypoint_data.push_back(waypoint_data_two);




// 第3个航点---一个航点一个动作，waypoint_index为第二个航点下标，seq自增
mavlink_waypoint_t  waypoint_data_three;
memset(&waypoint_data_three, 0, sizeof(mavlink_waypoint_t));
waypoint_data_three.x = simulationLatitude;
waypoint_data_three.y = simulationlongitude;
waypoint_data_three.z = 10 * 3 ;
waypoint_data_three.speed = 10;
waypoint_data_three.command = WP_ACTION_TYPE_VIDEO_STOP; // 停止录像
waypoint_data_three.seq = 3;
waypoint_data_three.waypoint_index = 2;
waypoint_data.push_back(waypoint_data_three);




memset(&waypoint_data_three, 0, sizeof(mavlink_waypoint_t));
waypoint_data_three.x = simulationLatitude;
waypoint_data_three.y = simulationlongitude;
waypoint_data_three.z = 10 * 3 ;
waypoint_data_three.speed = 10;
waypoint_data_three.command = WP_ACTION_TYPE_CRAFT_YAW; // 控制机头转60°
waypoint_data_three.param1 = 60;
waypoint_data_three.seq = 4;
waypoint_data_three.waypoint_index = 2;
waypoint_data.push_back(waypoint_data_three);




memset(&waypoint_data_three, 0, sizeof(mavlink_waypoint_t));
waypoint_data_three.x = simulationLatitude;
waypoint_data_three.y = simulationlongitude;
waypoint_data_three.z = 10 * 3 ;
waypoint_data_three.speed = 10;
waypoint_data_three.command = WP_ACTION_TYPE_STAY; // 悬停10s
waypoint_data_three.param1 = 10000;
waypoint_data_three.seq = 5;
waypoint_data_three.waypoint_index = 2;
waypoint_data.push_back(waypoint_data_three);






// 将所有航点的动作信息和航点区分出来
waypoint_mission_data = processWayPointData(waypoint_data);


// // 上传航点
ErrorCode::ErrorCodeType ret = uploadWayPoint(waypoint_mission_data);


sleep(3);


// 启动航点
startWayPoint();










SUBSCRIBE INFORMATION
头文件 :   #include "sub_information.hpp"

接口介绍：
bool subInformationInit()
功能：订阅信息的初始化
返回值：bool值

void subAircraftInformationTask(void)
功能：订阅飞机的信息

void subBatteryInformationTask(void)
功能：订阅电池的信息

void subGimbalInformationTask(void)
功能：订阅云台的信息

void isNanAircraftCondition(mavlink_aircraft_condition_t *aircraft_condition_msg)
功能：将数据为Nan的数据置为0

bool checkGPSCoordinate(double latitude, double longitude)
功能：检查经纬度是否在合理范围内
返回值：成功或失败


/*
*订阅状态信息
*/


// 订阅信息初始化
subInformationInit();


// 订阅飞机信息
subAircraftInformationTask();


// 订阅电池信息
subBatteryInformationTask();


// 订阅云台信息
subGimbalInformationTask();




HEALTH MANAGEMENT SYSTEM

头文件 :   #include "hmsinfomanage.hpp"

接口介绍：
int  monitorHMSInit()
功能：hms初始化
返回值：0或-1

void sendHMSAlarmInfo()
功能：发送HMS的报警信息

/*
*订阅健康管理系统信息
*/


// HMS初始化
monitorHMSInit();


// 处理HMS报警信息
sendHMSAlarmInfo()











APRILTAG LANDING CONTROL

void apriltag_init()
功能：apriltag初始化

void apriltag_control()
功能：实现精准降落控制
/*
*实现精准降落
*/


void *Control_Task(void *)
{
while(1)
{
apriltag_control();
usleep(20000);  
}
}


// apriltag初始化
apriltag_init()


// 创建精准降落控制线程
T_OsdkTaskHandle TaskHandle;
OsdkOsal_TaskCreate(&TaskHandle, Control_Task, 4096, NULL);











YOLO RECOGNIZE

void yolov5_init()
功能：yolov5初始化

bool get_isRecognized()
功能：是否识别到物体
返回值：true或false

cv::Mat yolov5_process(cv::Mat img)
功能：进行目标识别处理
参数：
img：一帧RGB数据
返回值：返回YOLO处理过的帧数据

void get_recognizeData(char *name, int &x, int &y, int &width, int &height)
功能：获取到识别结果
参数：
name：识别结果的名字
x：物体在图中的 x 坐标
y：物体在图中的 y 坐标
width：物体实际像素宽度
height：物体实际像素高度

/*
*获取到识别结果
*/

// yolov5初始化
yolov5_init();

if(get_isRecognized() != false){
get_recognizeData(name, x, y, width, height);
}









