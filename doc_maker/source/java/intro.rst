
.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

====
介绍
====

.. _FutuOpenD: ../intro/FutuOpenDGuide.html
.. _intro: ../intro/intro.html


Java API简介
-------------
  * Java API依赖FutuOpenD网关客户端，需要先运行登录 FutuOpenD_

  * Java API开源了调用库代码，FTAPI4J采用IntelliJ编译，要求平台JDK 8，用户可以根据需要采用更新的JDK版本升级源码后编译目标调用库

  * Sample提供了几个简单的行情和交易获取demo，可以用于上手学习。

  * 具体支持交易和行情品种参考\ `FutuOpenD网关客户端简介 <../intro/intro.html>`_

接口框架
-------------
 * 为了保证性能最大，我们的中间层采用C++编写，然后提供C#接口调用层
 .. image:: ../_static/NETAPI.png

.. note::
   因为涉及到底层Native线程和Java线程回调的问题，回调时需要特别注意自己的代码所处的线程。建议简单过滤后，把消息抛到上层主线程处理。

代码结构
-------------

.. code-block:: text

	|-- FTAPI4J
	|   |-- FTCAPI.java  Native接口导入类
	|   `-- pb  pb自动生成文件，用于组包解包pb
	|   |-- FTAPI_Trd.java 交易接口和交易操作函数
	|   |-- FTAPI_Qot.java 行情接口和交易操作函数
	|   |-- FTAPI.java 连接层接口
	|    
	`-- Sample 演示demo

调用须知
-------------
  * FTSPI_Conn、FTSPI_Qot、FTSPI_Trd调用和回调会在不同线程，注意线程安全问题。

  * 所有接口以异步回调方式完成


--------------

  .. _getGlobalState: ../protocol/quote_protocol.html#getglobalstate-proto-1002
  .. _sub: ../protocol/quote_protocol.html#qot-sub-proto-3001
  .. _regQotPush: ../protocol/quote_protocol.html#qot-regqotpush-proto-3002
  .. _getSubInfo: ../protocol/quote_protocol.html#qot-getsubinfo-proto-3003
  .. _getTicker: ../protocol/quote_protocol.html#qot-getticker-proto-3010
  .. _getBasicQot: ../protocol/quote_protocol.html#qot-getbasicqot-proto-3004
  .. _getOrderBook: ../protocol/quote_protocol.html#qot-getorderbook-proto-3012
  .. _getKL: ../protocol/quote_protocol.html#qot-getkl-proto-3006
  .. _getRT: ../protocol/quote_protocol.html#qot-getrt-proto-3008
  .. _getBroker: ../protocol/quote_protocol.html#qot-getbroker-proto-3014
  .. _getHistoryKL: ../protocol/quote_protocol.html#qot-gethistorykl-proto-3100
  .. _getHistoryKLPoints: ../protocol/quote_protocol.html#qot-gethistoryklpoints-proto-3101
  .. _getRehab: ../protocol/quote_protocol.html#qot-getrehab-proto-3102
  .. _requestRehab: ../protocol/quote_protocol.html#qot-requestrehab-proto-3105
  .. _requestHistoryKL: ../protocol/quote_protocol.html#qot-requesthistorykl-proto-3103
  .. _requestHistoryKLQuota: ../protocol/quote_protocol.html#qot-requesthistoryklquota-proto-3104
  .. _getTradeDate: ../protocol/quote_protocol.html#qot-gettradedate-proto-3200
  .. _getStaticInfo: ../protocol/quote_protocol.html#qot-getstaticinfo-proto-3202
  .. _getSecuritySnapshot: ../protocol/quote_protocol.html#qot-getsecuritysnapshot-proto-3203
  .. _getPlateSet: ../protocol/quote_protocol.html#qot-getplateset-proto-3204
  .. _getPlateSecurity: ../protocol/quote_protocol.html#qot-getplatesecurity-proto-3205
  .. _getReference: ../protocol/quote_protocol.html#qot-getreference-proto-3206
  .. _getOwnerPlate: ../protocol/quote_protocol.html#qot-getownerplate-proto-3207
  .. _getHoldingChangeList: ../protocol/quote_protocol.html#qot-getholdingchangelist-proto-3208
  .. _getOptionChain: ../protocol/quote_protocol.html#qot-getoptionchain-proto-3209
  .. _getWarrant: ../protocol/quote_protocol.html#qot-getwarrant-proto-3210
  .. _getCapitalFlow: ../protocol/quote_protocol.html#qot-getcapitalflow-proto-3211
  .. _getCapitalDistribution: ../protocol/quote_protocol.html#qot-getcapitaldistribution-proto-3212
  .. _getUserSecurity: ../protocol/quote_protocol.html#qot-getusersecurity-proto-3213
  .. _modifyUserSecurity: ../protocol/quote_protocol.html#qot-modifyusersecurity-proto-3214
  .. _notify: ../protocol/quote_protocol.html#notify-proto-1003
  .. _updateBasicQot: ../protocol/quote_protocol.html#qot-updatebasicqot-proto-3005
  .. _updateKL: ../protocol/quote_protocol.html#qot-updatekl-proto-3007
  .. _updateRT: ../protocol/quote_protocol.html#qot-updatert-proto-3009
  .. _updateTicker: ../protocol/quote_protocol.html#qot-updateticker-proto-3011
  .. _updateOrderBook: ../protocol/quote_protocol.html#qot-updateorderbook-proto-3013
  .. _updateBroker: ../protocol/quote_protocol.html#qot-updatebroker-proto-3015
  .. _updateOrderDetail: ../protocol/quote_protocol.html#qot-updateorderdetail-proto-3017
  .. _getAccList: ../protocol/trade_protocol.html#trd-getacclist-proto-2001
  .. _unlockTrade: ../protocol/trade_protocol.html#trd-unlocktrade-proto-2005
  .. _subAccPush: ../protocol/trade_protocol.html#trd-subaccpush-proto-2008
  .. _getFunds: ../protocol/trade_protocol.html#trd-getfunds-proto-2101
  .. _getPositionList: ../protocol/trade_protocol.html#trd-getpositionlist-proto-2102
  .. _getMaxTrdQtys: ../protocol/trade_protocol.html#trd-getmaxtrdqtys-proto-2111
  .. _getOrderList: ../protocol/trade_protocol.html#trd-getorderlist-proto-2201
  .. _getOrderFillList: ../protocol/trade_protocol.html#trd-getorderfilllist-proto-2211
  .. _getHistoryOrderList: ../protocol/trade_protocol.html#trd-gethistoryorderlist-proto-2221
  .. _getHistoryOrderFillList: ../protocol/trade_protocol.html#trd-gethistoryorderfilllist-proto-2222
  .. _updateOrder: ../protocol/trade_protocol.html#trd-updateorder-proto-2208
  .. _updateOrderFill: ../protocol/trade_protocol.html#trd-updateorderfill-proto-2218
  
---------------------------------------------------


主要函数列表
---------------

行情类FTAPI_Qot
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
================================    ==============================================   ==============================
函数名（点开链接可查看具体协议）        功能简介                                         回调函数(FTSPI_Qot)            
================================    ==============================================   ==============================
getGlobalState_                     获取全局状态                                       onReply_GetGlobalState
sub_                                订阅或者反订阅                                     onReply_Sub
regQotPush_                         注册推送                                           onReply_RegQotPush
getSubInfo_                         获取订阅信息                                       onReply_GetSubInfo
getTicker_                          获取逐笔,调用该接口前需要先订阅                      onReply_GetTicker
getBasicQot_                        获取基本行情,调用该接口前需要先订阅                  onReply_GetBasicQot
getOrderBook_                       获取摆盘,调用该接口前需要先订阅                      onReply_GetOrderBook
getKL_                              获取K线，调用该接口前需要先订阅                      onReply_GetKL
getRT_                              获取分时，调用该接口前需要先订阅                     onReply_GetRT
getBroker_                          获取经纪队列，调用该接口前需要先订阅                 onReply_GetBroker
getHistoryKL_                       获取本地历史K线                                     onReply_GetHistoryKL
getHistoryKLPoints_                 获取多股票多点本地历史K线                            onReply_GetHistoryKLPoints
getRehab_                           获取本地历史复权信息                                onReply_GetRehab
requestRehab_                       在线请求历史复权信息，不读本地历史数据DB             onReply_RequestRehab
requestHistoryKL_                   在线请求历史K线，不读本地历史数据DB                  onReply_RequestHistoryKL
requestHistoryKLQuota_              获取历史K线已经用掉的额度                           onReply_RequestHistoryKLQuota
getTradeDate_                       获取交易日                                         onReply_GetTradeDate
getStaticInfo_                      获取静态信息                                       onReply_GetStaticInfo
getSecuritySnapshot_                获取股票快照                                       onReply_GetSecuritySnapshot
getPlateSet_                        获取板块集合下的板块                               onReply_GetPlateSet
getPlateSecurity_                   获取板块下的股票                                   onReply_GetPlateSecurity
getReference_                       获取相关股票                                       onReply_GetReference
getOwnerPlate_                      获取股票所属的板块                                 onReply_GetOwnerPlate
getHoldingChangeList_               获取大股东持股变化列表                             onReply_GetHoldingChangeList
getOptionChain_                     筛选期权                                           onReply_GetOptionChain
getWarrant_                         筛选窝轮                                           onReply_GetWarrant
getCapitalFlow_                     获取资金流向                                       onReply_GetCapitalFlow
getCapitalDistribution_             获取资金分布                                       onReply_GetCapitalDistribution
getUserSecurity_                    获取自选股分组下的股票                             onReply_GetUserSecurity
modifyUserSecurity_                 修改自选股分组下的股票                             onReply_ModifyUserSecurity
================================    ==============================================   ==============================

FTSPI_Qot行情推送接收接口函数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
==================================    =================================================
回调函数（点开链接可查看具体协议）                                功能简介          
==================================    ================================================= 
notify_                               推送通知
updateBasicQot_                       推送基本行情
updateKL_                             推送K线
updateRT_                             推送分时
updateTicker_                         推送逐笔
updateOrderBook_                      推送买卖盘
updateBroker_                         推送经纪队列
updateOrderDetail_                    推送委托明细
==================================    ================================================= 


交易类FTAPI_Trd
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
================================    ==============================================   =================================
函数名（点开链接可查看具体协议）        功能简介                                         回调函数(FTSPI_Trd)            
================================    ==============================================   =================================
getAccList_                         获取交易账户列表                                   onReply_GetAccList
unlockTrade_                        解锁                                              onReply_UnlockTrade
subAccPush_                         订阅接收推送数据的交易账户                          onReply_SubAccPush
getFunds_                           获取账户资金                                        onReply_GetFunds
getPositionList_                    获取账户持仓                                       onReply_GetPositionList
getMaxTrdQtys_                      获取最大交易数量                                   onReply_GetMaxTrdQtys
getOrderList_                       获取当日订单列表                                   onReply_GetOrderList
getOrderFillList_                   获取当日成交列表                                   onReply_GetOrderFillList
getHistoryOrderList_                获取历史订单列表                                   onReply_GetHistoryOrderList
getHistoryOrderFillList_            获取历史成交列表                                   onReply_GetHistoryOrderFillList
================================    ==============================================   =================================

FTSPI_Trd交易推送接收接口函数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
==================================    =================================================
回调函数（点开链接可查看具体协议）                                功能简介          
==================================    ================================================= 
updateOrder_                          订单状态变动通知(推送)
updateOrderFill_                      成交通知(推送)
==================================    ================================================= 

