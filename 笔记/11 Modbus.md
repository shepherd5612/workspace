# Modbus基础知识



|                              | 信号含义         | 可读可写 |
| ---------------------------- | ---------------- | -------- |
| Coils，线圈                  | 逻辑状态，ON/OFF |          |
| Discrete Inputs，离散输入    |                  |          |
| InputRegisters，输入寄存器   |                  |          |
| HoldingRegisters，保持寄存器 |                  |          |





# 过程



PDU：协议数据单元



QModbusDevice

QModbusClient

QModbusRtuSerialMaster + QModbusTcpClient







### RTU

打开串口：初始化串口参数（串口名称、波特率、数据位、校验位、停止位、流控制）

建立连接

主站发送





### TCP/IP







## 程序流程

- 初始化：界面、默认值等初始化

- 连接：master连接方式选定，并与相应的硬件接口建立匹配：

  - 建立TCP/IP连接：tcp与对应端口匹配（ip地址、port）

  - 建立串口连接：串口设定串口参数（COM口、波特率、数据位、校验位、停止位、流控制等）

    **Question：Qt中的connectDevice（）函数是怎么连接至串口COM1的，源码什么含义？**

- master发送功能码，请求从机数据

- slave接收功能码，并发送给主机相应数据

- master接收从机数据，解析数据





## 连接流程

主站：

- PDU数据封装：功能码+数据，封装起来
- 从机地址封装：将发给哪台从机的地址也封装起来
- 校验：
- 发送：
- 接收判断：
  - 判断是否接收完成
  - 数据完整性校验
  - CRC校验
- 接收数据处理：
  - 
  - 
  - 



- 









modbus项目：



多个从机，轮询



- 函数1：pollSlave(int slaveAddr, int funcCode, char* buf, int len);	//查询从机地址为slaveAddr，功能码为funcCode，收到的数据保存到buf中，数据长度为len

- 函数2：数据解析：



