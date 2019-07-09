
.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

========
交易API
========

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



FTAPIBase连接层基类
-------------------

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

FTAPI_Trd主要函数列表
---------------------

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
---------------------


==================================    =================================================
回调函数（点开链接可查看具体协议）                                功能简介          
==================================    ================================================= 
UpdateOrder_                          订单状态变动通知(推送)
UpdateOrderFill_                      成交通知(推送)
==================================    ================================================= 

