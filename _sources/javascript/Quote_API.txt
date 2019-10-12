
.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

=======
行情API
=======


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
  .. _GetRehab: ../protocol/quote_protocol.html#qot-getrehab-proto-3102
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
  .. _Notify: ../protocol/base_define.html#notify-proto-1003
  .. _GetCodeChange: ../protocol/quote_protocol.html#qot-getcodechange-proto-3216
  .. _GetIpoList: ../protocol/quote_protocol.html#qot-getipolist-proto-3217ipo
  .. _UpdateBasicQot: ../protocol/quote_protocol.html#qot-updatebasicqot-proto-3005
  .. _UpdateKL: ../protocol/quote_protocol.html#qot-updatekl-proto-3007k
  .. _UpdateRT: ../protocol/quote_protocol.html#qot-updatert-proto-3009
  .. _UpdateTicker: ../protocol/quote_protocol.html#qot-updateticker-proto-3011
  .. _UpdateOrderBook: ../protocol/quote_protocol.html#qot-updateorderbook-proto-3013
  .. _UpdateBroker: ../protocol/quote_protocol.html#qot-updatebroker-proto-3015
  
---------------------------------------------------


功能函数
-------------------

================================    ==============================================
函数名（点开链接可查看具体协议）        功能简介
================================    ==============================================
GetGlobalState_                     获取全局状态
Sub_                                订阅或者反订阅
RegQotPush_                         注册推送
GetSubInfo_                         获取订阅信息
GetTicker_                          获取逐笔,调用该接口前需要先订阅
GetBasicQot_                        获取基本行情,调用该接口前需要先订阅
GetOrderBook_                       获取摆盘,调用该接口前需要先订阅
GetKL_                              获取K线，调用该接口前需要先订阅
GetRT_                              获取分时，调用该接口前需要先订阅
GetBroker_                          获取经纪队列，调用该接口前需要先订阅
RequestRehab_                       在线请求历史复权信息，不读本地历史数据DB
RequestHistoryKL_                   在线请求历史K线，不读本地历史数据DB
RequestHistoryKLQuota_              获取历史K线已经用掉的额度
GetTradeDate_                       获取交易日
GetStaticInfo_                      获取静态信息
GetSecuritySnapshot_                获取股票快照
GetPlateSet_                        获取板块集合下的板块
GetPlateSecurity_                   获取板块下的股票
GetReference_                       获取相关股票
GetOwnerPlate_                      获取股票所属的板块
GetHoldingChangeList_               获取大股东持股变化列表
GetOptionChain_                     筛选期权
GetWarrant_                         筛选窝轮
GetCapitalFlow_                     获取资金流向
GetCapitalDistribution_             获取资金分布
GetUserSecurity_                    获取自选股分组下的股票
ModifyUserSecurity_                 修改自选股分组下的股票
GetIpolist_                         获取ipo数据
GetCodeChange_                      获取股票代码变更
================================    ==============================================


行情推送接收接口函数
-----------------------------

.. code-block:: js

    this.websocket.onPush = onPush;

    function onPush(cmd, response) {
          const obj = ftWebsocket.findCmdObj(cmd);
          //通过cmd来区分具体的推送协议
          if (obj && obj.description) {
            console.log(obj.description, response);
          }
        },




