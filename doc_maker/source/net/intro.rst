.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

====
介绍
====

.. _FutuOpenD: ../intro/FutuOpenDGuide.html
.. _intro: ../intro/intro.html


FTAPI4Net介绍
-------------
  * FTAPI4Net依赖FutuOpenD网关客户端，需要先运行登录 FutuOpenD_

  * FTAPI4Net开源了调用库代码，FTAPI4Net.sln采用VS2013编译，要求平台.NET Framework 4.5，用户可以根据需要采用更新的VS版本升级源码后编译目标调用库

  * FTAPI4NetSample提供了几个简单的行情和交易获取demo，可以用于上手学习。

  * 具体支持交易和行情品种参考\ `FutuOpenD网关客户端简介 <../intro/intro.html>`_

接口框架
-------------
 * 为了保证性能最大，我们的中间层采用C++编写，然后提供C#接口调用层
 .. image:: ../_static/NETAPI.png

.. note::
   因为涉及到底层Native线程和.NET线程回调的问题，回调时需要特别注意自己的代码所处的线程。

调用须知
-------------
  * FTSPI_Conn、FTSPI_Qot、FTSPI_Trd调用和回调会在不同线程，注意线程安全问题。

  * 所有接口以异步回调方式完成

  * FTAPI4Net\lib目录下的FTAPIChannel.dll、OM.dll文件记得拷贝到程序运行目录下


--------------

  .. _InitConnect: ../protocol/base_define.html#initconnect-proto-1001
  .. _GetGlobalState: ../protocol/base_define.html#initconnect-proto-1001

---------------------------------------------------


主要函数列表
----------

行情类FTAPI_Qot，回调类FTSPI_Qot:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================================    ==============================================   ==============================   ==================
函数名                                 功能简介                                         回调函数                              收发协议
================================    ==============================================   ==============================   ==================
GetTradingDays                      获取交易日                                         OnReply_GetAccList               InitConnect_
GetStockBasicInfo                   获取指定市场中特定类型的股票基本信息                    OnReply_GetAccList               GetGlobalState_
GetTradingDays                      获取交易日                                          OnReply_GetAccList               GetGlobalState_
GetStockBasicInfo                   获取指定市场中特定类型的股票基本信息                    OnReply_GetAccList                GetGlobalState_
================================    ==============================================   ==============================   ==================

FTSPI_Qot行情推送接收接口函数:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================================    ==============================================   ==============================   ==================
函数名                                 功能简介                                         回调函数                              收发协议
================================    ==============================================   ==============================   ==================
GetTradingDays                      获取交易日                                         OnReply_GetAccList               InitConnect_
GetStockBasicInfo                   获取指定市场中特定类型的股票基本信息                    OnReply_GetAccList               GetGlobalState_
GetTradingDays                      获取交易日                                          OnReply_GetAccList               GetGlobalState_
GetStockBasicInfo                   获取指定市场中特定类型的股票基本信息                    OnReply_GetAccList                GetGlobalState_
================================    ==============================================   ==============================   ==================


交易类FTAPI_Trd，回调类FTSPI_Trd:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================================    ============================================================================ ==================
函数名                                 功能简介                                                                       收发协议
================================    ============================================================================ ==================
GetTradingDays                      获取交易日                                                                     InitConnect_
GetStockBasicInfo                   获取指定市场中特定类型的股票基本信息                                               GetGlobalState_
GetTradingDays                      获取交易日                                                                     GetGlobalState_
GetStockBasicInfo                   获取指定市场中特定类型的股票基本信息                                               GetGlobalState_
================================    ============================================================================ ==================

FTSPI_Trd交易推送接收接口函数:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================================    ============================================================================ ==================
函数名                                 功能简介                                                                       收发协议
================================    ============================================================================ ==================
GetTradingDays                      获取交易日                                                                     InitConnect_
GetStockBasicInfo                   获取指定市场中特定类型的股票基本信息                                               GetGlobalState_
GetTradingDays                      获取交易日                                                                     GetGlobalState_
GetStockBasicInfo                   获取指定市场中特定类型的股票基本信息                                               GetGlobalState_
================================    ============================================================================ ==================






	
	
	

