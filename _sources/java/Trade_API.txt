.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

=======
交易API
=======

--------------

  .. _getAccList: ../protocol/trade_protocol.html#trd-getacclist-proto-2001
  .. _unlockTrade: ../protocol/trade_protocol.html#trd-unlocktrade-proto-2005
  .. _subAccPush: ../protocol/trade_protocol.html#trd-subaccpush-proto-2008
  .. _getFunds: ../protocol/trade_protocol.html#trd-getfunds-proto-2101
  .. _getPositionList: ../protocol/trade_protocol.html#trd-getpositionlist-proto-2102
  .. _getMaxTrdQtys: ../protocol/trade_protocol.html#trd-getmaxtrdqtys-proto-2111
  .. _getOrderList: ../protocol/trade_protocol.html#trd-getorderlist-proto-2201
  .. _placeOrder: ../protocol/trade_protocol.html#trd-placeorder-proto-2202
  .. _modifyOrder: ../protocol/trade_protocol.html#trd-modifyorder-proto-2205
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
placeOrder_                         下单                                               OnReply_PlaceOrder
modifyOrder_                        改单                                               OnReply_ModifyOrder
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

