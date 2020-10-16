.. note::

  Futu OpenAPI 文档已于2020年10月16日全新升级，请移步至 `新文档 <https://openapi.futunn.com/futu-api-doc/>`_ 

  旧的github文档将不再更新，并于2020年11月16日正式停止访问。


.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

==========
基础API
==========

.. _FutuOpenD: ../intro/FutuOpenDGuide.html
.. _intro: ../intro/intro.html
.. _ConnectFailType: base.html#id3
.. _InitFailType: base.html#id4
.. _FTAPI_InitFail: base.html#id5

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

 ..  attribute:: GetHostByNameFailed

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

InitFailType - 初始化连接协议失败
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
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

指定初始化连接协议失败，即InitConnect协议相关的错误，错误值：100。

--------------------------------------

FTAPI - API全局工具类。
--------------------------------------

..  class:: FTAPI

API全局工具类，提供API初始化销毁以及接口实例创建销毁接口。

-------------------------------------------------------------------------------------------------

InitFTApi
~~~~~~~~~~~~~~~~~

..  method:: static void InitFTApi()

  初始化底层通道，程序启动时首先调用

  :return: void

--------------------------------------------

UnInitFTApi
~~~~~~~~~~~~~~~~~

..  method:: static void InitFTApi()

  清理底层通道，程序结束时调用

  :return: void

--------------------------------------------

CreateQotApi
~~~~~~~~~~~~~~~~~

..  method:: FTAPI_Qot* CreateQotApi();

  创建行情接口实例

  :return: FTAPI_Qot* 行情接口实例指针

--------------------------------------------

ReleaseQotApi
~~~~~~~~~~~~~~~~~

..  method:: void ReleaseQotApi(FTAPI_Qot* pQot);


  销毁行情接口实例。

  :param pQot: 行情接口实例指针
  :return: void

--------------------------------------------

CreateTrdApi
~~~~~~~~~~~~~~~~~

..  method:: FTAPI_Trd* CreateTrdApi();

  创建交易接口实例

  :return: FTAPI_Trd* 交易接口实例指针

--------------------------------------------

ReleaseTrdApi
~~~~~~~~~~~~~~~~~

..  method:: void ReleaseTrdApi(FTAPI_Trd* pTrd);


  销毁交易接口实例。

  :param pTrd: 交易接口实例指针
  :return: void

--------------------------------------------


FTAPI_Conn - 连接层基类
--------------------------------------

..  class:: FTAPI_Conn

连接层基类，提供连接方面公用的功能，FTAPI_Qot以及FTAPI_Trd都继承于该基类。

-------------------------------------------------------------------------------------------------

SetClientInfo
~~~~~~~~~~~~~~~~~

..  method:: void SetClientInfo(const char* szClientID, Futu::i32_t nClientVer)

  设置客户端信息

  :param szClientID: 客户端标识
  :param nClientVer: 客户端版本
  :return: void

--------------------------------------------

SetRSAPrivateKey
~~~~~~~~~~~~~~~~~

..  method:: void SetRSAPrivateKey(const char* szRSAPrivateKey)

  设置密钥

  :param strRSAPrivateKey: 密钥
  :return: void

--------------------------------------------

InitConnect
~~~~~~~~~~~~~~~~~

..  method:: bool InitConnect(const char* szIPAddr, Futu::u16_t nPort, bool bEnableEncrypt)

  初始化连接

  :param szIPAddr: 地址
  :param nPort: 端口
  :param bEnableEncrypt: 启用加密
  :return: bool 是否启动了执行，不代表连接结果，结果通过OnInitConnect回调

--------------------------------------------

GetConnectID
~~~~~~~~~~~~~~~~~

..  method:: Futu::u64_t GetConnectID()

  此连接的连接ID，连接的唯一标识，InitConnect协议返回，没有初始化前为0

  :return: Futu::u64_t 连接ID

--------------------------------------------

Close
~~~~~~~~~~~~~~~~~

..  method:: bool Close()

  释放内存。当对象不再使用时调用，否则会有内存泄漏。

  :return: bool 是否成功

--------------------------------------------


RegisterConnSpi
~~~~~~~~~~~~~~~~~

..  method:: bool RegisterConnSpi(FTSPI_Conn* pSpi)

  注册回调，用于处理连接相关的事件。

  :param pSpi: 回调实例，该对象没有反注册前不可销毁
  :return: bool 是否成功

--------------------------------------------

UnregisterConnSpi
~~~~~~~~~~~~~~~~~

..  method:: void UnregisterConnSpi()

  反注册回调

  :return: bool 是否成功

--------------------------------------------

FTSPI_Conn - 连接状态回调接口
------------------------------------------

..  class:: FTSPI_Conn

当与OpenD的连接状态变化时调用此接口。

------------------------------------

OnInitConnect
~~~~~~~~~~~~~~~~~

..  method:: void OnInitConnect(FTAPI_Conn* pConn, Futu::i64_t nErrCode, const char* strDesc)

  初始化连接状态变化。

  :param pConn: 对应连接实例指针
  :param nErrCode: 错误码。0表示成功，可以进行后续请求。当高32位为 ConnectFailType_ 类型时，低32位为系统错误码；当高32位等于 FTAPI_InitFail_，则低32位为 InitFailType_ 类型。
  :param strDesc: 错误描述
  :return: void

--------------------------------------------

OnDisConnect
~~~~~~~~~~~~~~~~~

..  method:: void OnDisConnect(FTAPI_Conn* pConn, Futu::i64_t nErrCode)

  连接断开。

  :param pConn: 对应连接实例指针
  :param nErrCode: 错误码。高32位为 ConnectFailType_ 类型，低32位为系统错误码；
  :return: void























