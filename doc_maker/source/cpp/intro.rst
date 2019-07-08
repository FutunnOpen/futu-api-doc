
.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

====
介绍
====

.. _FutuOpenD: ../intro/FutuOpenDGuide.html
.. _intro: ../intro/intro.html


Cpp API简介
-------------
  * Cpp API依赖FutuOpenD网关客户端，需要先运行登录 FutuOpenD_

  * Sample提供了几个简单的行情和交易获取demo，可以用于上手学习。

  * 具体支持交易和行情品种参考\ `FutuOpenD网关客户端简介 <../intro/intro.html>`_

  * 下载地址: 暂时请到FUTU OPEN API的QQ开发群里下载（108534288，229850364）

接口框架
-------------
 * FTAPIChannel模块是用于初始化连接，维护连接，该模块提供动态库不提供源码。
 * FTAPI在FTAPIChannel基础上新增了一些业务上的收发接口，提供静态库以及源码。

   
代码结构
-------------

.. code-block:: text

	|-- Bin        API所需的动态静态库，包括API通道动态库，pb静态库，API静态库。
	|-- Include    API头文件
	|-- Src        API源码
	`-- Sample     演示demo
	
调用须知
-------------
  * FTSPI_Conn、FTSPI_Qot、FTSPI_Trd调用和回调会在不同线程，注意线程安全问题。

  * 所有接口以异步回调方式完成

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
  .. _OnPush_Notify: ../protocol/quote_protocol.html#notify-proto-1003
  .. _OnPush_UpdateBasicQot: ../protocol/quote_protocol.html#qot-updatebasicqot-proto-3005
  .. _OnPush_UpdateKL: ../protocol/quote_protocol.html#qot-updatekl-proto-3007
  .. _OnPush_UpdateRT: ../protocol/quote_protocol.html#qot-updatert-proto-3009
  .. _OnPush_UpdateTicker: ../protocol/quote_protocol.html#qot-updateticker-proto-3011
  .. _OnPush_UpdateOrderBook: ../protocol/quote_protocol.html#qot-updateorderbook-proto-3013
  .. _OnPush_UpdateBroker: ../protocol/quote_protocol.html#qot-updatebroker-proto-3015
  .. _OnPush_UpdateOrderDetail: ../protocol/quote_protocol.html#qot-updateorderdetail-proto-3017
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
  .. _OnPush_UpdateOrder: ../protocol/trade_protocol.html#trd-updateorder-proto-2208
  .. _OnPush_UpdateOrderFill: ../protocol/trade_protocol.html#trd-updateorderfill-proto-2218
  
---------------------------------------------------


主要函数列表
---------------

行情类FTAPI_Qot
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
================================    ==============================================   ==============================
函数名（点开链接可查看具体协议）        功能简介                                         回调函数(FTSPI_Qot)            
================================    ==============================================   ==============================
GetGlobalState_                     获取全局状态                                       OnReply_GetGlobalState
Sub_                                订阅或者反订阅                                     OnReply_Sub
RegQotPush_                         注册推送                                           OnReply_RegQotPush
GetSubInfo_                         获取订阅信息                                       OnReply_GetSubInfo
GetTicker_                          获取逐笔,调用该接口前需要先订阅                    OnReply_GetTicker
GetBasicQot_                        获取基本行情,调用该接口前需要先订阅                OnReply_GetBasicQot
GetOrderBook_                       获取摆盘,调用该接口前需要先订阅                    OnReply_GetOrderBook
GetKL_                              获取K线，调用该接口前需要先订阅                    OnReply_GetKL
GetRT_                              获取分时，调用该接口前需要先订阅                   OnReply_GetRT
GetBroker_                          获取经纪队列，调用该接口前需要先订阅               OnReply_GetBroker
GetHistoryKL_                       获取本地历史K线                                    OnReply_GetHistoryKL
GetHistoryKLPoints_                 获取多股票多点本地历史K线                          OnReply_GetHistoryKLPoints
GetRehab_                           获取本地历史复权信息                               OnReply_GetRehab
RequestRehab_                       在线请求历史复权信息，不读本地历史数据DB           OnReply_RequestRehab
RequestHistoryKL_                   在线请求历史K线，不读本地历史数据DB                OnReply_RequestHistoryKL
RequestHistoryKLQuota_              获取历史K线已经用掉的额度                          OnReply_RequestHistoryKLQuota
GetTradeDate_                       获取交易日                                         OnReply_GetTradeDate
GetStaticInfo_                      获取静态信息                                       OnReply_GetStaticInfo
GetSecuritySnapshot_                获取股票快照                                       OnReply_GetSecuritySnapshot
GetPlateSet_                        获取板块集合下的板块                               OnReply_GetPlateSet
GetPlateSecurity_                   获取板块下的股票                                   OnReply_GetPlateSecurity
GetReference_                       获取相关股票                                       OnReply_GetReference
GetOwnerPlate_                      获取股票所属的板块                                 OnReply_GetOwnerPlate
GetHoldingChangeList_               获取大股东持股变化列表                             OnReply_GetHoldingChangeList
GetOptionChain_                     筛选期权                                           OnReply_GetOptionChain
GetWarrant_                         筛选窝轮                                           OnReply_GetWarrant
GetCapitalFlow_                     获取资金流向                                       OnReply_GetCapitalFlow
GetCapitalDistribution_             获取资金分布                                       OnReply_GetCapitalDistribution
GetUserSecurity_                    获取自选股分组下的股票                             OnReply_GetUserSecurity
ModifyUserSecurity_                 修改自选股分组下的股票                             OnReply_ModifyUserSecurity
================================    ==============================================   ==============================

FTSPI_Qot行情推送接收接口函数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
==================================    =================================================
回调函数（点开链接可查看具体协议）                                功能简介          
==================================    ================================================= 
OnPush_Notify_                         推送通知
OnPush_UpdateBasicQot_                 推送基本行情
OnPush_UpdateKL_                       推送K线
OnPush_UpdateRT_                       推送分时
OnPush_UpdateTicker_                   推送逐笔
OnPush_UpdateOrderBook_                推送买卖盘
OnPush_UpdateBroker_                   推送经纪队列
==================================    ================================================= 


交易类FTAPI_Trd
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
================================    ==============================================   =================================
函数名（点开链接可查看具体协议）        功能简介                                         回调函数(FTSPI_Trd)            
================================    ==============================================   =================================
GetAccList_                         获取交易账户列表                                   OnReply_GetAccList
UnlockTrade_                        解锁                                               OnReply_UnlockTrade
SubAccPush_                         订阅接收推送数据的交易账户                         OnReply_SubAccPush
GetFunds_                           获取账户资金                                       OnReply_GetFunds
GetPositionList_                    获取账户持仓                                       OnReply_GetPositionList
GetMaxTrdQtys_                      获取最大交易数量                                   OnReply_GetMaxTrdQtys
GetOrderList_                       获取当日订单列表                                   OnReply_GetOrderList
GetOrderFillList_                   获取当日成交列表                                   OnReply_GetOrderFillList
GetHistoryOrderList_                获取历史订单列表                                   OnReply_GetHistoryOrderList
GetHistoryOrderFillList_            获取历史成交列表                                   OnReply_GetHistoryOrderFillList
================================    ==============================================   =================================

FTSPI_Trd交易推送接收接口函数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
==================================    =================================================
回调函数（点开链接可查看具体协议）                                功能简介          
==================================    ================================================= 
OnPush_UpdateOrder_                    订单状态变动通知(推送)
OnPush_UpdateOrderFill_                成交通知(推送)
==================================    ================================================= 

