基础通道
========

 .. _SetClientInfo: api_base.html#id3
 .. _SetProtoFormatType: api_base.html#id4
 .. _SetRSAPrivateKey: api_base.html#id5
 .. _InitConnect: api_base.html#id6
 .. _OnInitConnect: api_base.html#id7
 .. _IsConnected: api_base.html#id8
 .. _Close: api_base.html#id9
 .. _Send: api_base.html#id10
 .. _OnConnect: api_base.html#id11
 .. _OnDisConnect: api_base.html#id12
 .. _OnTimeTicker: api_base.html#id13
 .. _OnReply: api_base.html#id14
 .. _OnPush: api_base.html#id15

 .. _FTAPI_Sample: api_sample.html#ftapi-sample

------------------------------------



接口清单
------------------

 ====================      ==================================
 接口                       说明
 ====================      ==================================
 SetClientInfo_            设置api的client信息
 SetProtoFormatType_       协议格式类型
 SetRSAPrivateKey_         设置RSA私钥
 InitConnect_              初始化连接
 OnInitConnect_            初始化连接结果回调函数
 IsConnected_              是否与OpenD连接成功
 Close_                    执行关闭与OpenD的连接，异步关闭
 Send_                     发送与OpenD的API协议
 OnConnect_                连接结果回调
 OnDisConnect_             发生断线回调
 OnTimeTicker_             每秒定时回调
 OnReply_                  收到(请求)应答协议
 OnPush_                   收到推送协议
 ====================      ==================================

------------------

FTAPI_Client - 基础通道
-----------------------------------

..  cpp:class:: FTAPI_Client : public FTAPI_ConnectSink

提供的通道，与OpenD建立通信，可通过派生其进行调用，详情 FTAPI_Sample_

------------------------------------

SetClientInfo
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: void SetClientInfo(const char* strClientID, i32_t nClientVer)

  设置调用api的client信息，非必调接口

  :param strClientID: const char*, client的标识
  :param nClientVer: i32_t, client的版本号
  :return: void
	
--------------------------------------------

SetProtoFormatType
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: void SetProtoFormatType(u8_t nProtoFmtType)

  协议格式类型，0代表Protobuf格式， 1代表Json格式，默认协议格式为Protobuf，非必调接口

  :param nProtoFmtType: u8_t, 协议格式类型
  :return: void

--------------------------------------------

SetRSAPrivateKey
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: void SetRSAPrivateKey(const char* strRSAPrivateKey)

  设置RSA私钥文件，要求1024位， 格式为PKCS#1，网关客户端和api需配置相同的RSA私钥文件，在连接初始化成功后，网关会下发随机生成的AES加密密钥

  :param strRSAPrivateKey:  const char*, RSA私钥
  :return: void

--------------------------------------------

InitConnect
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: virtual bool InitConnect(const char* strAddr, u16_t nPort)

  初始化连接，包含了创建连接和发送InitConnect协议等， 打通信道，可正常首发业务协议包，结果通过OnInitConnect异步回调，可继承重载加自己的逻辑，但需保证包含基类的所有处理逻辑

  :param strAddr:  const char*, OpenD服务的IP地址
  :param nPort:  u16_t, OpenD服务的端口号
  :return: bool 是否启动了执行，不代表连接结果，结果通过OnInitConnect回调

--------------------------------------------

OnInitConnect
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: virtual void OnInitConnect(bool bSucceed, const char* strDesc)

  初始化连接结果回调函数，在回调函数中响应处理，可继承重载加自己的逻辑，但需保证包含基类的所有处理逻辑

  :param bSucceed:  bool， 是否成功
  :param strDesc:  const char*，结果描述
  :return: void

--------------------------------------------

IsConnected
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: bool IsConnected()

  是否与OpenD连接成功

  :return: bool 是否连接成功

--------------------------------------------

Close
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: bool Close()

  执行关闭与OpenD的连接，异步关闭

  :return: bool 是否启动了执行

--------------------------------------------

Send
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: u32_t Send(u32_t nProtoID, u8_t nProtoVer, const i8_t* pProtoData, i32_t nDataLen)

  发送与OpenD的API协议，通过返回值判断是否发送成功，返回0发送失败，返回非0发送成功，且返回值是发送此协议包产生的的唯一序列号

  :param nProtoID:  u32_t， 协议号
  :param nProtoVer:  u8_t，协议版本
  :param pProtoData:  const i8_t*， 协议数据起地址
  :param nDataLen:  i32_t，协议数据长度
  :return: u32_t 0为失败，非0为包序列号

--------------------------------------------

OnConnect
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: virtual void OnConnect(u32_t nConnectID, i64_t nErrCode)

  连接结果(成功或失败)回调，错误码为0代表连接成功，其他为失败

  :param nConnectID:  u32_t， 连接ID
  :param nErrCode:  i64_t，错误码，可通过OMTcpGetErrDesc得到具体错误描述
  :return: void

--------------------------------------------

OnDisConnect
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: virtual void OnDisConnect(u32_t nConnectID, i64_t nErrCode, OMTcpDisConnectType enDisConnType)

  发生断线回调

  :param nConnectID:  u32_t， 连接ID
  :param nErrCode:  i64_t，错误码，可通过OMTcpGetErrDesc得到具体错误描述
  :param enDisConnType:  OMTcpDisConnectType，断线类型
  :return: void

--------------------------------------------

OnTimeTicker
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: virtual void OnTimeTicker(u32_t nConnectID)

  每秒定时回调，供上层使用者当秒级定时器使用，处理如超时判断等

  :param nConnectID:  u32_t， 连接ID
  :return: void

--------------------------------------------

OnReply
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: virtual void OnReply(u32_t nConnectID, FTAPI_ReqReplyType enReqReplyType, const FTAPI_ProtoHeader& protoHeader, const i8_t* pProtoData, i32_t nDataLen)

  收到(请求)应答协议

  :param nConnectID:  u32_t， 连接ID
  :param enReqReplyType:  FTAPI_ReqReplyType，应答类型
  :param protoHeader:  const FTAPI_ProtoHeader&，协议头
  :param pProtoData:  const i8_t*， 协议数据起地址
  :param nDataLen:  i32_t，协议数据长度
  :return: void

--------------------------------------------

OnPush
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: virtual void OnPush(u32_t nConnectID, const FTAPI_ProtoHeader& protoHeader, const i8_t* pProtoData, i32_t nDataLen)

  收到推送协议

  :param nConnectID:  u32_t， 连接ID
  :param protoHeader:  const FTAPI_ProtoHeader&，协议头
  :param pProtoData:  const i8_t*， 协议数据起地址
  :param nDataLen:  i32_t，协议数据长度
  :return: void

--------------------------------------------
