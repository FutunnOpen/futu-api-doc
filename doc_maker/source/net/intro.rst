
.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

=====
介绍
=====

.. _FutuOpenD: ../intro/FutuOpenDGuide.html
.. _intro: ../intro/intro.html


.NET API简介
-------------
  * .NET API依赖FutuOpenD网关客户端，需要先运行登录 FutuOpenD_

  * .NET API开源了调用库代码，FTAPI4Net.sln采用VS2013编译，要求平台.NET Framework 4.5。或者FTAPI4NetCore.sln采用VS2017编译，采用NetCore2.1。Windows下使用32位.Net，Linux和Mac使用64位.NET。 用户可以根据需要采用更新的VS版本升级源码后编译目标调用库

  * Sample提供了几个简单的行情和交易获取demo，可以用于上手学习。

  * 具体支持交易和行情品种参考\ `FutuOpenD网关客户端简介 <../intro/intro.html>`_
  
  * 下载地址: 暂时请到FUTU OPEN API的QQ开发群里下载（108534288，229850364）

接口框架
-------------
 * 为了保证性能最大，我们的中间层采用C++编写，然后提供C#接口调用层

 .. image:: ../_static/NETAPI.png

.. note::
   因为涉及到底层Native线程和.NET线程回调的问题，回调时需要特别注意自己的代码所处的线程。建议简单过滤后，把消息抛到上层主线程处理。

代码结构
-------------

.. code-block:: text

	|-- FTAPI4Net
	|   |-- lib 所有依赖的库，包含第3方库和API库。即用户不必自己生成API库。
	|   |-- FTCAPI.cs  Native接口导入类
	|   |-- pb  pb自动生成文件，用于组包解包pb
	|   |-- FTAPI_Trd.cs 交易接口和交易操作函数
	|   |-- FTAPI_Qot.cs 行情接口和交易操作函数
	|   |-- FTAPI.cs 连接层接口
	|    
	`-- Sample 演示demo

调用须知
-------------
  * FTSPI_Conn、FTSPI_Qot、FTSPI_Trd调用和回调会在不同线程，注意线程安全问题。

  * 所有接口以异步回调方式完成

  * FTAPI4Net\lib目录下的FTAPIChannel.dll件记得拷贝到程序运行目录下


--------------

  .. _GetGlobalState: ../protocol/quote_protocol.html#getglobalstate-proto-1002
  .. _Sub: ../protocol/quote_protocol.html#qot-sub-proto-3001
  .. _RegQotPush: ../protocol/quote_protocol.html#qot-regqotpush-proto-3002
  .. _GetSubInfo: ../protocol/quote_protocol.html#qot-getsubinfo-proto-3003
  .. _GetTicker: ../protocol/quote_protocol.html#qot-getticker-proto-3010
  .. _GetBasicQot: ../protocol/quote_protocol.html#qot-getbasicqot-proto-3004
  .. _GetOrderBook: ../protocol/quote_protocol.html#qot-getorderbook-proto-3012
  .. _GetKL: ../protocol/quote_protocol.html#qot-getkl-proto-3006
  .. _GetRT: ../protocol/quote_protocol.html#qot-getrt-proto-3008
  .. _GetBroker: ../protocol/quote_protocol.html#qot-getbroker-proto-3014
  .. _GetHistoryKL: ../protocol/quote_protocol.html#qot-gethistorykl-proto-3100
  .. _GetHistoryKLPoints: ../protocol/quote_protocol.html#qot-gethistoryklpoints-proto-3101
  .. _GetRehab: ../protocol/quote_protocol.html#qot-getrehab-proto-3102
  .. _RequestRehab: ../protocol/quote_protocol.html#qot-requestrehab-proto-3105
  .. _RequestHistoryKL: ../protocol/quote_protocol.html#qot-requesthistorykl-proto-3103
  .. _RequestHistoryKLQuota: ../protocol/quote_protocol.html#qot-requesthistoryklquota-proto-3104
  .. _GetTradeDate: ../protocol/quote_protocol.html#qot-gettradedate-proto-3200
  .. _GetStaticInfo: ../protocol/quote_protocol.html#qot-getstaticinfo-proto-3202
  .. _GetSecuritySnapshot: ../protocol/quote_protocol.html#qot-getsecuritysnapshot-proto-3203
  .. _GetPlateSet: ../protocol/quote_protocol.html#qot-getplateset-proto-3204
  .. _GetPlateSecurity: ../protocol/quote_protocol.html#qot-getplatesecurity-proto-3205
  .. _GetReference: ../protocol/quote_protocol.html#qot-getreference-proto-3206
  .. _GetOwnerPlate: ../protocol/quote_protocol.html#qot-getownerplate-proto-3207
  .. _GetHoldingChangeList: ../protocol/quote_protocol.html#qot-getholdingchangelist-proto-3208
  .. _GetOptionChain: ../protocol/quote_protocol.html#qot-getoptionchain-proto-3209
  .. _GetWarrant: ../protocol/quote_protocol.html#qot-getwarrant-proto-3210
  .. _GetCapitalFlow: ../protocol/quote_protocol.html#qot-getcapitalflow-proto-3211
  .. _GetCapitalDistribution: ../protocol/quote_protocol.html#qot-getcapitaldistribution-proto-3212
  .. _GetUserSecurity: ../protocol/quote_protocol.html#qot-getusersecurity-proto-3213
  .. _ModifyUserSecurity: ../protocol/quote_protocol.html#qot-modifyusersecurity-proto-3214
  .. _Notify: ../protocol/quote_protocol.html#notify-proto-1003
  .. _UpdateBasicQot: ../protocol/quote_protocol.html#qot-updatebasicqot-proto-3005
  .. _UpdateKL: ../protocol/quote_protocol.html#qot-updatekl-proto-3007
  .. _UpdateRT: ../protocol/quote_protocol.html#qot-updatert-proto-3009
  .. _UpdateTicker: ../protocol/quote_protocol.html#qot-updateticker-proto-3011
  .. _UpdateOrderBook: ../protocol/quote_protocol.html#qot-updateorderbook-proto-3013
  .. _UpdateBroker: ../protocol/quote_protocol.html#qot-updatebroker-proto-3015
  .. _UpdateOrderDetail: ../protocol/quote_protocol.html#qot-updateorderdetail-proto-3017
  .. _GetAccList: ../protocol/trade_protocol.html#trd-getacclist-proto-2001
  .. _UnlockTrade: ../protocol/trade_protocol.html#trd-unlocktrade-proto-2005
  .. _SubAccPush: ../protocol/trade_protocol.html#trd-subaccpush-proto-2008
  .. _GetFunds: ../protocol/trade_protocol.html#trd-getfunds-proto-2101
  .. _GetPositionList: ../protocol/trade_protocol.html#trd-getpositionlist-proto-2102
  .. _GetMaxTrdQtys: ../protocol/trade_protocol.html#trd-getmaxtrdqtys-proto-2111
  .. _GetOrderList: ../protocol/trade_protocol.html#trd-getorderlist-proto-2201
  .. _GetOrderFillList: ../protocol/trade_protocol.html#trd-getorderfilllist-proto-2211
  .. _GetHistoryOrderList: ../protocol/trade_protocol.html#trd-gethistoryorderlist-proto-2221
  .. _GetHistoryOrderFillList: ../protocol/trade_protocol.html#trd-gethistoryorderfilllist-proto-2222
  .. _UpdateOrder: ../protocol/trade_protocol.html#trd-updateorder-proto-2208
  .. _UpdateOrderFill: ../protocol/trade_protocol.html#trd-updateorderfill-proto-2218
  
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


主要函数列表
---------------

FTAPI - API功能基类。
--------------------------------------

..  class:: FTAPI

API功能基类，提供连接方面公用的功能。FTAPI_Qot（行情）和FTAPI_Trd（交易）都继承该类。

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

SetConnSpi
~~~~~~~~~~~~~~~~~

..  method:: void SetConnSpi(FTSPI_Conn callback)

  设置连接相关回调。

  :param callback: 参加下面 `FTSPI_Conn` 的说明
  :return: void

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

..  method:: void OnInitConnect(FTAPI client, long errCode, String desc)

  初始化连接状态变化。

  :param client: 对应的FTAPI实例
  :param errCode: 错误码。0表示成功，可以进行后续请求。当高32位为 `ConnectFailType` 类型时，低32位为系统错误码；当高32位等于FTAPI.InitFail，则低32位为 `InitFailType` 类型。
  :param desc: 错误描述
  :return: void

--------------------------------------------

OnDisConnect
~~~~~~~~~~~~~~~~~

..  method:: void OnDisconnect(FTAPI client, long errCode)

  初始化连接状态变化。

  :param client: 对应的FTAPI实例
  :param errCode: 错误码。高32位为 `ConnectFailType` 类型，低32位为系统错误码；
  :return: void

--------------------------------------------

主要函数列表
---------------

行情类FTAPI_Qot
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
================================    ==============================================   ==============================
函数名（点开链接可查看具体协议）        功能简介                                         回调函数(FTSPI_Qot)            
================================    ==============================================   ==============================
GetGlobalState_                     获取全局状态                                     OnReply_GetGlobalState
Sub_                                订阅或者反订阅                                   OnReply_Sub
RegQotPush_                         注册推送                                         OnReply_RegQotPush
GetSubInfo_                         获取订阅信息                                     OnReply_GetSubInfo
GetTicker_                          获取逐笔,调用该接口前需要先订阅                  OnReply_GetTicker
GetBasicQot_                        获取基本行情,调用该接口前需要先订阅              OnReply_GetBasicQot
GetOrderBook_                       获取摆盘,调用该接口前需要先订阅                  OnReply_GetOrderBook
GetKL_                              获取K线，调用该接口前需要先订阅                  OnReply_GetKL
GetRT_                              获取分时，调用该接口前需要先订阅                 OnReply_GetRT
GetBroker_                          获取经纪队列，调用该接口前需要先订阅             OnReply_GetBroker
GetHistoryKL_                       获取本地历史K线                                  OnReply_GetHistoryKL
GetHistoryKLPoints_                 获取多股票多点本地历史K线                        OnReply_GetHistoryKLPoints
GetRehab_                           获取本地历史复权信息                             OnReply_GetRehab
RequestRehab_                       在线请求历史复权信息，不读本地历史数据DB         OnReply_RequestRehab
RequestHistoryKL_                   在线请求历史K线，不读本地历史数据DB              OnReply_RequestHistoryKL
RequestHistoryKLQuota_              获取历史K线已经用掉的额度                        OnReply_RequestHistoryKLQuota
GetTradeDate_                       获取交易日                                       OnReply_GetTradeDate
GetStaticInfo_                      获取静态信息                                     OnReply_GetStaticInfo
GetSecuritySnapshot_                获取股票快照                                     OnReply_GetSecuritySnapshot
GetPlateSet_                        获取板块集合下的板块                             OnReply_GetPlateSet
GetPlateSecurity_                   获取板块下的股票                                 OnReply_GetPlateSecurity
GetReference_                       获取相关股票                                     OnReply_GetReference
GetOwnerPlate_                      获取股票所属的板块                               OnReply_GetOwnerPlate
GetHoldingChangeList_               获取大股东持股变化列表                           OnReply_GetHoldingChangeList
GetOptionChain_                     筛选期权                                         OnReply_GetOptionChain
GetWarrant_                         筛选窝轮                                         OnReply_GetWarrant
GetCapitalFlow_                     获取资金流向                                     OnReply_GetCapitalFlow
GetCapitalDistribution_             获取资金分布                                     OnReply_GetCapitalDistribution
GetUserSecurity_                    获取自选股分组下的股票                           OnReply_GetUserSecurity
ModifyUserSecurity_                 修改自选股分组下的股票                           OnReply_ModifyUserSecurity
================================    ==============================================   ==============================

FTSPI_Qot行情推送接收接口函数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
==================================    =================================================
回调函数（点开链接可查看具体协议）                                功能简介          
==================================    ================================================= 
Notify_                               推送通知
UpdateBasicQot_                       推送基本行情
UpdateKL_                             推送K线
UpdateRT_                             推送分时
UpdateTicker_                         推送逐笔
UpdateOrderBook_                      推送买卖盘
UpdateBroker_                         推送经纪队列
UpdateOrderDetail_                    推送委托明细
==================================    ================================================= 


交易类FTAPI_Trd
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
================================    ==============================================   ==============================
函数名（点开链接可查看具体协议）        功能简介                                         回调函数(FTSPI_Trd)            
================================    ==============================================   ==============================
GetAccList_                         获取交易账户列表                                 OnReply_GetAccList
UnlockTrade_                        解锁                                             OnReply_UnlockTrade
SubAccPush_                         订阅接收推送数据的交易账户                       OnReply_SubAccPush
GetFunds_                           获取账户资金                                     OnReply_GetFunds
GetPositionList_                    获取账户持仓                                     OnReply_GetPositionList
GetMaxTrdQtys_                      获取最大交易数量                                 OnReply_GetMaxTrdQtys
GetOrderList_                       获取当日订单列表                                 OnReply_GetOrderList
GetOrderFillList_                   获取当日成交列表                                 OnReply_GetOrderFillList
GetHistoryOrderList_                获取历史订单列表                                 OnReply_GetHistoryOrderList
GetHistoryOrderFillList_            获取历史成交列表                                 OnReply_GetHistoryOrderFillList
================================    ==============================================   ==============================

FTSPI_Trd交易推送接收接口函数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
==================================    =================================================
回调函数（点开链接可查看具体协议）                                功能简介          
==================================    ================================================= 
UpdateOrder_                          订单状态变动通知(推送)
UpdateOrderFill_                      成交通知(推送)
==================================    ================================================= 

