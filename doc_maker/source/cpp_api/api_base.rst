基础API
========

 .. _QotMarket: Base_API.html#_CPPv29QotMarket

------------------------------------



接口清单
------------------

 ==================      ==================================     ==================================================================
 导出类                  接口                                   说明
 ==================      ==================================     ==================================================================
 FOAC_Basic(加链接)      SetClientInfo(加链接)                  初始化.... 
 ==================      ==================================     ==================================================================
 


FOAC_Basic - 基础通道
---------------------

..  cpp:class:: FTAPI_Client : public FOAC_ConnectSink

提供的通道，与OpenD建议通信

------------------------------------

SetClientInfo
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: void SetClientInfo(const char* strClientID, i32_t nClientVer)

  设置调用api的client信息，非必调接口

  :param strClientID: const char*, client的标识
  :param nClientVer: i32_t, client的版本号
  :return: void

  :example:

  .. code:: cpp

   FTAPI_Client client;
   client.SetClientInfo("FOAC_Test", 100);
   client.InitConnect("127.0.0.1", 11111);
	
--------------------------------------------

SetProtoFormatType
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: void SetProtoFormatType(u8_t nProtoFmtType)

  协议格式类型，0代表Protobuf格式， 1代表Json格式，默认协议格式为Protobuf，非必调接口

  :param nProtoFmtType: u8_t, 协议格式类型
  :return: void

  :example:

  .. code:: cpp

   FTAPI_Client client;
   client.SetProtoFormatType(0);
   client.InitConnect("127.0.0.1", 11111);

--------------------------------------------

SetRSAPrivateKey
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: void SetRSAPrivateKey(const char* strRSAPrivateKey)

  设置RSA私钥文件，要求1024位， 格式为PKCS#1，网关客户端和api需配置相同的RSA私钥文件，在连接初始化成功后，网关会下发随机生成的AES加密密钥

  :param strRSAPrivateKey:  const char*, RSA私钥
  :return: void

  :example:

  .. code:: cpp

   FTAPI_Client client;
   client.SetRSAPrivateKey("");
   client.InitConnect("127.0.0.1", 11111);

--------------------------------------------

InitConnect
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: virtual bool InitConnect(const char* strAddr, u16_t nPort)

  初始化连接，包含了创建连接和发送InitConnect协议等， 打通信道，可正常首发业务协议包，结果通过OnInitConnect异步回调，可继承重载加自己的逻辑，但需保证包含基类的所有处理逻辑

  :param strAddr:  const char*, OpenD服务的IP地址
  :param nPort:  u16_t, OpenD服务的端口号
  :return: bool 是否启动了执行，不代表连接结果，结果通过OnInitConnect回调

  :example:

  .. code:: cpp

   FTAPI_Client client;
   client.InitConnect("127.0.0.1", 11111);

--------------------------------------------

OnInitConnect
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: virtual void OnInitConnect(bool bSucceed, const char* strDesc)

  初始化连接结果回调函数，在回调函数中响应处理，可继承重载加自己的逻辑，但需保证包含基类的所有处理逻辑

  :param bSucceed:  bool， 是否成功
  :param strDesc:  const char*，结果描述
  :return: void

  :example:

  .. code:: cpp
   
   class FTAPI_Sample : public FTAPI_Client
   {
	   void OnInitConnect(bool bRet, const char* strDesc)
	   {
		//通知初始化连接结果
	   }
   };
   FTAPI_Client client;
   client.InitConnect("127.0.0.1", 11111);

--------------------------------------------

IsConnected
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: bool IsConnected()

  是否与OpenD连接着

  :return: bool 是否连接着

  :example:

  .. code:: cpp

   FTAPI_Client client;
   client.InitConnect("127.0.0.1", 11111);
   client.IsConnected();

--------------------------------------------

Close
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: bool Close()

  执行关闭与OpenD的连接，异步关闭

  :return: bool 是否启动了执行

  :example:

  .. code:: cpp

   FTAPI_Client client;
   client.InitConnect("127.0.0.1", 11111);
   client.Close();

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

  :example:

  .. code:: cpp

   FTAPI_Client client;
   client.InitConnect("127.0.0.1", 11111);
   GetGlobalState::C2S *pC2S = new GetGlobalState::C2S();
   pC2S->set_userid(userID);
   GetGlobalState::Request req;
   req.set_allocated_c2s(pC2S);
   Send(FTAPI_ProtoID_GetGlobalState, req);

--------------------------------------------

OnConnect
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: virtual void OnConnect(u32_t nConnectID, i64_t nErrCode)

  连接结果回调函数，在回调函数中响应处理

  :param nConnectID:  u32_t， 连接标识
  :param nErrCode:  i64_t，连接结果描述
  :return: void

  :example:

  .. code:: cpp

   class FTAPI_Sample : public FTAPI_Client
   {
	   void OnConnect(u32_t nConnectID, i64_t nErrCode)
	   {
		//通知连接结果
	   }
   };
   FTAPI_Client client;
   client.InitConnect("127.0.0.1", 11111);

--------------------------------------------

OnDisConnect
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: virtual void OnDisConnect(u32_t nConnectID, i64_t nErrCode, OMTcpDisConnectType enDisConnType)

  断开连接回调函数，在回调函数中响应处理

  :param nConnectID:  u32_t， 连接标识
  :param nErrCode:  i64_t，连接结果描述
  :param enDisConnType:  OMTcpDisConnectType，连接断开原因类型枚举
  :return: void

  :example:

  .. code:: cpp

   class FTAPI_Sample : public FTAPI_Client
   {
	   OnDisConnect(u32_t nConnectID, i64_t nErrCode, OMTcpDisConnectType enDisConnType)
	   {
		//通知断开连接结果
	   }
   };
   FTAPI_Client client;
   client.InitConnect("127.0.0.1", 11111);

--------------------------------------------

OnTimeTicker
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: virtual void OnTimeTicker(u32_t nConnectID)

  计时器，在回调函数中响应处理

  :param nConnectID:  u32_t， 连接标识
  :return: void

  :example:

  .. code:: cpp

   class FTAPI_Sample : public FTAPI_Client
   {
	   OnTimeTicker(u32_t nConnectID)
	   {
		//计时器
	   }
   };
   FTAPI_Client client;
   client.InitConnect("127.0.0.1", 11111);

--------------------------------------------

OnReply
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: virtual void OnReply(u32_t nConnectID, FTAPI_ReqReplyType enReqReplyType, const FTAPI_ProtoHeader& protoHeader, const i8_t* pProtoData, i32_t nDataLen)

  回复请求的连接，在回调函数中响应处理

  :param nConnectID:  u32_t， 连接标识
  :param enReqReplyType:  FTAPI_ReqReplyType，FTAPI请求应答类型枚举
  :param protoHeader:  const FTAPI_ProtoHeader&，FTAPI协议包头结构定义
  :param pProtoData:  const i8_t*， 协议数据起地址
  :param nDataLen:  i32_t，协议数据长度
  :return: void

  :example:

  .. code:: cpp

   class FTAPI_Sample : public FTAPI_Client
   {
	   OnReply(u32_t nConnectID, FTAPI_ReqReplyType enReqReplyType, const FTAPI_ProtoHeader& protoHeader, const i8_t* pProtoData, i32_t nDataLen)
	   {
		//请求连接的回复
	   }
   };
   FTAPI_Client client;
   client.InitConnect("127.0.0.1", 11111);

--------------------------------------------

OnPush
~~~~~~~~~~~~~~~~~~~~~~

..  cpp:function:: virtual void OnPush(u32_t nConnectID, const FTAPI_ProtoHeader& protoHeader, const i8_t* pProtoData, i32_t nDataLen)

  推送请求的连接数据，在回调函数中响应处理

  :param nConnectID:  u32_t， 连接标识
  :param protoHeader:  const FTAPI_ProtoHeader&，FTAPI协议包头结构定义
  :param pProtoData:  const i8_t*， 协议数据起地址
  :param nDataLen:  i32_t，协议数据长度
  :return: void

  :example:

  .. code:: cpp

   class FTAPI_Sample : public FTAPI_Client
   {
	   OnPush(u32_t nConnectID, const FTAPI_ProtoHeader& protoHeader, const i8_t* pProtoData, i32_t nDataLen)
	   {
		//推送请求的连接数据
	   }
   };
   FTAPI_Client client;
   client.InitConnect("127.0.0.1", 11111);

--------------------------------------------

FOAC_Sample - 范例
---------------------

..  cpp:class:: FOAC_Sample : public FOAC_Basic

提供的范例，包含一些接口的组装和使用