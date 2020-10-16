.. note::

  Futu OpenAPI 文档已于2020年10月16日全新升级，请移步至 `新文档 <https://openapi.futunn.com/futu-api-doc/>`_ 

  旧的github文档将不再更新，并于2020年11月16日正式停止访问。


.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

==========
行情API
==========

--------------

  .. _GetGlobalState: ../protocol/base_define.html#getglobalstate-proto-1002
  .. _Sub: ../protocol/quote_protocol.html#qot-sub-proto-3001
  .. _RegQotPush: ../protocol/quote_protocol.html#qot-regqotpush-proto-3002
  .. _GetSubInfo: ../protocol/quote_protocol.html#qot-getsubinfo-proto-3003
  .. _GetTicker: ../protocol/quote_protocol.html#qot-getticker-proto-3010
  .. _GetBasicQot: ../protocol/quote_protocol.html#qot-getbasicqot-proto-3004
  .. _GetOrderBook: ../protocol/quote_protocol.html#qot-getorderbook-proto-3012
  .. _GetKL: ../protocol/quote_protocol.html#qot-getkl-proto-3006k
  .. _GetRT: ../protocol/quote_protocol.html#qot-getrt-proto-3008
  .. _GetBroker: ../protocol/quote_protocol.html#qot-getbroker-proto-3014
  .. _RequestRehab: ../protocol/quote_protocol.html#qot-requestrehab-proto-3105
  .. _RequestHistoryKL: ../protocol/quote_protocol.html#qot-requesthistorykl-proto-3103k
  .. _RequestHistoryKLQuota: ../protocol/quote_protocol.html#qot-requesthistoryklquota-proto-3104k
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
  .. _GetCodeChange: ../protocol/quote_protocol.html#qot-getcodechange-proto-3216
  .. _GetIpoList: ../protocol/quote_protocol.html#qot-getipolist-proto-3217ipo
  .. _GetFutureInfo: ../protocol/quote_protocol.html#qot-getfutureinfo-proto-3218
  .. _RequestTradeDate: ../protocol/quote_protocol.html#qot-requesttradedate-proto-3219
  .. _SetPriceReminder: ../protocol/quote_protocol.html#qot-setpricereminder-proto-3220
  .. _GetPriceReminder: ../protocol/quote_protocol.html#qot-getpricereminder-proto-3221
  .. _OnPush_Notify: ../protocol/base_define.html#notify-proto-1003
  .. _OnPush_UpdateBasicQot: ../protocol/quote_protocol.html#qot-updatebasicqot-proto-3005
  .. _OnPush_UpdateKL: ../protocol/quote_protocol.html#qot-updatekl-proto-3007k
  .. _OnPush_UpdateRT: ../protocol/quote_protocol.html#qot-updatert-proto-3009
  .. _OnPush_UpdateTicker: ../protocol/quote_protocol.html#qot-updateticker-proto-3011
  .. _OnPush_UpdateOrderBook: ../protocol/quote_protocol.html#qot-updateorderbook-proto-3013
  .. _OnPush_UpdateBroker: ../protocol/quote_protocol.html#qot-updatebroker-proto-3015
  .. _OnPush_UpdatePriceReminder: ../protocol/quote_protocol.html#qot-updatepricereminder-proto-3019
---------------------------------------------------
主要函数列表
---------------

FTAPI_Qot成员函数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ FTAPI_Qot继承自\ `FTAPI_Conn <./base.html#ftapi-conn>`_ ，连接层调用接口参考FTAPI_Conn说明。

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
GetIpolist_                         获取ipo数据                                        OnReply_GetIpolist
GetCodeChange_                      获取股票代码变更                                   OnReply_GetCodeChange
GetFutureInfo_                      获取期货合约资料                                   OnReply_GetFutureInfo
RequestTradeDate_                   获取市场交易日，在线拉取不在本地计算               OnReply_RequestTradeDate
SetPriceReminder_                   设置到价提醒                                       OnReply_SetPriceReminder
GetPriceReminder_                   获取到价提醒                                       OnReply_GetPriceReminder
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
OnPush_UpdatePriceReminder_            推送到价提醒
==================================    ================================================= 

