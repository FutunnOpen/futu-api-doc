
.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

==========
交易接口
==========

--------------
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

