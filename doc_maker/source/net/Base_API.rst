
.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

========
基础API
========

 .. _ConnectFailType: Base_API.html#id2
 .. _InitFailType: Base_API.html#id3
 .. _FTAPI_InitFail: Base_API.html#id4
  
---------------------------------------------------

枚举常量
---------

ConnectFailType - 连接错误码
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

连接错误码

..  cpp:enum:: ConnectFailType

 ..  attribute:: Unknown
 
  未知错误
  
 ..  attribute:: None
 
  没有错误
  
 ..  attribute:: CreateFailed
 
  socket创建失败

 ..  attribute:: CloseFailed

  socket close错误

 ..  attribute:: ShutdownFailed

 socket shutdown错误

 ..  attribute:: GetHost ByNameFailed

 gethostbyname错误

 ..  attribute:: GetHostByNameWrong

 gethostbyname调用成功，但返回的结果错误

 ..  attribute:: ConnectFailed

 连接失败

 ..  attribute:: BindFailed

 socket bind失败

 ..  attribute:: ListenFailed 

 socket listen失败

 ..  attribute:: SelectReturnError

 socket select错误

 ..  attribute:: SendFailed

 socket send失败

 ..  attribute:: RecvFailed

 socket recv失败
  
--------------------------------------

InitFailType - 初始化连接协议失败类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

初始化连接协议失败，即InitConnect协议相关的错误

..  cpp:enum:: InitFailType

 ..  attribute:: Unknow

 未知错误

 ..  attribute:: Timeout

 超时

 ..  attribute:: DisConnect

 连接断开

 ..  attribute:: SeriaNoNotMatch

 序列号不符

 ..  attribute:: SendInitReqFailed

 发送初始化协议失败

 ..  attribute:: OpenDReject

 FutuOpenD回包指定错误，具体错误看描述

--------------------------------------

FTAPI_InitFail - 初始化连接协议失败错误值
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

指定初始化连接协议失败，即InitConnect协议相关的错误，错误值：100。

--------------------------------------


FTAPI - API功能基类。
--------------------------------------

..  class:: FTAPI

API全局配置类，初始化和全局配置类。

-------------------------------------------------------------------------------------------------

Init
~~~~~~~~~~~~~~~~~

..  method:: static void Init()

  初始化底层通道，程序启动时首先调用

  :return: void

--------------------------------------------

UnInit
~~~~~~~~~~~~~~~~~

..  method:: static void UnInit()

  清理底层通道，程序结束时调用

  :return: void

--------------------------------------------


FTAPI_Conn连接层基类
-----------------------

..  class:: FTAPI

API功能基类，提供连接方面公用的功能。FTAPI_Qot（行情）和FTAPI_Trd（交易）都继承该类。

-------------------------------------------------------------------------------------------------


SetConnSpi
~~~~~~~~~~~~~~~~~

..  method:: void SetConnSpi(FTSPI_Conn callback)

  设置连接相关回调。

  :param callback: 参加下面 `FTSPI_Conn` 的说明
  :return: void

--------------------------------------------

SetClientInfo
~~~~~~~~~~~~~~~~~

..  method:: void SetClientInfo(string clientID, int clientVer)

  初始化连接信息。

  :param clientID: 连接标识符，请自定义独特的标识符
  :param clientVer: 连接版本号，请参考opendapi版本号
  :return: void

--------------------------------------------

InitConnect
~~~~~~~~~~~~~~~~~

..  method:: bool InitConnect(string ip, ushort port, bool isEnableEncrypt)

  初始化连接信息。

  :param ip: 连接地址
  :param port: 连接端口号
  :param isEnableEncrypt: 是否允许加密
  :return: bool 初始化失败返回false，其他错误依据callback返回

--------------------------------------------

SetRSAPrivateKey
~~~~~~~~~~~~~~~~~

..  method:: bool SetRSAPrivateKey(string key)

  设置密钥。

  :param key: 加密密钥
  :return: bool 初始化失败返回false，其他错误依据callback返回

--------------------------------------------

Close
~~~~~~~~~~~~~~~~~

..  method:: void Close()

  释放内存。当对象不再使用时调用，否则会有内存泄漏。

  :return: void

--------------------------------------------

FTSPI_Conn - 连接状态回调接口
------------------------------------------

..  class:: interface FTSPI_Conn

当与OpenD的连接状态变化时调用此接口。

------------------------------------

OnInitConnect
~~~~~~~~~~~~~~~~~

..  method:: void OnInitConnect(FTAPI_Conn client, long errCode, String desc)

  初始化连接状态变化。

  :param client: 对应的FTAPI实例
  :param errCode: 错误码。0表示成功，可以进行后续请求。当高32位为 ConnectFailType_ 类型时，低32位为系统错误码；当高32位等于 FTAPI_InitFail_，则低32位为 InitFailType_ 类型。
  :param desc: 错误描述
  :return: void

--------------------------------------------

OnDisConnect
~~~~~~~~~~~~~~~~~

..  method:: void OnDisconnect(FTAPI_Conn client, long errCode)

  连接断开。

  :param client: 对应的FTAPI实例
  :param errCode: 错误码。高32位为 ConnectFailType_ 类型，低32位为系统错误码；
  :return: void

--------------------------------------------


