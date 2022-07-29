# ROS基础与架构

ros的本质：软件中间件，连接linux等操作系统和机器人



## ROS的基础

ROS的本质/作用/存在的意义：软件复用



四位一体：



五大特点：





难点/问题：

- 点对点的设计存在的问题：
  - 各个节点间的逻辑关系复杂，容易出错
  - TCP/UDP通信设计本身有冗余，在ROS中通信时性能影响通信速率。机器人各节点的协同问题（大疆机器人是集成的，主控和各个部分都集成在了一个芯片，而ROS是：树莓派（主控）+雷达模块等点对点设计）



ROS存在的意义/作用



## ROS文件系统

### 1、安装包：二进制文件和源码文件



### 2、Catkin编译系统

#### （1）Catkin本质/概念

本质：Catkin = cmake + make。

定义：Catkin将cmake与make指令做了一个封装，从而完成整个编译过程的工具。

catkin_make编译本质过程：

- cmake + CMakeLists.txt -> Makefile：生成makefiles，放在_ws/build/文件夹中；
- make + Makefile -> (hello.cpp -> hello.o -> hello)：make上一步生成的makefiles文件，编译链接生成可执行文件，放在_ws/devel/文件夹中。

#### （2）实操过程

##### 工作空间创建

- name_ws文件夹创建
- 创建src文件夹
- 进入src文件夹后，开始创建pkg包



##### Package文件的创建：

- catkin_create创建pkg：catkin_create_pkg [pkg] [depends]

  catkin_create_pkg [mybot_description] [roscpp rospy std_msgs]

  创建几个关键文件夹：pkg的包文件夹，包内的src，include，

  创建几个关键文件：CMakeLists.txt，package.xml

  

- 



以package为基本单元，package包的内容：

- XML：固定的格式保存数据，目的是传输数据，而非显示数据。类似通信中的数据包的格式。
- catkin_make.txt



Package相关命令：

- rospack：
- rosdep：
- roscd：可以直接cd到ros的软件包中
- rosls：同上



节点操作：

rosnode命令



重点把握：从pkg到node（master和node），再到launch



##### launch等文件创建（用于启动多个node，即多个pkg包的可执行文件）

launch文件作用：一个机器人运行时，需要启动很多个node，不需要每个节点都进行rosrun，而是采用launch文件，将master和多个node一次性启动。

启动命令格式：roslaunch [pkg_name] [file_name.launch]（pkg_name为包的名称）

**重点理解：**roslaunch命令首先会自动检测系统的roscore有没有运行，即确认节点管理器（master）有没有启动。如果master未启动，那么roslaunch会先启动master，然后再按照launch规则执行多个node启动。（roslaunch就像一个启动工具，能按预先配置一次性启动多个node）。

.launch文件中的内容：

- < node pkg="" name="" type=""/>：pkg为包名，name为自取名字，type为各个包下的可执行文件。
- < arg > 





##### 编译过程：

- 进入src所在的目录
- catkin_make编译：会自动层层递归src下的各级目录，进行编译
- source ：source _ws/devel/setup.bash，加入环境变量（只在当前terminal生效）。可以通过将此加入~/.bashrc系统环境变量，从而随时启动。
- 自动生成build文件夹（编译好的文件）和devel（）文件夹





### 文件系统实践练习

1. 创建功能包，创建launch，启动一只小海龟
2. 分别用C、C++、python三种语言实现“hello，launch”，并完成在catkin中的编译
3. 创建第二个launch文件，启动一个自己的node节点，并使用arg
4. 在之前的小海龟launch中再启动一只小海龟，并给他分组，然后创建新的launch负责控制这两只小海龟，要求其中一只小海龟模仿另一只小海龟



### 文件系统操作命令

ROS的文件系统本质上还是操作系统文件，所以其命令为类似linux的命令。

增删改查和执行：

|      | 作用                                            | 命令                                                         | 传参                |
| ---- | ----------------------------------------------- | ------------------------------------------------------------ | ------------------- |
| 增   | 创建新ROS功能包                                 | catkin_create_pkg [pkg_name] [depend_pkg]                    | [新建包名] [依赖包] |
|      | 安装ROS功能包                                   | sudo apt install xxx                                         | [包名]              |
| 删   | 删除ROS功能包                                   | sudo apt purge xxx                                           | [包名]              |
| 改   | 修改功能包文件                                  | rosed [包名] [文件名]，需要安装vim，示例：rosed turtlesim Color.msg |                     |
| 查   | 列出所有功能包                                  | rospack list                                                 |                     |
|      | 查找功能包，若存在则返回安装路径                | rospack find                                                 |                     |
|      | 进入某功能包                                    | roscd                                                        |                     |
|      | 列出某个包下的文件                              | rosls                                                        |                     |
|      | 搜索某个功能包                                  | apt search xxx                                               |                     |
| 执行 | 启动：ros master，ros参数服务器，rosout日志节点 | roscore，roscore -p xxxx（指定端口号）                       |                     |
|      | 运行ROS节点                                     | rosrun [包名] [可执行文件/节点]                              |                     |
|      | 执行某个包下的launch文件                        | roslaunch [包名] [launch文件名]                              |                     |













## ROS通信架构



### Topic主题

master：对运动的控制、决策计算等。

node就是进程，多个node就是多个进程。

ros分布式结构（多个node）：每个节点的功能比较单一，不要太过复杂。



异步通信的理解：两个node先在master中注册，一旦注册创建连接，则node之间直接通信，不在进行同步处理，所以是异步的。



rostopic命令：



msg及rosmsg命令：





#### Topic练习：



### Service服务

模式：请求-回复

msg可以嵌套在srv中，即一个请求，请求答复的内容可以是msg格式的。







### Topic和Service对比





### 参数服务器和动作库

#### 参数服务器

静态数据，

增删改查



#### 动作库

动作

反馈





## 通信操作命令

与文件操作命令对比：文件操作命令是静态的，操作的是磁盘上的文件。而通信操作命令是动态的，在ROS程序启动后，可以动态获取运行中的节点或参数的相关信息。

### rosnode



|         | 命令            | 作用                 | 传参 |
| ------- | --------------- | -------------------- | ---- |
| rosnode | rosnode ping    | 测试到节点的连接状态 |      |
|         | rosnode list    | 列出活动节点         |      |
|         | rosnode info    | 打印节点信息         |      |
|         | rosnode machine | 列出指定设备上节点   |      |
|         | rosnode kill    | 杀死某个节点         |      |
|         | rosnode cleanup | 清除不可连接的节点   |      |



### rostopic



|          | 命令                                                         | 作用                                                     | 传参 |
| -------- | ------------------------------------------------------------ | -------------------------------------------------------- | ---- |
| rostopic | rostopic list，rostopic list -v                              | 列出当前运行的主题名称                                   |      |
|          | rostopic pub /\[主题名称]\[消息类型]\[消息内容]，<br />示例1：rostopic pub /turtle1/cmd_vel geometry_msgs/Twist "linear: x: 1.0 y: 0.0 z:0.0 angular: x:0.0 y: 0.0 z: 0.0"（只发布一次运行信息）<br />示例2：rostopic pub -r 10 /turtle1/cmd_vel geometry_msgs/Twist "linear: x: 1.0 y: 0.0 z:0.0 angular: x:0.0 y: 0.0 z: 0.0"（以10Hz的频率循环发布信息） | 发布topic                                                |      |
|          | rostopic echo                                                | 获取指定话题当前发布的消息                               |      |
|          | rostopic info                                                | 获取当前话题的相关信息，消息类型、发布者信息、订阅者信息 |      |
|          | rostopic type                                                | 话题的消息类型                                           |      |
|          | rostopic find [消息类型]                                     | 根据消息类型查找话题                                     |      |
|          | rostopic delay                                               | 列出消息头信息                                           |      |
|          | rostopic hz                                                  | 列出消息发布频率                                         |      |
|          | rostopic bw                                                  | 列出消息发布带宽                                         |      |





### rosmsg



|        | 命令                                                         | 作用                               | 传参 |
| ------ | ------------------------------------------------------------ | ---------------------------------- | ---- |
| rosmsg | rosmsg list                                                  | 列出当前ROS中的所有msg             |      |
|        | rosmsg packages                                              | 列出包含消息的所有包               |      |
|        | rosmsg package [包名]                                        | 列出某个包下的所有msg              |      |
|        | rosmsg show [消息名称]<br />示例：rosmsg show turtlesim/Pose | 显示消息的详细信息                 |      |
|        | rosmsg info                                                  | 与rosmsg show相同作用              |      |
|        | rosmsg md5                                                   | 一种校验算法，保证数据传输的一致性 |      |



### rosservice



|            | 命令                                                         | 作用                  | 传参 |
| ---------- | ------------------------------------------------------------ | --------------------- | ---- |
| rosservice | rosservice list                                              | 列出所有活动的service |      |
|            | rosservice args<br />示例：rosservice args /spawn            | 打印服务参数          |      |
|            | rosservice call<br />示例：rosservice call /spawn "x: 1.0 y:2.0 theta: 0.0 name: 'xxx' "，生成一只叫xxx的乌龟 | 调用服务              |      |
|            | rosservice find                                              | 根据消息类型获取话题  |      |
|            | rosservice info                                              | 获取服务话题详情      |      |
|            | rosservice type                                              | 获取消息类型          |      |
|            | rosservice uri                                               | 获取服务器uri         |      |



### rossrv



|        | 命令                                                         | 作用                     | 传参 |
| ------ | ------------------------------------------------------------ | ------------------------ | ---- |
| rossrv | rossrv list                                                  |                          |      |
|        | rossrv packages                                              | 列出包含服务消息的所有包 |      |
|        | rossrv package [包名]                                        | 列出某个包下的所有msg    |      |
|        | rossrv show [消息名称]<br />示例：rossrv show turtlesim/Spawn | 显示消息描述             |      |
|        | rossrv info                                                  | 与rossrv show相同作用    |      |
|        | rossrv md5                                                   | 对service数据使用md5校验 |      |



### rosparam

用于使用YAML编码文件在参数服务器上获取和设置ROS参数。



|          | 命令                                                         | 作用                 | 传参 |
| -------- | ------------------------------------------------------------ | -------------------- | ---- |
| rosparam | rosparam list                                                | 列出所有参数         |      |
|          | rosparam set<br />示例：rosparam set name huluwa             | 设置参数             |      |
|          | rosparam get<br />示例：rosparam get name                    | 获取参数             |      |
|          | rosparam delete<br />示例：rosparam delete name              | 删除参数             |      |
|          | rosparam load<br />示例：rosparam load xxx.yaml，需先编写yaml文件 | 从外部文件加载参数   |      |
|          | rosparam dump<br />示例：rosparam dump yyy.yaml              | 将参数写出到外部文件 |      |





## ROS常用 API

### 初始化：init



```c++
/** @brief ROS初始化函数。
 *
 * 该函数可以解析并使用节点启动时传入的参数(通过参数设置节点名称、命名空间...) 
 *
 * 该函数有多个重载版本，如果使用NodeHandle建议调用该版本。 
 *
 * \param argc 参数个数
 * \param argv 参数列表
 * \param name 节点名称，需要保证其唯一性，不允许包含命名空间
 * \param options 节点启动选项，被封装进了ros::init_options
 *
 */
void init(int &argc, char **argv, const std::string& name, uint32_t options = 0);

```



### 话题：topic

两大API：

- 发布：publish

```c++
/**
* \brief 根据话题生成发布对象
*
* 在 ROS master 注册并返回一个发布者对象，该对象可以发布消息
*
* 使用示例如下:
*   ros::Publisher pub = handle.advertise<std_msgs::Empty>("my_topic", 1);
*
* \param topic 发布消息使用的话题
* \param queue_size 等待发送给订阅者的最大消息数量
* \param latch (optional) 如果为 true,该话题发布的最后一条消息将被保存，并且后期当有订阅者连接时会将该消息发送给订阅者
*
* \return 调用成功时，会返回一个发布对象
*
*/
template <class M>
Publisher advertise(const std::string& topic, uint32_t queue_size, bool latch = false)
```



```c++
/**
* 发布消息          
*/
template <typename M>
void publish(const M& message) const
```



- 订阅：subscribe

```c++
/**
   * \brief 生成某个话题的订阅对象
   *
   * 该函数将根据给定的话题在ROS master 注册，并自动连接相同主题的发布方，每接收到一条消息，都会调用回调
   * 函数，并且传入该消息的共享指针，该消息不能被修改，因为可能其他订阅对象也会使用该消息。
   * 
   * 使用示例如下:

void callback(const std_msgs::Empty::ConstPtr& message)
{
}

ros::Subscriber sub = handle.subscribe("my_topic", 1, callback);

   *
* \param M [template] M 是指消息类型
* \param topic 订阅的话题
* \param queue_size 消息队列长度，超出长度时，头部的消息将被弃用
* \param fp 当订阅到一条消息时，需要执行的回调函数
* \return 调用成功时，返回一个订阅者对象，失败时，返回空对象
* 

void callback(const std_msgs::Empty::ConstPtr& message){...}
ros::NodeHandle nodeHandle;
ros::Subscriber sub = nodeHandle.subscribe("my_topic", 1, callback);
if (sub) // Enter if subscriber is valid
{
...
}

*/
template<class M>
Subscriber subscribe(const std::string& topic, uint32_t queue_size, void(*fp)(const boost::shared_ptr<M const>&), const TransportHints& transport_hints = TransportHints())
```



### 服务：service

两大API：

- 服务端

```c++
/**
* \brief 生成服务端对象
*
* 该函数可以连接到 ROS master，并提供一个具有给定名称的服务对象。
*
* 使用示例如下:
\verbatim
bool callback(std_srvs::Empty& request, std_srvs::Empty& response)
{
return true;
}

ros::ServiceServer service = handle.advertiseService("my_service", callback);
\endverbatim
*
* \param service 服务的主题名称
* \param srv_func 接收到请求时，需要处理请求的回调函数
* \return 请求成功时返回服务对象，否则返回空对象:
\verbatim
bool Foo::callback(std_srvs::Empty& request, std_srvs::Empty& response)
{
return true;
}
ros::NodeHandle nodeHandle;
Foo foo_object;
ros::ServiceServer service = nodeHandle.advertiseService("my_service", callback);
if (service) // Enter if advertised service is valid
{
...
}
\endverbatim

*/
template<class MReq, class MRes>
ServiceServer advertiseService(const std::string& service, bool(*srv_func)(MReq&, MRes&))
```



- 客户端

```c++
/** 
  * @brief 创建一个服务客户端对象
  *
  * 当清除最后一个连接的引用句柄时，连接将被关闭。
  *
  * @param service_name 服务主题名称
  */
 template<class Service>
 ServiceClient serviceClient(const std::string& service_name, bool persistent = false, 
                             const M_string& header_values = M_string())
```



```c++
//请求发送函数：
/**
   * @brief 发送请求
   * 返回值为 bool 类型，true，请求处理成功，false，处理失败。
   */
  template<class Service>
  bool call(Service& service)
```



```c++
//等待服务函数1：
/**
 * ros::service::waitForService("addInts");
 * \brief 等待服务可用，否则一致处于阻塞状态
 * \param service_name 被"等待"的服务的话题名称
 * \param timeout 等待最大时常，默认为 -1，可以永久等待直至节点关闭
 * \return 成功返回 true，否则返回 false。
 */
ROSCPP_DECL bool waitForService(const std::string& service_name, ros::Duration timeout = ros::Duration(-1));
```



```

```



### 回旋函数



### 时间



### 其他











# 机器人建模与运动仿真





## 建模



本质就是一个pkg



urdf功能包建立：模型参数

- 小车模型
- 传感器模型



xacro：urdf的进阶



过程：

- 进入工作空间xxx_ws，进入src文件夹，创建模型pkg：catkin_create_pkg [mybot_description] [roscpp rospy std_msgs]，文件名+依赖包（urdf不需要依赖包，因此不需要后面的参数）
- 新建必要的文件夹：比如launch、urdf、xacro、config（rviz）等
- 新建并编写.urdf或.xacro文件，构建模型
- 模型可视化：
  - rviz可视化：显示机器人时，需要用到launch文件，因为要同时启动好几个node（urdf、rviz等节点）
    - .rviz文件的来源和使用？？？？？？？
  - gazebo可视化：





## 工具

可视化：rqt，rviz

仿真：gazebo

日志：rosbag

跨平台：rosbridge

运动接口：moveit！



## 仿真

运动与控制：node

rviz，gazebo



## 建图、定位与自主导航







# ROS编程

## 过程



### Topic



### Service

- 确定通信架构
- 确定数据格式：新建xxx.srv文件，在其中定义好req数据及格式、res数据及格式（就类似一个struct）；在编译时自动生成xxx.h头文件。
- 新建client文件夹：新建client.cpp文件
  - ros初始化；
  - 句柄节点nh声明；
  - nh的client的实例化：类的声明
  - 数据结构的实例化：即xxx.h中的数据结构体声明，然后对该结构体赋值。
  - 功能函数：
- 新建server文件夹：新建server.cpp文件
  - ros初始化；
  - 句柄节点nh声明；
  - nh的server的实例化：
- 编译



### 参数服务器





## 重点文件理解

CMakeLists.





# 机器人系统（平台）设计



基本组成和结构

架构

学习路线



## 机器人基本结构

眼：传感器

- 单目摄像头：目标检测、监控、视觉测量
- 双目摄像头：测量、定位
- 雷达：激光雷达、毫米波雷达
- 其他：红外摄像头、RGBD（D为distance，距离）、超声波、声呐



手：执行机构

- 关节，link
- 连杆，joint



脚：

- 轮式：前轮转向、后轮驱动
- 履带式/四轮式：四个电机
- 人形机器人：舵机，自由度



小脑：数据的获取和简单处理

- 惯性导航：测量自身位移、速度、加速度、角速度
- 里程计
- GPS：
- 高度计：



大脑：做运算处理

- 树莓派
- NX家族
- FPGA
- 台式机



### 机器人系统集成

通信：机器人内部各部分通信，主要是嵌入式接口技术开发和通信



系统：ROS

从硬件层->硬件驱动层->硬件抽象层->服务层->应用层



算法：

- 运动与控制
- 图像识别与目标追踪
- 地图构建与自主导航
- 语音识别和语音交互



## 机器人系统构建







- 传感系统
  - 内部感知
    - 姿态与平衡：IMU（惯性测量单元）
    - 行程与距离：
  - 外部感知

- 驱动系统：**驱动的本质作用：不同平台/硬件之间怎么互通。**
  - 电源子系统：电压等级转化
  - 电机驱动子系统：电机，电机闭环控制传感器
  - 超声波里程计

- 执行机构

- 控制系统：**分布式**（智能硬件的一个大趋势）
  - PC端：
  - 机器人本地系统：



### 控制系统

开环控制

闭环控制：

- 单闭环
- 多闭环

分层设计控制策略

- 最底层：电机
- 中间层：机器人本体的运动控制
- 最上层：路径规划



控制系统模型：

PID



电机控制：

TIM8：

- 输出也有上拉、下拉等

- 能产生3路互补输出，控制三相电机。





## 学习路线



stm32：

- 裸机+状态机
- FreeRTOS



智能控制：控制和数据捕获。在传统感知基础上加上算法



智能感知：加上数据的处理

- 激光雷达
- 摄像头
- 音频



## 传感器

四大测距：

- 红外
- 超声波
- 激光雷达
- 相机
  - 双目相机：两个摄像头不同焦距
  - 深度相机：



激光雷达内部三角测距原理：

激光雷达立体三角测距：



### 红外



### 激光雷达

激光雷达手册资料找一找？？？



#### 分类

- 毫米波雷达
- 微波雷达
- 激光雷达



#### 原理



#### 协议内容







#### 应用过程：

##### 应用方式1：ROS接口

ROS中的驱动接口（C++实现）：这部分就是调用API，关键参数：

- dev名称：tty/usb...，去外设的文件夹找设备名
- 波特率
- frame_id：名称



特点



应用场景



##### 应用方式2：stm32驱动程序

- uart串口连接激光雷达
- 通过请求和应答的方式，由主机请求激光雷达数据，由激光雷达向主机发送数据，实现主机对数据的采集





### 相机

分类



原理



特点



应用场景



### 超声波









### 电机

#### 分类



#### 组成

- 电机：
- 减速箱：减速，提升输出力矩，动力更强。
- 编码器



#### 硬件接口

M1：IO口置1，正转；IO口置0，反转。（编写驱动程序，做单步测试？？？？？）

M2：PWM输出：控制电机转速（编写驱动程序，做单步测试？？？？？）

C1：编码器数据读取：（编写驱动程序，做单步测试？？？？？）

C2：

VCC：

GND：





### 编码器

分类

- 光栅编码器
- 霍尔编码器：发出脉冲信号。在一定采样周期内，收到的脉冲信号数量，除以轮子转一圈的脉冲数，就得到转动的圈数，再除以采样周期，就是转速。



### IMU

#### 基本概念

定义：惯性测量单元（Inertial Measurement Uint）



作用：使用加速度计和陀螺仪来测量物体三轴姿态角（或角速率）以及加速度的装置。

- 加速度计：检测物体在载体坐标系统独立三轴的加速度信号
- 陀螺仪：检测载体相对于导航坐标系的角速度信号，测量物体在三维空间中的角速度和加速度，并以此解算出物体的姿态。
- IMU在导航中的核心价值无可替代，为了提高其可靠性，还可以为每个单轴配备更多种类的传感器。为保证测量准确性，一般IMU要安装在被测物体的重心上。



原理：



加速度计量程：



陀螺仪量程：



采样频率和数字低通滤波器：



特性：

- 更新频率高，工作频率可以达到 100Hz 以上
- 短时间内的推算精度高，不会有太大的误差





#### 组成

组成：

- 加速度计（Accelerometer）：3轴

- 陀螺仪（Gyroscope）：3轴

- 磁力计：额外3轴，可通过辅助IIC接口连接
- 温度计：
- ADC模块：
- 数字运动处理器（DMP）：







外部接口：

- 辅助IIC：作为master，接传感器，比如磁力计等，形成9轴
- 主要IIC/SPI：作为slave，接微控制器
- ADO：作为slave地址的LSB位，设定IIC从机地址
- MPU-INT：MPU的中断，用于MPU作为从机时，当发生中断，执行向主机发送一些数据功能。（IIC半双工模式，当master正在向slave发送数据，此时若从机采集数据已满，需要向master发送数据，则需要用的中断控制，中断主机的发送）
- 电源：VDD
- VLOGIC：为IIC输出提供逻辑电平



寄存器：







#### 关键功能和参数

FIFO：

- 功能：1024字节的FIFO寄存器，可以决定让什么数据进入，包括加速度计、陀螺仪、温度、外部传感器、FSYNC输入等
- FIFO计数器：跟踪记录存入FIFO的字节数

中断：

- 自由落体中断
- 运动中断
- 静止中断



时钟：



#### 应用

配置过程：

物理接口：

- 电源
- 主要IIC接口配置：



IIC配置：IIC函数服务于IMU的配置，因此需要先配置好IIC

- start：
- 



IMU配置：

- 初始化IIC
- 电源：复位、唤醒
- 设置陀螺仪和加速度计的量程
- 设置数字低通滤波器：
- 设置采样频率
- 设置中断
- 设置IIC主模式/从模式
- 获取MPU的地址，若地址正确，则设置CLKSEL、PLL，开启陀螺仪和加速度计，设置采样频率







## 微控制器（板卡）

运行设备驱动程序

作用：电机控制，传感器数据获取，串口通信。



### Arduino







### STM32F4

驱动程序架构：

motor相关：

- encoder编码器：
- motor电机：
- pid控制：
- chrg电源：



外设：

- gpio
- led



bsp相关：

- sys
- delay
- tim



通信相关：

- uart
- IIC



esp相关：wifi





### ESP8266



- esp作为sta端接入ros所在的局域网
- esp设置tcp连接，作为client连入rosbridge_server





### 基于ROS的嵌入式板







## 微处理器

CPU板和GPU板



运行ROS，实现视频、算法运算等

### Raspberry Pi





## ROS相关

要搞清楚嵌入式端传感器的参数怎么与ROS对接的：传感器->串口->数据的处理->数据的发布

要搞清楚数据的含义和作用：卡尔曼滤波



### rosbridge

rosbridge：wiki官网看用法。

#### 安装：

```c
sudo apt install ros-melodic-rosbridge-suite
//suite包含了其他包
```



#### 原理：

类似CS架构，嵌入式设备（esp8266）为client端，ROS为server端

- 在ubuntu上

  ```c
  rosrun rosbridge_server rosbridge_tcp //在ubuntu上启动rosbrige节点，形成一个server
  ```

- 在嵌入式设备上，通过esp8266，作为sta端，连入ubuntu所在的局域网，使两者在同一局域网。此时esp8266相当于client端

- 在嵌入式设备驱动程序中，通过串口向esp8266发送JSON格式数据（符合ROS格式），则ubuntu上的rosbridge的server就能接收到数据，按格式解析，即相当于订阅了嵌入式设备的topic。



#### 数据格式：

JSON数据格式

```c
{ "op": "publish",
  "topic": "/turtle1/cmd_vel",
  "type": "geometry_msgs/Twist",
  "msg": {"linear": {"x": -4, "y": -2, "z": 0}, "angular": {"x": 0, "y": 1, "z": 0}}
}
```



过程





#### 实践：以小海龟为例



#### 在M4+ESP8266实践完成小车控制



LaserScan格式：



```c
/*stm上通过esp发送数据*/
{ "op": "publish",
  "topic": "/turtle1/cmd_vel",
  "type": "geometry_msgs/Twist",
  "msg": {"linear": {"x": -4, "y": -2, "z": 0}, "angular": {"x": 0, "y": 1, "z": 0}}
}
```







### TF坐标转换

基本变换：

平移和旋转：x y z yaw pitch roll，6个坐标





哪些设备需要变换：

- 激光雷达
- IMU
- 陀螺仪，gyro
- 



### EKF卡尔曼滤波



### slam

建图





#### AMCL：

粒子滤波



### Navigation包





# 算法

## 智能算法

智能点：非模型，没有办法用一个模型表示，非线性、无规律

- 模糊算法

- 专家算法

- 遗传算法

- 神经网络





## 模型算法

具有一定的模型规律

- pid算法





### pid算法

#### 基本概念

定义：从当前状态达到目标状态的闭环控制过程，通过3种参数改变控制值。

P：Proportional，比例（线性）

I：Integral，积分（累加）

D：Derivative，微分（求导数）



#### 作用、实现过程与理解

P：将需要控制的物理量带到目标值附近（调节作用明显）

- 烧水问题：按（目标水温-当前水温）*Kp的值来升温
- 按照（目标值和当前值的差值）*比例系数，求得变化量，用于调节输入、控制输出，能够迅速减少误差：

- 可能出现**稳态误差**：当有外界因素导致部分输入无效时，就会产生稳态误差，导致当前值距离目标值永远相差一定值，比如水温上升（外界散热）、汽车运动（摩擦阻力）等。

I：减小静态情况下的误差

- 烧水+外界散热问题：只用P就会导致只能烧到80℃
- 积分控制：误差的积分（离散情况就是做累加）
- 当达到稳态时，可以通过误差积分项，继续缩小当前值与目标值的距离

D：能够预见量的变化趋势，减小这种惯性，使得物理量的变化速度趋于0。向反方向阻挡的力量

- 刹车问题：
- 烧水问题：



#### 原理、位置式和增量式对比

（1）一般公式：
$$
u(t)=K_p[e(t)+\frac{1}{T_i}\int_{0}^{t}e(t)d_t+T_d\frac{d_e(t)}{d_t}]
$$
——u(t)：控制器输出的控制量（输出）；

——e(t)：error，偏差信号，等于给定量与输出量之差（输入）；

——Kp：比例系数

——TI：积分时间常数

——TD：微分时间常数

（2）离散公式：简化为3个参数


$$
u(k)=K_p[e(k)+\frac{T}{T_i}\sum_{n=0}^{k}e(n)+\frac{T_d}{T}e((k)-e(k-1))]
$$

$$
u(k)=K_pe(k)+\frac{K_pT}{T_i}\sum_{n=0}^{k}e(n)+\frac{K_pT_d}{T}(e(k)-e(k-1))
$$

$$
u(k)=K_pe(k)+K_i\sum_{n=0}^{k}e(n)+K_d(e(k)-e(k-1))
$$



（3）位置式：

- 公式为（2）中的公式

- 误差值error：e(k)=目标值-当前值
- P：本次误差
- I：误差累加
- D：本次误差-上次误差

缺点：

- 积分中e(n)是所有误差的累加值，即当前的输出u(k)**与过去所有的状态都有关系**，导致一旦控制输出出错（当前状态值出现问题），则u(k)的大幅变化会引起系统的大幅变化。不利于稳定控制。
- 当积分项达到饱和时，误差仍然会在积分作用下继续累加，一旦误差开始反向变化，系统需要一定时间从饱和区退出，所以在u(k)达到最大和最小时，要停止积分作用，并且要有积分限幅和输出限幅。

适用：

- 在使用位置式PID时，一般直接使用PD控制
- 适用于执行机构不带积分部件的对象，如舵机、平衡小车的直立、温控系统等



（4）增量式

增量式：增量式PID中不需要累加，控制增量的确定仅与最近3次的采样值有关。即使系统发生问题，其仍能较好的维持之前的状态，不会严重影响系统工作。
$$
Δu(k)=u(k)-u(k-1)\\
=K_p(e(k)-e(k-1))+K_ie(k)+K_d(e(k)-2e(k-1)+e(k-2))
$$

- P：本次误差-上次误差
- I：误差
- D：本次误差-2*上次误差+上上次误差
- 求得的是控制的增量，即需要在上一次的控制量基础上需要增加（或减少）的量

优点：

- 只要使用前后三次测量值的误差，即可求出控制增量
- 不需要积分累加，系统发生问题时，增量式不会严重影响系统的工作



（5）位置式和增量式对比

- 增量式算法不需要做累加，控制量增量的确定仅与最近几次偏差采样值有关，计算误差对控制 量计算的影响较小。而位置式算法要用到过去偏差的累加值，容易产生较大的累加误差。 

- 增量式算法得出的是控制量的增量，例如在阀门控制中，只输出阀门开度的变化部分，误动作 影响小，必要时还可通过逻辑判断限制或禁止本次输出，不会严重影响系统的工作。 而位置式的输出直接对应对象的输出，因此对系统影响较大。

- 增量式PID控制输出的是控制量增量，并无积分作用，因此该方法适用于执行机构带积分部件的对象，如步进电机等，而位置式PID适用于执行机构不带积分部件的对象，如电液伺服阀。

- 在进行PID控制时，位置式PID需要有积分限幅和输出限幅，而增量式PID只需输出限幅

位置式PID优缺点：

- 优点：
  - ①位置式PID是一种非递推式算法，可直接控制执行机构（如平衡小车），u(k)的值和执行机构的实际位置（如小车当前角度）是一一对应的，因此在执行机构不带积分部件的对象中可以很好应用

- 缺点：
  - ①每次输出均与过去的状态有关，计算时要对e(k)进行累加，运算工作量大。

增量式PID优缺点：

- 优点：

  - ①误动作时影响小，必要时可用逻辑判断的方法去掉出错数据。

  - ②手动/自动切换时冲击小，便于实现无扰动切换。当计算机故障时，仍能保持原值。

  - ③算式中不需要累加。控制增量Δu(k)的确定仅与最近3次的采样值有关。

- 缺点：
  - ①积分截断效应大，有稳态误差；
  - ②溢出的影响大。有的被控对象用增量式则不太好；



#### 应用场景

- 将某一个物理量“保持稳定”的场合，如维持平衡、稳定温度、转速等







# 智慧服务系统

项目中的服务器：

- 分布式结构，一个进程拆分为多个
- 底层的接口select等；
- 以服务的形式在一个进程中实现
- 插件动态加载：进程不结束，动态加载某个功能





项目：

- 已学过的：微控制器，感知设备，设备驱动
- 未学过的：
  - 采集智能：数据处理和融合，数据滤波（应用层面的问题，所以需要滤掉异常的数据，从而使用该数据）
  - 控制智能：电机控制
  - ROS：学会怎么用ROS；ROS应用开发，即pkg、添加包、编译等。（机器人集成技术）；ROS的分布式架构，建图、导航、定位三大功能（用别人的pkg）；智能交互（摄像头、语音等）



第一周：

需求分析、架构图、流程图、数据流图



## （一）感知与运动（ROS平台）需求分析

### 1、构建二轮差分运动模型

#### 基本概念

机器人组成：

- 传感系统：
  - 外部传感：激光雷达、相机
  - 内部传感：电机编码器
- 驱动系统：直流减速电机
- 执行机构：底盘、驱动轮、万向轮
- 控制系统：嵌入式微控制器、Raspberry Pi或PC中控

机器人模型主要属性：

- Link：连杆
- Joint：关节
- Collision：碰撞属性
- Inertial：惯性属性



#### 需求分析

仿真模型构建：使用urdf或xacro构建仿真模型，使用gazebo搭建仿真环境（搭建机器人运行的world，包括边界、障碍物等），使用rviz实现机器人模型及仿真的可视化。

机器人实体构建：

- 二轮差分的驱动与执行机构：
  - 两个驱动轮分别由两个直流减速电机驱动，通过控制两个电机不同转速，实现小车的方向控制；
  - 通过控制电机正反转，实现小车前进与后退；
  - 通过电机编码器数据，计算得到小车速度、角度、里程等信息。
- 传感系统：
  - 通过激光雷达扫描环境，构建地图；
  - 通过激光雷达扫描小车行驶路线上的障碍物，实现避障与本地路径规划；
  - 通过相机获取小车外部环境监控视频。
- 控制系统：
  - 嵌入式微控制器STM32F4：小车驱动系统的控制，采集车内传感器信息、控制电机；
  - Raspberry Pi控制系统：运行在小车的ROS系统，下发控制和执行命令；
  - PC控制系统：运行在PC的ROS系统，与Raspberry Pi组成多节点ROS系统，主要实现视频显示、可视化、大量运算等消耗硬件资源的功能。



### 2、闭环控制

#### 基本概念

感知和控制执行形成闭环。根据当前参数与目标参数的差异，实施控制措施，不断缩小当前参数与目标参数的差值，最终达到目标参数。

比如，电机调速：调速实现是一个闭环，每一次循环都会根据当前时速与目标时速的差值，再乘以固定系数，计算出需要输出的PWM值。

比如，温控系统：根据温度传感器的值与需要维持的环境温度值的差值，确定需要加热还是停止加热，从而控制加热系统动作。

#### 需求分析

闭环1：根据电机编码器数据，判断电机转速、正反转等；下发电机转速、正反转命令，根据电机编码器判断下发的命令是否正确执行。从而电机控制局部闭环。

闭环2：激光雷达测距：根据激光雷达发布的topic数据，判断小车距离障碍物距离，做出小车前进或后退（电机正反转）、小车转弯（两轮电机差速）、加速或减速（电机转速）等topic命令下发；根据激光雷达数据，判断下发的电机控制命令是否正确执行。形成小车移动局部闭环。



### 3、地图构建

#### 基本概念

slam：Simultaneous Location and Mapping，即时定位与建图。机器人在未知环境中，从一个未知位置出发，在移动过程中根据位置估计和地图进行自身定位，同时在自身定位的基础上建造增量式地图，以绘制出外部环境的完全地图。（建图、自身定位）

slam算法类别：

- gmapping
- hector_slam
- cartographer
- 。。。

#### 需求分析

采用gmapping功能包中的slam_gmapping节点，通过订阅激光雷达信息，形成地图数据（发布地图数据的topic）。

实现步骤如下：

##### （1）环境感知：

激光雷达：获取激光雷达发布的topic。

##### （2）机器人移动建图：

方式一：手动键盘控制：用teleop_twist_keyboard功能包，实现手动按键控制小车移动，再由激光雷达采集的数据，构建地图。

方式二：用slam的节点和move_base节点实现小车在环境中的自主移动和建图。

##### （3）地图保存：

map_server功能包的两个节点map_saver和map_server实现地图保存和调用。

- map_saver：保存地图。通过订阅(nav_msgs/OccupancyGrid)画图，生成地图文件。
- map_server：地图调用。

地图文件：共两个文件，一个是xxx.pgm，本质是一张图片，即地图；一个是xxx.yaml，保存的是地图的元数据信息，用于描述地图图片，主要参数包括：

- 分辨率（resolution）
- 地图障碍可能性数据：
  - occupied_thresh：表示占用概率大于此阈值的像素被视为完全占用，即障碍物。
  - free_thresh：表示占用率小于此阈值的像素被视为完全空闲，即无障碍物。



### 4、定位

#### 基本概念

定位：推算机器人自身在全局地图中的位置。此处只地图构建完成后，在导航阶段的定位。导航时，机器人按照设定的路线运动，通过定位可以判断机器人的实际轨迹是否符合预期。

AMCL（Adaptive Monte Carlo Localization，自适应蒙特卡洛定位），是用于2D机器人的概率定位系统，实现了自适应蒙特卡洛定位方法，可以根据已有地图使用粒子滤波器推算机器人位置。

#### 需求分析

采用navigation功能包中的amcl节点实现机器人自身定位。

实现步骤：加载地图，启动amcl节点，启动rviz（在rviz上添加PoseArray插件，订阅amcl的PoseArray话题，图形化显示定位概率集合）。



### 5、路径规划

#### 基本概念

路径规划：根据给定的目标点及机器人自身定位信息，自动规划由当前位置前往目标点的行驶路径。

全局路径规划：根据给定的目标点和全局地图实现总体的路径规划，使用Dijkstra或A*算法进行全局路径规划，计算最优路线。

本地路径规划：在实际导航过程中，可能会出现障碍物变化，导致机器人无法按照给定的全局最优路线行驶。本地规划就是使用一定算法来实现障碍物的规避，比选取当前最优路径以尽量符合全局最优路径。

几类地图：

- 静态地图：slam构建的地图。一般无法直接用于导航，无法避免障碍物有变动，或由于机器人惯性、转弯而与障碍物碰撞。
- 代价地图：分为全局代价地图（global_costmap）和本地代价地图（local_costmap），分别用于全局路径规划和本地路径规划。都可由多层地图叠加：
  - 静态地图层（Static Map Layer）：即slam构建的地图；
  - 障碍地图层（Obstacle Map Layer）：由传感器感知的障碍物信息；
  - 膨胀层（Inflation Layer）：在以上两层地图上进行膨胀（向外扩张），避免机器人外壳撞上障碍物（加大余量）。

#### 需求分析

实现方法：采用navigation功能包中的move_base功能包、move_base节点实现。基于动作（action）的路径规划实现，move_base可根据给定的目标点，控制机器人底盘运动至目标位置，同时在运动过程中会连续反馈机器人自身姿态与目标点的状态信息。

move_base：订阅目标点相关action，发布机器人底盘运动控制消息topic，发布机器人动作信息（底盘坐标、操作信息等），从而实现机器人的闭环控制。

move_base实际配置：可配置节点参数，包括：

- 机器人相关：外形尺寸、距离障碍物安全距离、膨胀半径、传感器配置等；
- 全局代价地图：地图坐标系、机器人坐标系、地图更新和发布频率等
- 本地代价地图：里程计坐标系、机器人坐标系、地图更新和发布频率、局部地图宽和高、分辨率等
- 局部规划器（planner）：机器人最大和最小速度限值、加速度阈值等









### 数据流图

#### （1）驱动部分：

编码器->STM32F4：

电机->STM32F4：

STM32F4->电机：



#### （2）ROS上位机部分：

STM32F4->RaPi-ROS：本质是编码器pkg，电机pkg

RaPi-ROS->STM32F4：

LaserPkg：LaserPkg做topic发布，[gmapping绘图。。。]需要订阅Lasertopic？

MotorPkg：做cmd发布，[。。。]需要订阅该cmd？





## （二）实践分解





第二周：

一定要产出demo



第三周：

模块内容搞出来



第四周：

模块对接、联调、展示



机器人驱动：

- M4原理图，M4源码阅读理解，主要驱动程序理解
- M4与ROS信息传递过程：串口->esp8266->rosbridge
  - esp驱动程序编写：
- 驱动程序重写和运行



rosbridge：

- 以小海龟为例，理解实践rosbridge的数据格式、控制原理、控制过程
- 在M4+esp8266上完成控制



ros：

- 调包，用launch启动各部分，能使用
- 理解tf、ekf、nav、slam等几个核心包原理
- 灵活应用
- usb摄像头：
  - usb摄像头通过usb连接至ubuntu
  - 找到usb摄像头的串口设备名称
  - ubuntu中的ros下载usb摄像头的pkg
  - 在ros中调包，打开usb摄像头
- esp8266：
  - 在ubuntu端的ros中，启动rosbridge_server，建立一个服务端，得到服务端ip和端口
  - esp通过usb转ttl连接至ubuntu
  - 在串口调试助手，以AT指令方式，将esp设为sta端，并作为client端连接至ubuntu的rosbridge_server端，实现嵌入式与ros的通信打通





## （）Qt人机交互



# 2022.3.16

M4的wifi测试跑通

机器人系统开发课程



ros小海龟控制的几种方式：

roscore的实际作用

rosrun和roslaunch命令的本质区别和实质，注意理解内涵

命令行参数：JSON格式和文本格式，注意区分



node与topic的关系：通过rqt_graph了解数据流



## rosbridge

过程

- 安装包
- rosbridge作为server，宿主机连接虚拟机的server
- rosbridge数据格式

M4上移植轻量级的JSON，



M4与PC上的ROS打通过程：

- M4与esp8266连接，esp8266作为state端，与ubuntu连接到一个局域网，ping通



## tf变换

原理



## ekf卡尔曼滤波



# 自动驾驶汽车





# 机器人遥控操作





# ROS项目基础

**记住一切皆节点。**

**1、注意区分几个参数：**

- 话题：rostopic list获取要订阅/发布的话题。得到话题：/turtlr1/pose

- 消息类型：rostopic type [/turtle1/pose]，传参话题，获取话题的消息类型。得到话题消息类型：turtlesim/Pose

- 消息格式：rosmsg info [turtlesim/Pose]，传参消息，获取消息的格式。得到消息格式：

  float32 x

  float32 y

  float32 theta

  float32 linear_velocity

  float32 angular_velocity

- 回调函数：订阅话题时，需要传参回调函数，在回调函数中处理获取到的话题的消息。



**int argc和char* argv[]什么含义和作用：**





**spin回旋函数：**



**ros::start()：什么时候用此函数**





## 话题 Topic

### 话题的发布

```c++
int main(int argc, char* argv[])
{
    //(1)初始化ros节点：传参为argc， argv， 节点名称
    ros::init(argc, argv, "qt_speed_board");	//qt_speed_board为自己创建的节点名称
    //(2)
    ros::start();
    //(3)创建句柄对象
    ros::NodeHandle n;
    //(4)创建发布/订阅对象，设置发布/订阅消息类型（此处为“chatter”，或可为“/cmd_vel”等）、消息队列参数
    ros::Publisher pub = n.advertise<std_msgs::String>("chatter", 1000);
    //(5)创建消息对象，设置消息参数，循环发送
    std_msgs msg;
    msg.x_data = 1.0;
    msg.y_data = 2.0;
    ...

    while(ros::ok())
    {
        pub.publish(msg);

        ros::spinOnce();
    }
}

```



### 话题的订阅

需要设计回调函数，在回调函数中传参为订阅的消息类型，在回调函数中处理订阅到的消息，将信息解析出来。

```c++
//回调函数：处理订阅到的话题的消息
void doPose(const turtlesim::Pose::ConstPtr& p)
{
    ROS::INFO("乌龟位姿信息：x = %.2f, y = %.2f, theta = %.2f, lv = %.2f, av = %.2f",
             p->x, p->y, p->theta, p->linear_velocity, p->angular_velocity);
}

int main(int argc, char* argv[])
{
    //(1)初始化ros节点
    ros::init(argc, argv, "sub_pose");	//"sub_pose"为此节点名称
    //(2)创建ros句柄
    ros::NodeHandle nh;
    //(3)创建订阅者对象
    ros::Subscriber sub = nh.subscribe<turtlesim::Pose>("/turtle1/pose", 1000, doPose);
    //(4)回调函数doPose处理订阅的数据
    //(5)spin
    ros::spin();
    return 0;
}
```





### 两者比较









# Qt_ROS项目

## 基础

Qt和ROS的关联：

- ROS的workspace、pkg等按照ros规则建立，然后通过catkin_make编译成功，形成可执行文件
- Qt中通过CMakeLists.txt文件打开项目，按照Qt的mainwindow、ui等创建文件，在mainwindow和ui中实现窗口等界面，通过调用ROS的可执行文件的函数，来操作ROS系统



注意：

- ros的pkg先catkin_make编译，成功后，再通过qtcreator打开项目，编译运行

### 创建pkg

- ws文件夹、src文件夹创建
- src文件夹中创建pkg：catkin_create_qt_pkg  [qt_ros_pkg_demo]  [depend]





### 修改CMakelists文件



```shell
##############################################################################
# CMake
##############################################################################

cmake_minimum_required(VERSION 2.8.0)
project(ros_qt_demo)
#add
set(CMAKE_INCLUDE_CURRENT_DIR ON)
##############################################################################
# Catkin
##############################################################################

# qt_build provides the qt cmake glue, roscpp the comms for a default talker
find_package(catkin REQUIRED COMPONENTS roscpp)
include_directories(${catkin_INCLUDE_DIRS})
#add
find_package(Qt5 REQUIRED Core Widgets)
set(QT_LIBRARIES Qt5::Widgets)
# Use this to define what the package will export (e.g. libs, headers).
# Since the default here is to produce only a binary, we don't worry about
# exporting anything. 
catkin_package()

##############################################################################
# Qt Environment
##############################################################################

# this comes from qt_build's qt-ros.cmake which is automatically 
# included via the dependency call in package.xml
#remove
#rosbuild_prepare_qt4(QtCore QtGui) # Add the appropriate components to the component list here

##############################################################################
# Sections
##############################################################################

file(GLOB QT_FORMS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} ui/*.ui)
file(GLOB QT_RESOURCES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} resources/*.qrc)
file(GLOB_RECURSE QT_MOC RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} FOLLOW_SYMLINKS include/ros_qt_demo/*.hpp *.h)

#change
QT5_ADD_RESOURCES(QT_RESOURCES_CPP ${QT_RESOURCES})
QT5_WRAP_UI(QT_FORMS_HPP ${QT_FORMS})
QT5_WRAP_CPP(QT_MOC_HPP ${QT_MOC})

##############################################################################
# Sources
##############################################################################

file(GLOB_RECURSE QT_SOURCES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} FOLLOW_SYMLINKS src/*.cpp)

##############################################################################
# Binaries
##############################################################################

add_executable(ros_qt_demo ${QT_SOURCES} ${QT_RESOURCES_CPP} ${QT_FORMS_HPP} ${QT_MOC_HPP})
target_link_libraries(ros_qt_demo ${QT_LIBRARIES} ${catkin_LIBRARIES})
install(TARGETS ros_qt_demo RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

```



### 修改package.xml文件



```xml
<?xml version="1.0"?>
<package>
  <name>ros_qt_demo</name>
  <version>0.1.0</version>
  <description>

     ros_qt_demo

  </description>
  <maintainer email="chengyangkj@qq.com">chengyangkj</maintainer>
  <author>chengyangkj</author>
  <license>BSD</license>
  <!-- <url type="bugtracker">https://github.com/chengyangkj/ros_qt_demo/issues</url> -->
  <!-- <url type="repository">https://github.com/chengyangkj/ros_qt_demo</url> -->

  <buildtool_depend>catkin</buildtool_depend>
  <build_depend>qt_build</build_depend>
  <build_depend>roscpp</build_depend>
  <build_depend>libqt4-dev</build_depend>
  <run_depend>qt_build</run_depend>
  <run_depend>roscpp</run_depend>
  <run_depend>libqt4-dev</run_depend>
  <build_depend>roscpp</build_depend>
  <run_depend>roscpp</run_depend>
  <build_depend>rviz</build_depend>
  <run_depend>rviz</run_depend>

 
</package>


```



### 修改mainwindow.hpp



```shell
#before
include <QtGui/QMainWindow>

#change
include <QtWidgets/QMainWindow>
```



### 编译pkg

- 进入ws文件夹，catkin_make
- 编译没问题后，才可在qt中打开项目



### qt打开项目

- 选择打开**工作空间ws下**的CMakeLists.txt文件
- 编译运行
- 默认会创建连接：



## 分步

### qt中实现订阅消息



### 方向按键、消息发布、控制小乌龟运动

方向按键->发布cmd_vel消息->turtle订阅此消息->turtle移动

- 方向按键：
  - 控件绘制；
  - 按键与键盘快捷键关联；
  - 按键按下槽函数：
    - 非全向轮模式：获取按下的按键的字符；通过map映射表找到方向数组；求得x、y、z、angular数据；赋值给msg
    - 全向轮模式：
- 发布cmd_vel消息：
  - 转化为x，y，z，angular四个数据
  - 创建msg，并发布
- 运行小乌龟，小乌龟订阅cmd_vel消息，然后运动





键盘按键控制小乌龟运动原理：

map<char, vector<int>> m;

消息定义：turtlesim/



























