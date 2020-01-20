.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

=======
行情API
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
  .. _getCodeChange: ../protocol/quote_protocol.html#qot-getcodechange-proto-3216
  .. _getIpoList: ../protocol/quote_protocol.html#qot-getipolist-proto-3217ipo
  .. _getFutureInfo: ../protocol/quote_protocol.html#qot-getfutureinfo-proto-3218
  .. _stockFilter: ../protocol/quote_protocol.html#qot-stockfilter-proto-3215
  .. _updateBasicQot: ../protocol/quote_protocol.html#qot-updatebasicqot-proto-3005
  .. _updateKL: ../protocol/quote_protocol.html#qot-updatekl-proto-3007k
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


FTAPI_Qot成员函数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+ FTAPI_Qot继承自\ `FTAPI_Conn <./Base_API.html#ftapi-conn>`_ ，连接层调用接口参考FTAPI_Conn说明。

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
getUserSecurity_                    获取自选股分组下的股票                              onReply_GetUserSecurity
modifyUserSecurity_                 修改自选股分组下的股票                              onReply_ModifyUserSecurity
getIpolist_                         获取ipo数据                                        onReply_GetIpoList
getCodeChange_                      获取股票代码变更                                    onReply_GetCodeChange
getFutureInfo_                      获取期货合约资料                                    onReply_GetFutureInfo
stockFilter_                        筛选股票                                           onReply_StockFilter
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
==================================    ================================================= 