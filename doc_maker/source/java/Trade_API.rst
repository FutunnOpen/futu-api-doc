.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

=======
交易API
=======

--------------

  .. _getGlobalState: ../protocol/base_define.html#getglobalstate-proto-1002
  .. _sub: ../protocol/quote_protocol.html#qot-sub-proto-3001
  .. _regQotPush: ../protocol/quote_protocol.html#qot-regqotpush-proto-3002
  .. _getSubInfo: ../protocol/quote_protocol.html#qot-getsubinfo-proto-3003
  .. _getTicker: ../protocol/quote_protocol.html#qot-getticker-proto-3010
  .. _getBasicQot: ../protocol/quote_protocol.html#qot-getbasicqot-proto-3004
  .. _getOrderBook: ../protocol/quote_protocol.html#qot-getorderbook-proto-3012
  .. _getKL: ../protocol/quote_protocol.html#qot-getkl-proto-3006k
  .. _getRT: ../protocol/quote_protocol.html#qot-getrt-proto-3008
  .. _getBroker: ../protocol/quote_protocol.html#qot-getbroker-proto-3014
  .. _getRehab: ../protocol/quote_protocol.html#qot-getrehab-proto-3102
  .. _requestRehab: ../protocol/quote_protocol.html#qot-requestrehab-proto-3105
  .. _requestHistoryKL: ../protocol/quote_protocol.html#qot-requesthistorykl-proto-3103k
  .. _requestHistoryKLQuota: ../protocol/quote_protocol.html#qot-requesthistoryklquota-proto-3104k
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
  .. _notify: ../protocol/base_define.html#notify-proto-1003
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

FTAPI_Trd成员函数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+ FTAPI_Trd继承自\ `FTAPI_Conn <./Base_API.html#ftapi-conn>`_ ，连接层调用接口参考FTAPI_Conn说明。


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
PlaceOrder_                         下单                                               OnReply_PlaceOrder
ModifyOrder_                        改单                                               OnReply_ModifyOrder
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

