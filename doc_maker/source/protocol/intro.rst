.. note::

  Futu OpenAPI 文档已于2020年10月16日全新升级，请移步至 `新文档 <https://openapi.futunn.com/futu-api-doc/>`_ 

  旧的github文档将不再更新，并于2020年11月16日正式停止访问。


.. _py-futu-api: ../api/intro.html


协议接口
====================
+ FutuOpenD是futu-api项目的网关客户端，在本机或云端成功运行后，第三方应用即可通过约定的TCP协议与之通讯，从而达到调用指定行情和交易接口的目的。
+ py-futu-api_ 可以简化在python编程上协议通讯的复杂度，其他语言接口正在陆续开发中……

--------------

  .. _nProtoFmtType: #id8
  .. _InitConnect: base_define.html#initconnect-proto-1001
  .. _InitConnect.proto: base_define.html#initconnect-proto-1001
  .. _GetGlobalState.proto:  base_define.html#getglobalstate-proto-1002
  .. _Notify.proto:  base_define.html#notify-proto-1003
  .. _KeepAlive.proto:  base_define.html#keepalive-proto-1004
  .. _KeepAlive:  base_define.html#keepalive-proto-1004
  
  .. _Trd_GetAccList.proto:  trade_protocol.html#trd-getacclist-proto-2001
  
  .. _Trd_UnlockTrade.proto:  trade_protocol.html#trd-unlocktrade-proto-2005
  .. _2005:  trade_protocol.html#trd-unlocktrade-proto-2005
  
  .. _Trd_SubAccPush.proto:  trade_protocol.html#trd-subaccpush-proto-2008
  .. _Trd_GetFunds.proto:  trade_protocol.html#trd-getfunds-proto-2101
  .. _2101: trade_protocol.html#trd-getfunds-proto-2101
  .. _Trd_GetPositionList.proto:  trade_protocol.html#trd-getpositionlist-proto-2102
  .. _2102:  trade_protocol.html#trd-getpositionlist-proto-2102
  
  .. _Trd_GetMaxTrdQtys.proto:  trade_protocol.html#trd-getmaxtrdqtys-proto-2111
  .. _2111:  trade_protocol.html#trd-getmaxtrdqtys-proto-2111
  
  .. _Trd_GetOrderList.proto:  trade_protocol.html#trd-getorderlist-proto-2201
  .. _2201:  trade_protocol.html#trd-getorderlist-proto-2201
  
  .. _Trd_PlaceOrder.proto:  trade_protocol.html#trd-placeorder-proto-2202
  .. _2202:  trade_protocol.html#trd-placeorder-proto-2202
  
  .. _Trd_ModifyOrder.proto:  trade_protocol.html#trd-modifyorder-proto-2205
  .. _2205:  trade_protocol.html#trd-modifyorder-proto-2205
  
  .. _Trd_UpdateOrder.proto:  trade_protocol.html#trd-updateorder-proto-2208
  
  .. _Trd_GetOrderFillList.proto:  trade_protocol.html#trd-getorderfilllist-proto-2211
  .. _2211:  trade_protocol.html#trd-getorderfilllist-proto-2211
  .. _Trd_UpdateOrderFill.proto:  trade_protocol.html#trd-updateorderfill-proto-2218
  
  .. _Trd_GetHistoryOrderList.proto:  trade_protocol.html#trd-gethistoryorderlist-proto-2221
  .. _2221:  trade_protocol.html#trd-gethistoryorderlist-proto-2221
  
  .. _Trd_GetHistoryOrderFillList.proto:  trade_protocol.html#trd-gethistoryorderfilllist-proto-2222
  .. _2222:  trade_protocol.html#trd-gethistoryorderfilllist-proto-2222
 
  .. _Qot_Sub.proto:  quote_protocol.html#qot-sub-proto-3001
  .. _3001:  quote_protocol.html#qot-sub-proto-3001
  .. _Qot_RegQotPush.proto:  quote_protocol.html#qot-regqotpush-proto-3002
  .. _Qot_GetSubInfo.proto:  quote_protocol.html#qot-getsubinfo-proto-3003
  .. _Qot_GetBasicQot.proto:  quote_protocol.html#qot-getbasicqot-proto-3004
  .. _Qot_UpdateBasicQot.proto:  quote_protocol.html#qot-updatebasicqot-proto-3005
  
  .. _Qot_GetKL.proto:  quote_protocol.html#qot-getkl-proto-3006k
  .. _3006:  quote_protocol.html#qot-getkl-proto-3006k
  .. _Qot_UpdateKL.proto:  quote_protocol.html#qot-updatekl-proto-3007k
  .. _Qot_GetRT.proto:  quote_protocol.html#qot-getrt-proto-3008
  .. _Qot_UpdateRT.proto:  quote_protocol.html#qot-updatert-proto-3009
  .. _Qot_GetTicker.proto:  quote_protocol.html#qot-getticker-proto-3010
  .. _3010:  quote_protocol.html#qot-getticker-proto-3010
  .. _Qot_UpdateTicker.proto:  quote_protocol.html#qot-updateticker-proto-3011
  .. _Qot_GetOrderBook.proto:  quote_protocol.html#qot-getorderbook-proto-3012
  .. _Qot_UpdateOrderBook.proto:  quote_protocol.html#qot-updateorderbook-proto-3013
  .. _Qot_GetBroker.proto:  quote_protocol.html#qot-getbroker-proto-3014
  .. _Qot_UpdateBroker.proto:  quote_protocol.html#qot-updatebroker-proto-3015
  
  .. _Qot_GetHistoryKL.proto:  quote_protocol.html#qot-gethistorykl-proto-3100k
  .. _Qot_GetHistoryKLPoints.proto:  quote_protocol.html#qot-gethistoryklpoints-proto-3101k
  .. _Qot_GetRehab.proto:  quote_protocol.html#qot-getrehab-proto-3102
  .. _Qot_RequestRehab.proto:  quote_protocol.html#qot-requestrehab-proto-3105
  .. _3105:  quote_protocol.html#qot-requestrehab-proto-3105
  
  .. _Qot_RequestHistoryKL.proto:  quote_protocol.html#qot-requesthistorykl-proto-3103k
  .. _3103:  quote_protocol.html#qot-requesthistorykl-proto-3103k
  
  .. _Qot_GetTradeDate.proto:  quote_protocol.html#qot-gettradedate-proto-3200
  .. _Qot_GetStaticInfo.proto:  quote_protocol.html#qot-getstaticinfo-proto-3202
  
  .. _Qot_GetSecuritySnapshot.proto:  quote_protocol.html#qot-getsecuritysnapshot-proto-3203
  .. _3203:  quote_protocol.html#qot-getsecuritysnapshot-proto-3203
  
  .. _Qot_GetPlateSet.proto:  quote_protocol.html#qot-getplateset-proto-3204
  .. _3204:  quote_protocol.html#qot-getplateset-proto-3204
  .. _Qot_GetPlateSecurity.proto:  quote_protocol.html#qot-getplatesecurity-proto-3205
  .. _3205:  quote_protocol.html#qot-getplatesecurity-proto-3205
  .. _Qot_GetReference.proto:  quote_protocol.html#qot-getreference-proto-3206
  .. _3206:  quote_protocol.html#qot-getreference-proto-3206
  .. _Qot_GetOwnerPlate.proto:  quote_protocol.html#qot-getownerplate-proto-3207
  .. _3207:  quote_protocol.html#qot-getownerplate-proto-3207
  .. _Qot_GetHoldingChangeList.proto:  quote_protocol.html#qot-getholdingchangelist-proto-3208
  .. _3208:  quote_protocol.html#qot-getholdingchangelist-proto-3208
  .. _Qot_GetOptionChain.proto:  quote_protocol.html#qot-getoptionchain-proto-3209
  .. _3209:  quote_protocol.html#qot-getoptionchain-proto-3209
  .. _SubType: base_define.html#subtype

  .. _Qot_GetWarrant.proto:  quote_protocol.html#qot-getwarrant-proto-3210
  .. _3210:  quote_protocol.html#qot-getwarrant-proto-3210

  .. _Qot_GetCapitalFlow.proto:  quote_protocol.html#qot-getcapitalflow-proto-3211
  .. _3211:  quote_protocol.html#qot-getcapitalflow-proto-3211
  .. _Qot_GetCapitalDistribution.proto:  quote_protocol.html#qot-getcapitaldistribution-proto-3212
  .. _3212:  quote_protocol.html#qot-getcapitaldistribution-proto-3212

  .. _Qot_GetUserSecurity.proto:  quote_protocol.html#qot-getusersecurity-proto-3213
  .. _3213:  quote_protocol.html#qot-getusersecurity-proto-3213
  
  .. _Qot_ModifyUserSecurity.proto:  quote_protocol.html#qot-modifyusersecurity-proto-3214
  .. _3214:  quote_protocol.html#qot-modifyusersecurity-proto-3214
  
  .. _Qot_StockFilter.proto:  quote_protocol.html#qot-stockfilter-proto-3215
  .. _3215:  quote_protocol.html#qot-stockfilter-proto-3215
  
  .. _Qot_GetIpoList.proto:  quote_protocol.html#qot-getipolist-proto-3217ipo
  .. _3217:  quote_protocol.html#qot-getipolist-proto-3217ipo

  .. _Qot_GetFutureInfo.proto:  quote_protocol.html#qot-getfutureinfo-proto-3218
  .. _3218:  quote_protocol.html#qot-getfutureinfo-proto-3218
  
  .. _Qot_RequestTradeDate.proto:  quote_protocol.html#qot-requesttradedate-proto-3219
  .. _3219:  quote_protocol.html#qot-requesttradedate-proto-3219

  .. _Qot_SetPriceReminder.proto:  quote_protocol.html#qot-setpricereminder-proto-3220
  .. _3220:  quote_protocol.html#qot-setpricereminder-proto-3220
  
  .. _Qot_GetPriceReminder.proto:  quote_protocol.html#qot-getpricereminder-proto-3221
  .. _3221:  quote_protocol.html#qot-getpricereminder-proto-3221
  
  .. _Qot_GetUserSecurityGroup.proto:  quote_protocol.html#qot-getusersecuritygroup-proto-3222
  .. _3222:  quote_protocol.html#qot-getusersecuritygroup-proto-3222
  
  .. _Qot_GetMarketState.proto:  quote_protocol.html#qot-getmarketstate-proto-3223
  .. _3223:  quote_protocol.html#qot-getmarketstate-proto-3223

  .. role:: red-strengthen

特点
-------

+ 基于TCP传输协议实现，稳定高效。
+ 支持protobuf/json两种协议格式， 灵活接入。
+ 协议设计支持加密、数据校验及回放功击保护，安全可靠。

 
---------------------------------------------------
 
协议清单
----------

 ==============   ==================================     ==================================================================
 协议ID           Protobuf文件                           说明
 ==============   ==================================     ==================================================================
 1001        	  InitConnect.proto_                      初始化连接
 1002             GetGlobalState.proto_                   获取全局状态 
 1003             Notify.proto_                           系统通知推送
 1004             KeepAlive.proto_                        保活心跳
 2001             Trd_GetAccList.proto_                   获取业务账户列表
 2005             Trd_UnlockTrade.proto_                  解锁或锁定交易
 2008             Trd_SubAccPush.proto_                   订阅业务账户的交易推送数据
 2101             Trd_GetFunds.proto_                     获取账户资金
 2102             Trd_GetPositionList.proto_              获取账户持仓
 2111             Trd_GetMaxTrdQtys.proto_                获取最大交易数量 
 2201             Trd_GetOrderList.proto_                 获取订单列表
 2202             Trd_PlaceOrder.proto_                   下单
 2205             Trd_ModifyOrder.proto_                  修改订单
 2208             Trd_UpdateOrder.proto_                  推送订单状态变动通知
 2211             Trd_GetOrderFillList.proto_             获取成交列表
 2218             Trd_UpdateOrderFill.proto_              推送成交通知
 2221             Trd_GetHistoryOrderList.proto_          获取历史订单列表
 2222             Trd_GetHistoryOrderFillList.proto_      获取历史成交列表
 3001             Qot_Sub.proto_                          订阅或者反订阅
 3002             Qot_RegQotPush.proto_                   注册推送
 3003             Qot_GetSubInfo.proto_                   获取订阅信息
 3004             Qot_GetBasicQot.proto_                  获取股票基本报价
 3005             Qot_UpdateBasicQot.proto_               推送股票基本报价
 3006             Qot_GetKL.proto_                        获取K线
 3007             Qot_UpdateKL.proto_                     推送K线
 3008             Qot_GetRT.proto_                        获取分时
 3009             Qot_UpdateRT.proto_                     推送分时
 3010             Qot_GetTicker.proto_                    获取逐笔
 3011             Qot_UpdateTicker.proto_                 推送逐笔
 3012             Qot_GetOrderBook.proto_                 获取买卖盘
 3013             Qot_UpdateOrderBook.proto_              推送买卖盘
 3014             Qot_GetBroker.proto_                    获取经纪队列
 3015             Qot_UpdateBroker.proto_                 推送经纪队列
 3100             Qot_GetHistoryKL.proto_                 从本地下载历史数据获取单只股票一段历史K线
 3101             Qot_GetHistoryKLPoints.proto_           从本地下载历史数据获取多只股票多点历史K线
 3102             Qot_GetRehab.proto_                     从本地下载历史数据获取复权信息
 3103             Qot_RequestHistoryKL.proto_             在线获取单只股票一段历史K线
 3105             Qot_RequestRehab.proto_             	  在线获取单只股票复权信息
 3200             Qot_GetTradeDate.proto_                 获取市场交易日
 3202             Qot_GetStaticInfo.proto_                获取股票静态信息
 3203             Qot_GetSecuritySnapshot.proto_          获取股票快照
 3204             Qot_GetPlateSet.proto_                  获取板块集合下的板块
 3205             Qot_GetPlateSecurity.proto_             获取板块下的股票 
 3206             Qot_GetReference.proto_                 获取正股相关股票 
 3207             Qot_GetOwnerPlate.proto_                获取股票所属板块
 3208             Qot_GetHoldingChangeList.proto_         获取持股变化列表
 3209             Qot_GetOptionChain.proto_               获取期权链
 3210             Qot_GetWarrant.proto_                   获取窝轮
 3211             Qot_GetCapitalFlow.proto_               获取资金流向
 3212             Qot_GetCapitalDistribution.proto_       获取资金分布
 3213             Qot_GetUserSecurity.proto_       		  获取自选股分组下的股票
 3214             Qot_ModifyUserSecurity.proto_       	  修改自选股分组下的股票
 3215             Qot_StockFilter.proto_                  获取条件选股
 3217             Qot_GetIpoList.proto_                   获取新股
 3218             Qot_GetFutureInfo.proto_                获取期货合约资料
 3219             Qot_RequestTradeDate.proto_             获取市场交易日，在线拉取不在本地计算
 3220             Qot_SetPriceReminder.proto_             设置到价提醒
 3221             Qot_GetPriceReminder.proto_             获取到价提醒 
 3222             Qot_GetUserSecurityGroup.proto_         获取自选股分组列表
 3223             Qot_GetMarketState.proto_               获取指定品种的市场状态  
 ==============   ==================================     ==================================================================

.. note::

    * 所有 Protobuf 文件可从 `futu-api <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb>`_ Python开源项目下获取

---------------------------------------------------
 
.. _quota-limit:

协议请求限制
---------------

API用户额度
~~~~~~~~~~~~~~~~~~~~~~
 
 部分协议额度限制划分如下：
 
	.. image:: ../_static/quota-table.png
	
 1. 总资产
 
 是指您在富途的所有资产，包括港美 A 股账户、Futu Inc. 账户、基金账户等，按照即期汇率换算成以港币为单位。

 2. 交易笔数

 会综合您当前自然月与上一自然月的交易情况，取您上个自然月的成交笔数与当前自然月的成交笔数的较大值进行计算，即：max (上个自然月的成交笔数，当前自然月的成交笔数)。

 3. 交易额

 是取您上个自然月的成交总金额与当前自然月的成交总金额的较大值进行计算，即：max（上个自然月的成交总金额，当前自然月的成交总金额），按照即期汇率换算成以港币为单位。其中，期货交易额的计算，需要乘以相应的调整系数（默认取0.1），期货交易额计算公式如下：
 
 期货交易额=∑（单笔成交数 * 成交价 * 合约乘数 * 汇率 * 调整系数）

 4. 订阅额度

 适用于订阅才可获取到的实时数据接口，每只股票订阅一个类型即占用 1 个订阅额度，取消订阅会释放已占用的额度。

 **举例：**

 假设您的订阅额度是 100。
 当您同时订阅了 HK.00700 的实时摆盘、US.AAPL 的实时逐笔、SH.600519 的实时报价时，此时订阅额度会占用 3 个，剩余的订阅额度为 97。
 这时，如果您取消了 HK.00700 的实时摆盘订阅，您的订阅额度占用将变成 2 个，剩余订阅额度会变成 98。
 
 5. 历史 K 线额度（30 天内）

 适用于历史 K 线接口。最近 30 天内，每请求 1 只股票的历史 K 线，将会占用 1 个历史 K 线额度。最近 30 天内重复请求同一只股票的历史 K 线，不会重复累计。

 **举例：**
 
 假设您的历史 K 线额度是 100，今天是 2020 年 7 月 5 日。
 您在 2020 年 6 月 5 日~2020 年 7 月 5 日之间，共计请求了 60 只股票的历史 K 线，则剩余的历史 K 线额度为 40。


.. _unlock-limit:

解锁或锁定交易
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

	* 协议ID: 2005_
	* :red-strengthen:`30` 秒内请求次数最多 :red-strengthen:`10` 次

.. _acctradinginfo-query-limit:

获取最大交易数量
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 协议ID: 2111_
	* :red-strengthen:`30` 秒内请求次数最多 :red-strengthen:`10` 次

.. _accinfo-query-limit:

获取账户资金
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 2101_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次
	* 仅当refreshCache为True时限制频率

.. _position-list-query-limit:

获取账户持仓
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 2102_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次
	* 仅当refreshCache为True时限制频率

.. _deal-list-query-limit:

获取成交列表
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 2211_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次
	* 仅当refreshCache为True时限制频率

.. _order-list-query-limit:

获取订单列表
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 2201_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次
	* 仅当refreshCache为True时限制频率
	
.. _place-order-limit:

下单
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 2202_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`15` 次，连续两次请求的间隔不可小于 :red-strengthen:`0.02` 秒。

.. _modify-order-limit:	
	
修改订单
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 2205_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`20` 次，连续两次请求的间隔不可小于 :red-strengthen:`0.02` 秒。
	
.. _history-order-list-query-limit:	

获取历史订单列表
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 2221_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次

.. _history-deal-list-query-limit:	

获取历史成交列表
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 2222_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次

.. _subscribe-limit:

订阅反订阅
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  * 请求协议ID: 3001_
  * 支持多种实时数据类型的定阅，参见 SubType_ , 每支股票订阅一个类型占用一个额度。
  * 额度现在限制请参见 :ref:`API用户额度 <quota-limit>`
  * 至少订阅一分钟才可以反订阅。
  * 由于港股 SF 行情摆盘数据量较大，为保证 SF 行情的速度和 OpenD 的处理性能，目前 SF 权限用户仅限同时订阅 50 只证券类产品（含 hkex 的正股、窝轮、牛熊）的摆盘（如需放开限制，请联系工作人员 https://help.futu5.com/faq/topic2348），剩余订阅额度仍可用于订阅其他类型，如：逐笔，买卖经纪等
  * 港股期权期货在LV1权限下，不支持订阅逐笔类型。

.. _get-cur-kline-limit:
	
获取K线
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3006_
	* 最多能获取最近 :red-strengthen:`1000` 根
	* 市盈率，换手率字段只有日K及日K以上周期的正股才有数据。

.. _get-rt-ticker-limit:
	
获取逐笔
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3010_
	* 最多能获取最近 :red-strengthen:`1000` 根
	* 港股期权期货在LV1权限下，不支持获取逐笔。

.. _request-history-kline-limit:

在线获取单只股票一段历史K线
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  * 请求协议ID: 3103_
  * 30天内在线获取历史K线最多可请求股票数请参见 :ref:`API用户额度 <quota-limit>`
  * :red-strengthen:`30` 秒内请求最多 :red-strengthen:`60` 次，可分页的请求，第1页限频，后续页请求不限频
  * 分K提供最近2年数据，日K及以上提供近10年数据。
  * 美股盘前和盘后仅支持 60 分钟及以下级别的K线。由于美股盘前和盘后时段为非常规交易时段，此时段的K线数据可能不足两年。
  
.. _get-rehab-limit:

在线获取单只股票复权信息
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  * 请求协议ID: 3105_
  * :red-strengthen:`30` 秒内请求最多 :red-strengthen:`60` 次

.. _get-market-snapshot-limit:

获取股票快照
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  * 请求协议ID: 3203_
  * 每次可请求股票数最多 :red-strengthen:`400` 个
  * 30秒内快照最多请求次数 :red-strengthen:`60` 次
  * 港股 BMP 权限下，单次请求的香港证券（含窝轮牛熊界内证）快照数量上限是 :red-strengthen:`20` 个
  * 港股期权期货 BMP 权限下，单次请求的香港期货和期权的快照数量上限是 :red-strengthen:`20` 个

.. _get-plate-list-limit:

获取板块集合下的板块
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3204_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次
	
.. _get-plate-stock-limit:	
	
获取板块下的股票
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3205_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次

.. _get-referencestock-list-limit:	
		
获取股票关联数据
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3206_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次
	* 查询相关窝轮不限频

.. _get-owner-plate-limit:	

获取股票所属板块
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3207_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次
	* 传入股票最多 :red-strengthen:`200` 个
	* 仅支持正股和指数

.. _get-holding-change-list-limit:	

获取持股变化列表
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3208_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次
	* 最多返回前 :red-strengthen:`100` 大股东的变化
	* 仅支持美股

.. _get-option-chain-limit:

获取期权链
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3209_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次
	* 传入时间跨度最多 :red-strengthen:`30` 天

.. _get-warrant-limit:
	
获取窝轮
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3210_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`60` 次
	* 每次请求的数据个数最多 :red-strengthen:`200` 个
	* 仅支持港股

.. _get-capital-flow-limit:

获取资金流向
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3211_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`30` 次
	* 仅支持正股、窝轮和基金
	
.. _get-capital-distribution-limit:

获取资金分布
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3212_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`30` 次
	* 仅支持正股、窝轮和基金

.. _get-user-security-limit:

获取自选股分组下的股票
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3213_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次
	* 不支持持仓（Positions），基金宝（Mutual Fund），外汇（Forex）的查询。

.. _modify-user-security-limit:
	
修改自选股分组下的股票
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3214_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次
	* 仅支持自定义分组
	* 自选股的数量是有上限的。
	* 如果有同名的分组，会返回自定义分组里面排序第一个分组的信息。

.. _get-stock-filter-limit:

获取条件选股
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3215_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次
	* 每次请求的数据个数最多 :red-strengthen:`200` 个
	* 建议筛选条件不超过 :red-strengthen:`250` 个，否则可能会出现“业务处理超时没返回”。
	* 累积属性的同一筛选条件数量最多 :red-strengthen:`10` 个
	* 自定义指标属性的同一类筛选条件数量最多 :red-strengthen:`10` 个

.. _get-ipo-list-limit:

获取IPO列表
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3217_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次
	
.. _get-future-info-limit:
	
获取期货合约资料
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3218_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`30` 次
	* 传入股票最多 :red-strengthen:`200` 个

.. _request-trading-day-limit:	

在线拉取市场交易日
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3219_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`30` 次

.. _set-price-reminder-limit:	

设置到价提醒
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3220_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`60` 次

.. _get-price-reminder-limit:
	
获取到价提醒列表
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3221_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次
	
.. _get-user-security-group-limit:
	
获取自选股分组列表
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3222_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次

.. _get-market-state-limit:
	
获取指定品种的市场状态的限制
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* 请求协议ID: 3223_
	* :red-strengthen:`30` 秒内请求最多 :red-strengthen:`10` 次
	* 每次请求的数据个数最多 :red-strengthen:`400` 个
	
协议请求流程 
-------------
	* 建立连接
	* 初始化连接
	* 请求数据或接收推送数据
	* 定时发送 KeepAlive_ 保持连接
	
.. image:: ../_static/proto.png

--------------

协议设计
---------
  协议数据包括协议头以及协议体，协议头固定字段，协议体根据具体协议决定。
  
协议头结构
~~~~~~~~~~~~~~~

.. code-block:: bash
    
	struct APIProtoHeader
	{
	    u8_t szHeaderFlag[2];
	    u32_t nProtoID;
	    u8_t nProtoFmtType;
	    u8_t nProtoVer;
	    u32_t nSerialNo;
	    u32_t nBodyLen;
	    u8_t arrBodySHA1[20];
	    u8_t arrReserved[8];
	};


==============   ==================================================================
字段             说明
==============   ==================================================================
szHeaderFlag     包头起始标志，固定为“FT”
nProtoID         协议ID
nProtoFmtType    协议格式类型，0为Protobuf格式，1为Json格式
nProtoVer        协议版本，用于迭代兼容, 目前填0
nSerialNo        包序列号，用于对应请求包和回包, 要求递增
nBodyLen         包体长度
arrBodySHA1      包体原始数据(解密后)的SHA1哈希值
arrReserved      保留8字节扩展
==============   ==================================================================

.. note::

    *   u8_t表示8位无符号整数，u32_t表示32位无符号整数
    *   FutuOpenD内部处理使用Protobuf，因此协议格式建议使用Protobuf，减少Json转换开销
    *   nProtoFmtType字段指定了包体的数据类型，回包会回对应类型的数据；推送协议数据类型由FutuOpenD配置文件指定
    *   **arrBodySHA1用于校验请求数据在网络传输前后的一致性，必须正确填入**
    *   **协议头的二进制流使用的是小端字节序，即一般不需要使用ntohl等相关函数转换数据**

---------------------------------------------------
	
协议体结构
~~~~~~~~~~~

**Protobuf协议请求包体结构**

.. code-block:: bash
    
	message C2S
	{
	    required int64 req = 1; 
	}

	message Request
	{
	    required C2S c2s = 1;
	}

**Protobuf协议回应包体结构**

.. code-block:: bash
	
	message S2C
	{
	    required int64 data = 1; 
	}

	message Response
	{
	    required int32 retType = 1 [default = -400]; //RetType,返回结果
	    optional string retMsg = 2;
	    optional int32 errCode = 3;
	    optional S2C s2c = 4;
	}

**Json协议请求包体结构**

.. code-block:: bash
	
	{
	    "c2s": 
	    {
	    	 "req": 0
	    }
	}

**Json协议回应包体结构**

.. code-block:: bash
	
	{
	    "retType" : 0
	    "retMsg" : ""
	    "errCode" : 0
	    "s2c": 
	    {
	        "data": 0
	    }
	}

---------

==============   ==================================================================
字段             说明
==============   ==================================================================
c2s              请求参数结构
req              请求参数，实际根据协议定义
retType          请求结果
retMsg           若请求失败，说明失败原因
errCode          若请求失败对应错误码
s2c              回应数据结构，部分协议不返回数据则无该字段
data             回应数据，实际根据协议定义
==============   ==================================================================
 
.. note::

	*  包体格式类型请求包由协议头 nProtoFmtType_ 指定， FutuOPenD主动推送格式参见 `FutuOpenD配置 <https://futunnopen.github.io/py-futu-api/setup/FutuOpenDGuide.html#id5>`_ 约定的 “push_proto_type“ 配置项
	*  原始协议文件格式是以Protobuf格式定义，若需要json格式传输，建议使用protobuf3的接口直接转换成json
	*  枚举值字段定义使用有符号整形，注释指明对应枚举，枚举一般定义于Common.proto，Qot_Common.proto，Trd_Common.proto文件中
	*  **协议中价格、百分比等数据用浮点类型来传输，直接使用会有精度问题，需要根据精度（如协议中未指明，默认小数点后三位）做四舍五入之后再使用**
	
---------------------------------------------------

加密通信流程
~~~~~~~~~~~~~~~

  * 若FutuOpenD配置了加密, InitConnect_ 初始化连接协议必须使用RSA公钥加密，后续其他协议使用 InitConnect_ 返回的随机密钥进行AES加密通信。
  * FutuOpenD的加密流程借鉴了SSL协议，但考虑到一般是本地部署服务和应用，简化了相关流程，FutuOpenD与接入Client共用了同一个RSA 私钥文件，请妥善保存和分发私钥文件。
  * 可到"http://web.chacuo.net/netrsakeypair"这个网址在线生成随机RSA密钥对，密钥格式必须为PCKS#1，密钥长度512，1024都可以，不要设置密码，将生成的私钥复制保存到文件中，然后将私钥文件路径配置到 `FutuOpenD配置 <https://futunnopen.github.io/py-futu-api/setup/FutuOpenDGuide.html#id5>`_ 约定的 “rsa_private_key”配置项中 
  * 
  * **强烈建议有实盘交易的用户配置加密，避免账户和交易信息泄露**
  
  .. image:: ../_static/encrypt.png
  
	
---------------------------------------------------

RSA加解密
~~~~~~~~~~~~~~~~~~~
	* `FutuOpenD配置 <https://futunnopen.github.io/py-futu-api/setup/FutuOpenDGuide.html#id5>`_ 约定"rsa_private_key"为私钥文件路径
	* FutuOpenD 与接入客户端共用相同的私钥文件
	* RSA加解密仅用于 InitConnect_ 请求，用于安全获取其它请求协议的对称加密Key
	* FutuOpenD的RSA密钥为1024位, 填充方式PKCS1, 公钥加密，私钥解密，公钥可通过私钥生成
	* Python API 参考实现: `RsaCrypt <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/sys_config.py>`_  类的encrypt / decrypt 接口
	

 **发送数据加密**

  * RSA加密规则:若密钥位数是key_size, 单次加密串的最大长度为 (key_size)/8 - 11, 目前位数1024, 一次加密长度可定为100
  
  * 将明文数据分成一个或数个最长100字节的小段进行加密，拼接分段加密数据即为最终的Body加密数据
  
 **接收数据解密** 

	* RSA解密同样遵循分段规则，对于1024位密钥, 每小段待解密数据长度为128字节
	
	* 将密文数据分成一个或数个128字节长的小段进行解密，拼接分段解密数据即为最终的Body解密数据
	
	
-------------------------------------------------------------


AES加解密
~~~~~~~~~~~~~~~~~~~
	* 加密key由 InitConnect_ 协议返回
	* 使用的是AES的ecb加密模式。
	* Python API 参考实现: `FutuConnMng <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/conn_mng.py>`_  类的encrypt_conn_data / decrypt_conn_data 接口
	
 **发送数据加密**

  * AES加密要求源数据长度必须是16的整数倍,  故需补‘\0'对齐后再加密，记录mod_len为源数据长度与16取模值

  * 因加密前有可能对源数据作修改， 故需在加密后的数据尾再增加一个16字节的填充数据块，其最后一个字节赋值mod_len, 其余字节赋值'\0'， 将加密数据和额外的填充数据块拼接作为最终要发送协议的body数据

 **接收数据解密**

  * 协议body数据, 先将最后一个字节取出，记为mod_len， 然后将body截掉尾部16字节填充数据块后再解密（与加密填充额外数据块逻辑对应）

  * mod_len 为0时，上述解密后的数据即为协议返回的body数据, 否则需截掉尾部(16 - mod_len)长度的用于填充对齐的数据

  .. image:: ../_static/AES.png
