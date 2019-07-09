
.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

========
基础API
========

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



