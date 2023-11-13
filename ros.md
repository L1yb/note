## terminal

sudo apt install terminator

水平拆分终端– `Ctrl+Shift+0`

垂直拆分终端– `Ctrl+Shift+e

关闭终端 Ctrl+Shift+w

切换窗口 alt + 上下左右

## 编译

catkin_make

source ~/catkin_ws/devel/setup.bash 

source /opt/ros/noetic/setup.bash（系统编译好的软件包）

写入初始化文件

gedit ~/.bashrc 



将文件添加入工作空间

catkin_ws/**src** 

安装插件 

1. ros
2. cmake



快捷编译建

crtl+shift+b
	设置       "group": {"kind":"build","isDefault": true},
			

## 新建包 

在src目录下：

catkin_create_pkg ssr_pkg rospy roscpp std_msgs



## 创建节点

在包的src目录下创建  name_node.cpp

在CMakeList.txt 的找到Build中的

\# add_executable(**${PROJECT_NAME}**_node src/**ssr_pkg**__node.cpp)

复制到文件末尾，修改为：

实例：add_executable(chao_node src/chao_node.cpp)



## 主要通讯方式



![image-20230715185723992](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20230715185723992.png)



1. 话题Topic是节点间进行**持续通讯**的一种形式
2. 话题通讯的两个节点通过**话题的名称**建立起话题通讯连接
3. 话题中的通讯数据叫做消息 Message
4. 消息通常会按照一定的频率持续不断地发送，以保证消息数据的实时性
5. 消息的**发送方**叫做话题的发布者 Publisher
6. 消息的**接收方**叫做话题的订阅者Subscriber



外延补充

1. 一个ROS网络中，可以存在**多个**话题
2. 一个话题可以有**多个**发布者，也可以有**多个**订阅者 
3. 一个节点可以对**多个**话题进行订阅，也可以发布**多个**话题
4. 不同的传感器消息通常会拥有各自独立的话题名称，每个话题只能由一个发布者
5. 机器人速度指令话题通常由多个发布者，但是**同一时间**只能由**一个**发言人

 

## 发布者节点

1. 确定话题名称和消息类型

2. 在代码中include消息类型的头文件

3. 在main函数中通过NodeHandler**大管家**发布一个话题并得到消息发送对象

4. 生成要发布的消息包并进行发送数据的赋值

5. 调用消息发送对象的publish（）函数将消息发布到话题中

   =================================================

>**######定义一个publisher   ，然后调用publish（）函数**
>
>创建的发布者节点需要用过大管家NodeHandle来管理，也就是初始化！
>
>while循环发送消息，可以设置延时

========================================================

```cpp
    ros::init(argc, argv, "chao_node");
    ros::NodeHandle nh;
    ros::Publisher pub = nh.advertise<std_msgs::String>("text_node_publish_chao",10);

    ros::Rate loop_rate(10);

    while(ros::ok()) {
        cout << " running" << endl;
        std_msgs::String msg;
        msg.data = "text content_chao";
        pub.publish(msg);
        loop_rate.sleep();

    }
```

add_executable(chao_node src/chao_node.cpp)

target_link_libraries(chao_node

 ${catkin_LIBRARIES}

)





常用工具

rostopic list  列出活跃着的话题

rostopic echo 话题名称      

显示话题中发送消息包的内容

rostopic hz 话题名称

统计发送的频率



## 话题的订阅

1. 确定话题名称和消息类型

2. 在代码文件中include消息类型的头文件

3. 在main函数中通过NodeHandler大管家订阅一个话题并设置消息接收回调函数

4. 定义一个回调函数，对接收到的消息包进行处理

5. main函数中需要执行ros::spinOnce()，让回调函数能够响应接收到的消息包

   =================================================

   >**######定义一个Subscriber节点 **
   >
   >初始化订阅者节点，需要  **##话题名称##**  ， 长度  ，最重要的是  **##回调函数##**
   >
   >每接受一下消息，就会调用一次回调函数，回调函数要在主函数外定义，参数是接受的消息（message）的数据类型，

   ========================================================



```cpp
    setlocale(LC_ALL,"zh_CN.UTF-8");
    ros::init(argc, argv, "ma_node");

    ros::NodeHandle nh;
    ros::Subscriber sub = nh.subscribe("text_node_publish_chao",10,chao_callback);
    ros::Subscriber sub2 = nh.subscribe("text_node_publish_yao",10,yao_callback);
   
    while(ros::ok()) {
        ros::spinOnce();
    }

```

add_executable(ma_node src/ma_node.cpp)

target_link_libraries(ma_node

 ${catkin_LIBRARIES}

)



常用工具

rqt_graft

图形化显示当前系统的活跃的节点以及节点间话题通讯关系



## launch文件启动多个节点

1. 使用launch文件，可以通过roslaunch指令一次启动多个节点
2. 在launch文件中，为节点添加output = "screen"属性，可以让节点信息输出在终端中。
3. 在launch文件中，为节点添加 launch-prefix = "gnome-terminal -e"属性，可以让节点单独运行在一个独立终端中





## 机器人运动控制

实现思路

1. 构建一个软件包，包名叫 vel_pkg
2. 在软件包中创建一个节点，节点名叫 vel_node
3. 在节点中，向ros大管家NodeHandle申请发布话题 /cmd_vel,并拿到发布对象vel_pub 
4. 构建一个geometry_msgs/Twist类型的消息包vel_msgs，用来承载要发送的速度值
5. 开启一个while循环，不停的使用vel_pub对象发送消息包vel_msg





 catkin_create_pkg vel_pkg roscpp rospy geometry_msgs

```cpp
    //在软件包中创建一个节点，节点名叫 vel_node
    ros::init (argc, argv, "vel_node");
    //在节点中，向ros大管家NodeHandle申请发布话题 /cmd_vel,并拿到发布对象vel_pub
    ros::NodeHandle nh;
    ros::Publisher vel_pub = nh.advertise<geometry_msgs::Twist>("/cmd_vel",10); 
    //构建一个geometry_msgs/Twist类型的消息包vel_msgs，用来承载要发送的速度值
    geometry_msgs::Twist vel_msg;
    vel_msg.linear.x = 0;
    vel_msg.linear.y = 0;
    vel_msg.linear.z = 0;
    vel_msg.angular.x = 0;
    vel_msg.angular.y = 0;
    vel_msg.angular.z = 0.5;
    //开启一个while循环，不停的使用vel_pub对象发送消息包vel_msg
    ros::Rate r(10);
    while(ros::ok()) {
        vel_pub.publish(vel_msg);
        r.sleep();
    }
```



add_executable(vel_node src/vel_node.cpp)

add_dependencies(vel_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

target_link_libraries(vel_node

 ${catkin_LIBRARIES}

)





## 获取激光雷达数据

1. 构建一个新的软件包，名叫lidar_pkg
2. 在软件包中新建一个节点，节点名叫lidar_node
3. 在节点中，向大管家申请订阅节点话题/sacn，并设置回调函数LidarCallback（）
4. 构建回调函数，用来接收和处理雷达数据
5. 调用ROS_INFO显示雷达检测到的前方障碍物的距离

```cpp
void  LidarCallback(const sensor_msgs::LaserScan msg) {
    float fMidDist = msg.ranges[180];
    ROS_INFO("前方测距 %f 米", fMidDist);
}
int main(int argc, char  *argv[])
{
    setlocale(LC_ALL,"");

    ros::init(argc, argv, "lidar_node");

    ros::NodeHandle nh;
    ros::Subscriber lidar_sub = nh.subscribe("/scan", 10, &LidarCallback);

    ros::spin();


    /* code */
    return 0;
}

```

add_executable(lidar_node src/lidar_node.cpp)

target_link_libraries(lidar_node

 ${catkin_LIBRARIES}

)



## 激光雷达避障

1. 让大管家发布速度控制话题/cmd_vel
2. 构建速度控制消息包 vel_cmd
3. 根据激光雷达的测距数值，实时调整机器人运动速度，避开障碍物





## IMU 惯性测量单元消息包

Header header

geometry_msgs/Quaternion orientation
float64[9] orientation_covariance # Row major about x, y, z axes

geometry_msgs/Vector3 angular_velocity
float64[9] angular_velocity_covariance # Row major about x, y, z axes

geometry_msgs/Vector3 linear_acceleration
float64[9] linear_acceleration_covariance # Row major x, y z



- mu/data_raw(sensor_msgs/Imu)
  - Sensor output grouping accelerometer (linear_acceleration) and gyroscope (angular_velocity) data.
  - 加速度计输出的矢量加速度 和 陀螺仪输出的旋转角速度。
- imu/data(sensor_msgs/Imu)
  - Same as imu/data_raw, with an included quaternion orientation estimate (orientation).
  - 与imu/data_raw相同，包括四元数方向估计（方向）。
- imu/mag(sensor_msgs/MagneticField)
  - Sensor output containing magnetometer data.



### IMU数据获取

1. 构建一个新的软件包，叫imu_pkg
2. 在软件包中创建一个节点，节点名叫imu_node
3. 在节点中，向大管家申请订阅话题/imu/data，并设置回调函数IMUCallback
4. 构建回调函数，用来接收和处理数据
5. 使用tf工具将四元数转换成欧拉角
6. 调用ROS_INFO显示



## IMU航向锁定

1. 让大管家发布速度控制话题/cmd_vel
2. 设定一个目标朝向角，当姿态信息中的朝向叫和目标朝向叫不一致时，控制机器人转向目标朝向叫



```cpp
ros::Publisher vel_pub;
void IMUCallback(sensor_msgs::Imu msg) {
    if(msg.orientation_covariance[0] < 0) 
        return ;
    tf::Quaternion quaternion (
        msg.orientation.x,
        msg.orientation.y,
        msg.orientation.z,
        msg.orientation.w
    );

    double roll, pitch, yaw;
    tf::Matrix3x3(quaternion).getRPY(roll, pitch, yaw);
    roll = roll * 180 / M_PI;
    pitch = pitch * 180 / M_PI;
    yaw = yaw * 180 / M_PI;

    ROS_INFO("滚转= %.0f 俯仰= %.0f 朝向= %.0f", roll, pitch, yaw);

    double target_yaw = 90;
    double diff_yaw = target_yaw - yaw;
    geometry_msgs::Twist vel_cmd ;
    vel_cmd.angular.z = diff_yaw * 0.01;
    vel_pub.publish(vel_cmd);



}

int main(int argc, char  *argv[])
{
    setlocale(LC_ALL,"");
    ros::init(argc, argv, "imu_node");

    ros::NodeHandle nh;
    ros::Subscriber imu_sub = nh.subscribe("/imu/data", 10, IMUCallback);
    vel_pub = nh.advertise<geometry_msgs::Twist>("/cmd_vel", 10);

    ros::spin();

    return 0;
}
```







## 自定义消息包

格式

数据类型  变量名



1. 创建软件包，依赖项message_generation、message_runtime
2. 软件包添加**msg目录**，新建自定义软件包，以**.msg**结尾
3. 在CmakeList.txt中，将新建的**.msg文件**加入**add_message_files()**
4. 在CmakeList.txt中，去掉**generation_message**()注释符号，将依赖的其他消息包名称添加进去
5. 在CmakeList.txt中，将**message_runtime**加**catkin_package**的**CATKIN_DEPENDS**
6. 在**package**.xml中,将**message_generation**、**message_runtime**加入<**build_depend**>和<**exec_depend**> 
7. 编译软件包，生成自定义消息类型



查看是否生成成功：rosmsg show qq_msgs/Carry 

### 使用自定义消息包

1. 在节点代码中，先include新消息类型的头文件
2. 在发布或订阅话题的时候，将话题中的消息类型设置为新的消息类型
3. 按照新的消息类型，对消息包进行赋值发送或读取解析
4. 在CMakeList.txt文件中的find_package中，添加新消息包名称作为依赖项
5. 在节点的编译规则中，添加一条add_dependencies,将新消息软件包名称_gennerate_messages_cpp作为依赖项
6. 在package.xml中，将新消息包添加到<build_depend>和<exec_depend>中去
7. 编译



## 发布自定义地图

1. 构建一个软件包map_pkg ，依赖项加上nav_msgs
2. 在map_pkg里创建一个节点map_pub_node
3. 在节点中发布话题/map，消息类型为**nav_msgs::OccupancyGrid**
4. 构建一个nav_msgs::OccupancyGrid地图消息包，并对其赋值
5. 将地图消息包发送到话题/map
6. 编译运行节点
7. 启动rviz ，订阅话题/map显示地图













