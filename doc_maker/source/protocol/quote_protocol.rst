.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

行情协议
==========
	这里对FutuOpenD开放的行情协议接口作出归档说明。

.. note::

    *   为避免增删导致的版本兼容问题，所有enum枚举类型只用于值的定义，在protobuf结构体中声明类型时使用int32类型
    *   所有类型定义使用protobuf格式声明，不同语言对接时请自行通过相关工具转换成对应的头文件
    *   XXX.proto表示协议文件名, 点击超链接可打开github上的协议文件，每条协议内容以github上的为准，此文档更新可能存在滞后

--------------

`Qot_Sub.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_Sub.proto>`_ - 3001订阅或者反订阅
---------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_Sub;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		repeated Qot_Common.Security securityList = 1; //股票
		repeated int32 subTypeList = 2; //Qot_Common.SubType,订阅数据类型
		required bool isSubOrUnSub = 3; //true表示订阅,false表示反订阅
		optional bool isRegOrUnRegPush = 4; //是否注册或反注册该连接上面行情的推送,该参数不指定不做注册反注册操作
		repeated int32 regPushRehabTypeList = 5; //Qot_Common.RehabType,复权类型,注册推送并且是K线类型才生效,其他订阅类型忽略该参数,注册K线推送时该参数不指定默认前复权
		optional bool isFirstPush = 6; //注册后如果本地已有数据是否首推一次已存在数据,该参数不指定则默认true
		optional bool isUnsubAll = 7; //一键取消当前连接的所有订阅,当被设置为True时忽略其他参数。
		optional bool isSubOrderBookDetail = 8; //订阅摆盘可用,是否订阅摆盘明细,仅支持SF行情,该参数不指定则默认false
	}

	message S2C
	{
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
				
		optional S2C s2c = 4;
	}
	
.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 订阅数据类型参考 `SubType <base_define.html#subtype>`_
	* 复权类型参考 `RehabType <base_define.html#rehabtype-k>`_
	* 为控制定阅产生推送数据流量，股票定阅总量有额度控制，订阅规则参考限制条件 :ref:`订阅反订阅 <subscribe-limit>`
	* 高频数据接口需要订阅之后才能使用，注册推送之后才可以收到数据更新推送
	
-------------------------------------

`Qot_RegQotPush.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_RegQotPush.proto>`_ - 3002注册行情推送
------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_RegQotPush;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		repeated Qot_Common.Security securityList = 1; //股票
		repeated int32 subTypeList = 2; //Qot_Common.SubType,要注册到该连接的订阅类型
		repeated int32 rehabTypeList = 3; //Qot_Common.RehabType,复权类型,注册K线类型才生效,其他订阅类型忽略该参数,注册K线时该参数不指定默认前复权
		required bool isRegOrUnReg = 4; //注册或取消
		optional bool isFirstPush = 5; //注册后如果本地已有数据是否首推一次已存在数据,该参数不指定则默认true
	}

	message S2C
	{
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;

		optional S2C s2c = 4;
	}
	
.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 订阅数据类型参考 `SubType <base_define.html#subtype>`_
	* 复权类型参考 `RehabType <base_define.html#rehabtype-k>`_
	* 行情需要订阅成功才能注册推送
	
-------------------------------------

`Qot_GetSubInfo.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetSubInfo.proto>`_ - 3003获取订阅信息
---------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetSubInfo;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		optional bool isReqAllConn = 1; //是否返回所有连接的订阅状态,不传或者传false只返回当前连接数据
	}

	message S2C
	{
		repeated Qot_Common.ConnSubInfo connSubInfoList = 1; //订阅订阅信息
		required int32 totalUsedQuota = 2; //FutuOpenD已使用的订阅额度
		required int32 remainQuota = 3; //FutuOpenD剩余订阅额度
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}
	
.. note::
	
	* 订阅信息结构参考 `ConnSubInfo <base_define.html#connsubinfo>`_
	
-------------------------------------

`Qot_GetBasicQot.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetBasicQot.proto>`_ - 3004获取股票基本行情
---------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetBasicQot;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		repeated Qot_Common.Security securityList = 1; //股票
	}

	message S2C
	{
		repeated Qot_Common.BasicQot basicQotList = 1; //股票基本报价
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}
	
.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 基本报价结构参考 `BasicQot <base_define.html#basicqot>`_
	
-------------------------------------

`Qot_UpdateBasicQot.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_UpdateBasicQot.proto>`_ - 3005推送股票基本报价
-------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_UpdateBasicQot;

	import "Common.proto";
	import "Qot_Common.proto";

	message S2C
	{
		repeated Qot_Common.BasicQot basicQotList = 1; //股票基本行情
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}
	
.. note::
	
	* 基本报价结构参考 `BasicQot <base_define.html#basicqot>`_
	
-------------------------------------

`Qot_GetKL.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetKL.proto>`_ - 3006获取K线
------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetKL;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required int32 rehabType = 1; //Qot_Common.RehabType,复权类型
		required int32 klType = 2; //Qot_Common.KLType,K线类型
		required Qot_Common.Security security = 3; //股票
		required int32 reqNum = 4; //请求K线根数
	}

	message S2C
	{
		required Qot_Common.Security security = 1; //股票
		repeated Qot_Common.KLine klList = 2; //k线点
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;

		optional S2C s2c = 4;
	}
	
.. note::
	
	* 复权类型参考 `RehabType <base_define.html#rehabtype-k>`_
	* K线类型参考 `KLType <base_define.html#kltype-k>`_
	* 股票结构参考 `Security <base_define.html#security>`_
	* K线结构参考 `KLine <base_define.html#kline-k>`_
	* 请求K线目前最多最近1000根
	
-------------------------------------

`Qot_UpdateKL.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_UpdateKL.proto>`_ - 3007推送K线
-------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_UpdateKL;

	import "Common.proto";
	import "Qot_Common.proto";

	message S2C
	{
		required int32 rehabType = 1; //Qot_Common.RehabType,复权类型
		required int32 klType = 2; //Qot_Common.KLType,K线类型
		required Qot_Common.Security security = 3; //股票
		repeated Qot_Common.KLine klList = 4; //推送的k线点
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}
	
.. note::
	
	* 复权类型参考 `RehabType <base_define.html#rehabtype-k>`_
	* K线类型参考 `KLType <base_define.html#kltype-k>`_
	* 股票结构参考 `Security <base_define.html#security>`_
	* K线结构参考 `KLine <base_define.html#kline-k>`_
	
-------------------------------------

`Qot_GetRT.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetRT.proto>`_ - 3008获取分时
------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetRT;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required Qot_Common.Security security = 1; //股票
	}

	message S2C
	{
		required Qot_Common.Security security = 1; //股票
		repeated Qot_Common.TimeShare rtList = 2; //分时点
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}
	
.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 分时结构参考 `TimeShare <base_define.html#timeshare>`_
	
-------------------------------------

`Qot_UpdateRT.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_UpdateRT.proto>`_ - 3009推送分时
-------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_UpdateRT;

	import "Common.proto";
	import "Qot_Common.proto";

	message S2C
	{
		required Qot_Common.Security security = 1;
		repeated Qot_Common.TimeShare rtList = 2; //推送的分时点
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}
	
.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 分时结构参考 `TimeShare <base_define.html#timeshare>`_
	
-------------------------------------

`Qot_GetTicker.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetTicker.proto>`_ - 3010获取逐笔
---------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetTicker;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required Qot_Common.Security security = 1; //股票
		required int32 maxRetNum = 2; //最多返回的逐笔个数,实际返回数量不一定会返回这么多,最多返回1000个
	}

	message S2C
	{
		required Qot_Common.Security security = 1; //股票
		repeated Qot_Common.Ticker tickerList = 2; //逐笔
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		optional S2C s2c = 4;
	}
	
.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 逐笔结构参考 `Ticker <base_define.html#ticker>`_
	* 请求逐笔目前最多最近1000个
	
-------------------------------------

`Qot_UpdateTicker.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_UpdateTicker.proto>`_ - 3011推送逐笔
---------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_UpdateTicker;

	import "Common.proto";
	import "Qot_Common.proto";

	message S2C
	{
		required Qot_Common.Security security = 1; //股票
		repeated Qot_Common.Ticker tickerList = 2; //逐笔
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}
	
.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 逐笔结构参考 `Ticker <base_define.html#ticker>`_
-------------------------------------

`Qot_GetOrderBook.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetOrderBook.proto>`_ - 3012获取买卖盘
---------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetOrderBook;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required Qot_Common.Security security = 1; //股票
		required int32 num = 2; //请求的摆盘个数
	}

	message S2C
	{
		required Qot_Common.Security security = 1; //股票
		repeated Qot_Common.OrderBook orderBookAskList = 2; //卖盘
		repeated Qot_Common.OrderBook orderBookBidList = 3; //买盘
		optional string svrRecvTimeBid = 4; // 富途服务器从交易所收到数据的时间(for bid)部分数据的接收时间为零，例如服务器重启或第一次推送的缓存数据。该字段暂时只支持港股。
		optional double svrRecvTimeBidTimestamp = 5; // 富途服务器从交易所收到数据的时间戳(for bid)
		optional string svrRecvTimeAsk = 6; // 富途服务器从交易所收到数据的时间(for ask)
		optional double svrRecvTimeAskTimestamp = 7; // 富途服务器从交易所收到数据的时间戳(for ask)
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		optional S2C s2c = 4;
	}

.. note::

	* 股票结构参考 `Security <base_define.html#security>`_
	* 买卖盘结构参考 `OrderBook <base_define.html#orderbook>`_
	* 富途服务器从交易所收到数据的时间字段，仅支持港股正股和窝轮，且仅开盘时间才有此数据。
	* 富途服务器从交易所收到数据的时间字段，部分数据的接收时间为零，例如服务器重启或第一次推送的缓存数据。
	
-------------------------------------

`Qot_UpdateOrderBook.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_UpdateOrderBook.proto>`_ - 3013推送买卖盘
---------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_UpdateOrderBook;

	import "Common.proto";
	import "Qot_Common.proto";

	message S2C
	{
		required Qot_Common.Security security = 1; //股票
		repeated Qot_Common.OrderBook orderBookAskList = 2; //卖盘
		repeated Qot_Common.OrderBook orderBookBidList = 3; //买盘
		optional string svrRecvTimeBid = 4; // 富途服务器从交易所收到数据的时间(for bid)部分数据的接收时间为零，例如服务器重启或第一次推送的缓存数据。该字段暂时只支持港股。
		optional double svrRecvTimeBidTimestamp = 5; // 富途服务器从交易所收到数据的时间戳(for bid)
		optional string svrRecvTimeAsk = 6; // 富途服务器从交易所收到数据的时间(for ask)
		optional double svrRecvTimeAskTimestamp = 7; // 富途服务器从交易所收到数据的时间戳(for ask)
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}

.. note::

	* 股票结构参考 `Security <base_define.html#security>`_
	* 买卖盘结构参考 `OrderBook <base_define.html#orderbook>`_
	
-------------------------------------

`Qot_GetBroker.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetBroker.proto>`_ - 3014获取经纪队列
---------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetBroker;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required Qot_Common.Security security = 1; //股票
		optional int32 num = 2; //请求的经纪个数
	}

	message S2C
	{
		required Qot_Common.Security security = 1; //股票
		repeated Qot_Common.Broker brokerAskList = 2; //经纪Ask(卖)盘
		repeated Qot_Common.Broker brokerBidList = 3; //经纪Bid(买)盘
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		optional S2C s2c = 4;
	}
	
.. note::

	* 股票结构参考 `Security <base_define.html#security>`_
	* 经纪队列结构参考 `Broker <base_define.html#broker>`_
-------------------------------------

`Qot_UpdateBroker.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_UpdateBroker.proto>`_ - 3015推送经纪队列
---------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_UpdateBroker;

	import "Common.proto";
	import "Qot_Common.proto";

	message S2C
	{
		required Qot_Common.Security security = 1; //股票
		repeated Qot_Common.Broker brokerAskList = 2; //经纪Ask(卖)盘
		repeated Qot_Common.Broker brokerBidList = 3; //经纪Bid(买)盘
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}
	
.. note::

	* 股票结构参考 `Security <base_define.html#security>`_
	* 经纪队列结构参考 `Broker <base_define.html#broker>`_	
-------------------------------------



`Qot_RequestRehab.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_RequestRehab.proto>`_ - 3105在线获取单只股票复权信息
--------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_RequestRehab;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required Qot_Common.Security security = 1; //股票
	}

	message S2C
	{
		repeated Qot_Common.Rehab rehabList = 1; //复权信息
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}

.. note::

	* 股票结构参考 `Security <base_define.html#security>`_
	* 复权结构参考 `Rehab <base_define.html#rehab>`_
	* 限频接口：30秒内最多60次
	
-------------------------------------

`Qot_RequestHistoryKL.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_RequestHistoryKL.proto>`_ - 3103在线获取单只股票一段历史K线
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_RequestHistoryKL;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required int32 rehabType = 1; //Qot_Common.RehabType,复权类型
		required int32 klType = 2; //Qot_Common.KLType,K线类型
		required Qot_Common.Security security = 3; //股票市场以及股票代码
		required string beginTime = 4; //开始时间字符串
		required string endTime = 5; //结束时间字符串
		optional int32 maxAckKLNum = 6; //最多返回多少根K线，如果未指定表示不限制
		optional int64 needKLFieldsFlag = 7; //指定返回K线结构体特定某几项数据，KLFields枚举值或组合，如果未指定返回全部字段
		optional bytes nextReqKey = 8; //分页请求key
		optional bool extendedTime = 9;  //是否获取美股盘前盘后数据，当前仅支持1分k。
	}

	message S2C
	{
		required Qot_Common.Security security = 1;
		repeated Qot_Common.KLine klList = 2; //K线数据
		optional bytes nextReqKey = 3; //分页请求key。一次请求没有返回所有数据时，下次请求带上这个key，会接着请求
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}

.. note::
	
	* 复权类型参考 `RehabType <base_define.html#rehabtype-k>`_
	* K线类型参考 `KLType <base_define.html#kltype-k>`_
	* 股票结构参考 `Security <base_define.html#security>`_
	* K线结构参考 `KLine <base_define.html#kline-k>`_
	* K线字段类型参考 `KLFields <base_define.html#klfields-k>`_
	* 请求最大个数参考OpenAPI用户等级权限
	* 分页请求的key。如果start和end之间的数据点多于max_count，那么后续请求时，要传入上次调用返回的page_req_key。初始请求时应该传None。

-------------------------------------

`Qot_GetTradeDate.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetTradeDate.proto>`_ - :strike:`3200获取市场交易日`
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetTradeDate;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required int32 market = 1; //Qot_Common.QotMarket,股票市场
		required string beginTime = 2; //开始时间字符串
		required string endTime = 3; //结束时间字符串
	}

	message TradeDate
	{
		required string time = 1; //时间字符串
		optional double timestamp = 2; //时间戳
		optional int32 tradeDateType = 3; //Qot_Common.TradeDateType,交易时间类型
	}

	message S2C
	{
		repeated TradeDate tradeDateList = 1; //交易日,注意该交易日是通过自然日去除周末以及节假日得到，不包括临时休市数据
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}

.. note::

	* 市场类型参考 `QotMarket <base_define.html#qotmarket>`_
	* 交易时间类型参考 `TradeDateType <base_define.html#tradedatetype>`_

-------------------------------------

`Qot_GetStaticInfo.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetStaticInfo.proto>`_ - 3202获取股票静态信息
------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetStaticInfo;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		optional int32 market = 1; //Qot_Common.QotMarket,股票市场
		optional int32 secType = 2; //Qot_Common.SecurityType,股票类型
		repeated Qot_Common.Security securityList = 3; //股票，若该字段存在，忽略其他字段，只返回该字段股票的静态信息
		//当传入程序无法识别的股票时（包括很久之前退市的股票和不存在的股票），仍然返回股票信息，用静态信息标志来该股票不存在。
	}

	message S2C
	{
		repeated Qot_Common.SecurityStaticInfo staticInfoList = 1; //静态信息
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}
	
.. note::

	* 股票结构参考 `Security <base_define.html#security>`_
	* 市场类型参考 `QotMarket <base_define.html#qotmarket>`_
	* 股票静态信息结构参考 `SecurityStaticInfo <base_define.html#securitystaticbasic>`_
	
-------------------------------------

`Qot_GetSecuritySnapshot.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetSecuritySnapshot.proto>`_ - 3203获取股票快照
--------------------------------------------------------------------------------------------------------------------------------------------------------------------
.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetSecuritySnapshot;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		repeated Qot_Common.Security securityList = 1; //股票
	}

	 // 正股类型额外数据
	message EquitySnapshotExData
	{
		required int64 issuedShares = 1; // 发行股本,即总股本
		required double issuedMarketVal = 2; // 总市值 =总股本*当前价格（单位：元）
		required double netAsset = 3; // 资产净值
		required double netProfit = 4; // 盈利（亏损）
		required double earningsPershare = 5; // 每股盈利
		required int64 outstandingShares = 6; // 流通股本
		required double outstandingMarketVal = 7; // 流通市值 =流通股本*当前价格（单位：元）
		required double netAssetPershare = 8; // 每股净资产
		required double eyRate = 9; // 收益率（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		required double peRate = 10; // 市盈率
		required double pbRate = 11; // 市净率
		required double peTTMRate = 12; // 市盈率TTM
		optional double dividendTTM = 13; // 股息TTM，派息
		optional double dividendRatioTTM = 14; // 股息率TTM（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		optional double dividendLFY = 15; // 股息LFY，上一年度派息
		optional double dividendLFYRatio = 16; // 股息率LFY（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
	}

	 // 窝轮类型额外数据
	message WarrantSnapshotExData
	{
		required double conversionRate = 1; //换股比率
		required int32 warrantType = 2; //Qot_Common.WarrantType,窝轮类型
		required double strikePrice = 3; //行使价
		required string maturityTime = 4; //到期日时间字符串
		required string endTradeTime = 5; //最后交易日时间字符串
		required Qot_Common.Security owner = 6; //所属正股 
		required double recoveryPrice = 7; //收回价,仅牛熊证支持该字段
		required int64 streetVolumn = 8; //街货量
		required int64 issueVolumn = 9; //发行量
		required double streetRate = 10; //街货占比（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		required double delta = 11; //对冲值,仅认购认沽支持该字段
		required double impliedVolatility = 12; //引申波幅,仅认购认沽支持该字段
		required double premium = 13; //溢价（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		optional double maturityTimestamp = 14; //到期日时间戳
		optional double endTradeTimestamp = 15; //最后交易日时间戳
		optional double leverage = 16;  // 杠杆比率（倍）
		optional double ipop = 17; // 价内/价外（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		optional double breakEvenPoint = 18; // 打和点
		optional double conversionPrice = 19;  // 换股价
		optional double priceRecoveryRatio = 20; // 正股距收回价（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		optional double score = 21; // 综合评分
		optional double upperStrikePrice = 22; //上限价，仅界内证支持该字段
		optional double lowerStrikePrice = 23; //下限价，仅界内证支持该字段
		optional int32 inLinePriceStatus = 24; //Qot_Common.PriceType, 界内界外，仅界内证支持该字段
		optional string issuerCode = 25; //发行人代码
	}

	 // 期权类型额外数据
	message OptionSnapshotExData
	{
		required int32 type = 1; //Qot_Common.OptionType,期权
		required Qot_Common.Security owner = 2; //标的股
		required string strikeTime = 3; //行权日
		required double strikePrice = 4; //行权价
		required int32 contractSize = 5; //每份合约数
		required int32 openInterest = 6; //未平仓合约数
		required double impliedVolatility = 7; //隐含波动率（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		required double premium = 8; //溢价（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		required double delta = 9; //希腊值 Delta
		required double gamma = 10; //希腊值 Gamma
		required double vega = 11; //希腊值 Vega
		required double theta = 12; //希腊值 Theta
		required double rho = 13; //希腊值 Rho
		optional double strikeTimestamp = 14; //行权日时间戳  
		
		//以下字段仅支持港股期权
		optional int32 indexOptionType = 15; //Qot_Common.IndexOptionType, 指数期权的类型，仅在指数期权有效
		optional int32 netOpenInterest = 16; //净未平仓合约数
		optional int32 expiryDateDistance = 17; //距离到期日天数
		optional double contractNominalValue = 18; //合约名义金额
		optional double ownerLotMultiplier = 19; //相等正股手数，指数期权无该字段
		optional int32 optionAreaType = 20; //OptionAreaType, 期权类型（按行权时间）
		optional double contractMultiplier = 21; //合约乘数，指数期权特有字段
	}

	// 指数类型额外数据
	message IndexSnapshotExData
	{
		required int32 raiseCount = 1;  // 上涨支数
		required int32 fallCount = 2;  // 下跌支数
		required int32 equalCount = 3;  // 平盘支数
	}

	// 板块类型额外数据
	message PlateSnapshotExData
	{
		required int32 raiseCount = 1;  // 上涨支数
		required int32 fallCount = 2;  // 下跌支数
		required int32 equalCount = 3;  // 平盘支数
	}
	
	//期货类型额外数据
	message FutureSnapshotExData
	{
		required double lastSettlePrice = 1; //昨结
		required int32 position = 2; //持仓量
		required int32 positionChange = 3; //日增仓
		required string lastTradeTime = 4; //最后交易日，只有非主连期货合约才有该字段
		optional double lastTradeTimestamp = 5; //最后交易日时间戳，只有非主连期货合约才有该字段
		required bool isMainContract = 6; //是否主连合约
	}
	
	//基金类型额外数据
	message TrustSnapshotExData
	{
		required double dividendYield = 1; //股息率（该字段为百分比字段，默认不展示%，如20实际对应20%）
		required double aum = 2; //资产规模（单位：元）
		required int64 outstandingUnits = 3; //总发行量
		required double netAssetValue = 4; //单位净值
		required double premium = 5; //溢价（该字段为百分比字段，默认不展示%，如20实际对应20%）
		required int32 assetClass = 6; //Qot_Common.AssetClass，资产类别
	}

	//基本快照数据
	message SnapshotBasicData
	{
		required Qot_Common.Security security = 1; //股票
		required int32 type = 2; //Qot_Common.SecurityType,股票类型
		required bool isSuspend = 3; //是否停牌
		required string listTime = 4; //上市时间字符串
		required int32 lotSize = 5; //每手数量
		required double priceSpread = 6; //价差
		required string updateTime = 7; //更新时间字符串
		required double highPrice = 8; //最高价
		required double openPrice = 9; //开盘价
		required double lowPrice = 10; //最低价
		required double lastClosePrice = 11; //昨收价
		required double curPrice = 12; //最新价
		required int64 volume = 13; //成交量
		required double turnover = 14; //成交额
		required double turnoverRate = 15; //换手率（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		optional double listTimestamp = 16; //上市时间戳
		optional double updateTimestamp = 17; //更新时间戳
		optional double askPrice = 18;//卖价
		optional double bidPrice = 19;//买价
		optional int64 askVol = 20;//卖量
		optional int64 bidVol = 21;//买量
		optional bool enableMargin = 22; // 是否可融资，如果为true，后两个字段才有意义
		optional double mortgageRatio = 23; // 股票抵押率（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		optional double longMarginInitialRatio = 24; // 融资初始保证金率（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		optional bool enableShortSell = 25; // 是否可卖空，如果为true，后三个字段才有意义
		optional double shortSellRate = 26; // 卖空参考利率（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		optional int64 shortAvailableVolume = 27; // 剩余可卖空数量（股）
		optional double shortMarginInitialRatio = 28; // 卖空（融券）初始保证金率（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		optional double amplitude = 29; // 振幅（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		optional double avgPrice = 30; // 平均价
		optional double bidAskRatio = 31; // 委比（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		optional double volumeRatio = 32;  // 量比
		optional double highest52WeeksPrice = 33;  // 52周最高价
		optional double lowest52WeeksPrice = 34;  // 52周最低价
		optional double highestHistoryPrice = 35;  // 历史最高价
		optional double lowestHistoryPrice = 36;  // 历史最低价
		optional Qot_Common.PreAfterMarketData preMarket = 37; //Qot_Common::PreAfterMarketData 盘前数据
		optional Qot_Common.PreAfterMarketData afterMarket = 38; //Qot_Common::PreAfterMarketData 盘后数据
		optional int32 secStatus = 39; //Qot_Common::SecurityStatus 股票状态
	}

	message Snapshot
	{
		required SnapshotBasicData basic = 1; //快照基本数据
		optional EquitySnapshotExData equityExData = 2; //正股快照额外数据
		optional WarrantSnapshotExData warrantExData = 3; //窝轮快照额外数据
		optional OptionSnapshotExData optionExData = 4; //期权快照额外数据
		optional IndexSnapshotExData indexExData = 5; //指数快照额外数据
		optional PlateSnapshotExData plateExData = 6; //板块快照额外数据
		optional FutureSnapshotExData futureExData = 7; //期货类型额外数据
		optional TrustSnapshotExData trustExData = 8; //基金类型额外数据
	}

	message S2C
	{
		repeated Snapshot snapshotList = 1; //股票快照
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}


.. note::

	* 股票结构参考 `Security <base_define.html#security>`_
	* 接口限制请参见 `获取股票快照限制 <intro.html#id36>`_
	
-------------------------------------

`Qot_GetPlateSet.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetPlateSet.proto>`_ - 3204获取板块集合下的板块
-----------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetPlateSet;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required int32 market = 1; //Qot_Common.QotMarket,股票市场
		required int32 plateSetType = 2; //Qot_Common.PlateSetType,板块集合的类型
	}

	message S2C
	{
		repeated Qot_Common.PlateInfo plateInfoList = 1; //板块集合下的板块信息
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}
	
.. note::

	* 市场类型参考 `QotMarket <base_define.html#qotmarket>`_
	* 板块集合类型参考 `PlateSetType <base_define.html#platesettype>`_
	* 股票结构参考 `Security <base_define.html#security>`_
	* 板块信息结构参考  `PlateInfo <base_define.html#plateinfo>`_
	* 限频接口：30秒内最多10次	
	
-------------------------------------

`Qot_GetPlateSecurity.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetPlateSecurity.proto>`_ - 3205获取板块下的股票
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetPlateSecurity;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required Qot_Common.Security plate = 1; //板块
		optional int32 sortField = 2;//Qot_Common.SortField,根据哪个字段排序,不填默认Code排序
		optional bool ascend = 3;//升序true, 降序false, 不填默认升序

	}

	message S2C
	{
		repeated Qot_Common.SecurityStaticInfo staticInfoList = 1; //板块下的股票静态信息
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}

.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 股票静态信息结构参考 `SecurityStaticInfo <base_define.html#securitystaticbasic>`_
	* 限频接口：30秒内最多10次
	
-------------------------------------

`Qot_GetReference.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetReference.proto>`_ - 3206 获取正股相关股票
--------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetReference;

	import "Common.proto";
	import "Qot_Common.proto";

	enum ReferenceType
	{
		ReferenceType_Unknow = 0; 
		ReferenceType_Warrant = 1; //正股相关的窝轮
		ReferenceType_Future = 2; //期货主连的相关合约
	}

	message C2S
	{
		required Qot_Common.Security security = 1; //股票
		required int32 referenceType = 2; // ReferenceType, 相关类型
	}

	message S2C
	{
		repeated Qot_Common.SecurityStaticInfo staticInfoList = 2; //相关股票列表
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}
	
.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 股票静态信息结构参考 `SecurityStaticInfo <base_define.html#securitystaticbasic>`_

-------------------------------------

`Qot_GetOwnerPlate.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetOwnerPlate.proto>`_ - 3207获取股票所属板块
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetOwnerPlate;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		repeated Qot_Common.Security securityList = 1; //股票
	}

	message SecurityOwnerPlate
	{
		required Qot_Common.Security security = 1; //股票
		repeated Qot_Common.PlateInfo plateInfoList = 2; //所属板块
	}

	message S2C
	{
		repeated SecurityOwnerPlate ownerPlateList = 1; //所属板块信息
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}


.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 板块信息结构参考  `PlateInfo <base_define.html#plateinfo>`_
	* 限频接口：30秒内最多10次	
	* 最多可传入200只股票
	* 仅支持正股和指数

-------------------------------------

`Qot_GetHoldingChangeList.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetHoldingChangeList.proto>`_ - 3208获取持股变化列表
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetHoldingChangeList;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required Qot_Common.Security security = 1; //股票
		required int32 holderCategory = 2; //Qot_Common.HolderCategory 持有者类别
		//以下是发布时间筛选，不传返回所有数据，传了返回发布时间属于开始时间到结束时间段内的数据
		optional string beginTime = 3; //开始时间，严格按YYYY-MM-DD HH:MM:SS或YYYY-MM-DD HH:MM:SS.MS格式传
		optional string endTime = 4; //结束时间，严格按YYYY-MM-DD HH:MM:SS或YYYY-MM-DD HH:MM:SS.MS格式传
	}

	message S2C
	{
		required Qot_Common.Security security = 1; //股票
		repeated Qot_Common.ShareHoldingChange holdingChangeList = 2; //对应类别的持股变化列表（最多返回前100大股东的变化）
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		optional S2C s2c = 4;
	}

.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 持有者类别枚举参考  `HolderCategory <base_define.html#holdercategory>`_
	* 持股变化列表结构参考  `ShareHoldingChange <base_define.html#shareholdingchange>`_
	* 限频接口：30秒内最多10次	
	* 最多返回前100大股东的变化
	* 目前仅支持美股

-------------------------------------

`Qot_GetOptionChain.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetOptionChain.proto>`_ - 3209获取期权链
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

    syntax = "proto2";
	package Qot_GetOptionChain;
	option java_package = "com.futu.openapi.pb";

	import "Common.proto";
	import "Qot_Common.proto";

	enum OptionCondType
	{
		OptionCondType_Unknow = 0;
		OptionCondType_WithIn = 1; //价内
		OptionCondType_Outside = 2; //价外
	}

	//以下为数据字段筛选，可选字段，不填表示不过滤
	message DataFilter
	{
		optional double impliedVolatilityMin = 1; //隐含波动率过滤起点
		optional double impliedVolatilityMax = 2; //隐含波动率过滤终点

		optional double deltaMin = 3; //希腊值 Delta过滤起点
		optional double deltaMax = 4; //希腊值 Delta过滤终点

		optional double gammaMin = 5; //希腊值 Gamma过滤起点
		optional double gammaMax = 6; //希腊值 Gamma过滤终点

		optional double vegaMin = 7; //希腊值 Vega过滤起点
		optional double vegaMax = 8; //希腊值 Vega过滤终点

		optional double thetaMin = 9; //希腊值 Theta过滤起点
		optional double thetaMax = 10; //希腊值 Theta过滤终点

		optional double rhoMin = 11; //希腊值 Rho过滤起点
		optional double rhoMax = 12; //希腊值 Rho过滤终点

		optional double netOpenInterestMin = 13; //净未平仓合约数过滤起点
		optional double netOpenInterestMax = 14; //净未平仓合约数过滤终点

		optional double openInterestMin = 15; //未平仓合约数过滤起点
		optional double openInterestMax = 16; //未平仓合约数过滤终点

		optional double volMin = 17; //成交量过滤起点
		optional double volMax = 18; //成交量过滤终点
	}

	message C2S
	{
		required Qot_Common.Security owner = 1; //期权标的股,目前仅支持传入港美正股以及恒指国指
		optional int32 indexOptionType = 6; //Qot_Common.IndexOptionType, 指数期权的类型，仅用于恒指国指
		optional int32 type = 2; //Qot_Common.OptionType,期权类型,可选字段,不指定则表示都返回
		optional int32 condition = 3; //OptionCondType,价内价外,可选字段,不指定则表示都返回
		required string beginTime = 4; //期权到期日开始时间
		required string endTime = 5; //期权到期日结束时间,时间跨度最多一个月
		optional DataFilter dataFilter = 7; //数据字段筛选
	}

	message OptionItem
	{
		optional Qot_Common.SecurityStaticInfo call = 1; //看涨,不一定有该字段,由请求条件决定
		optional Qot_Common.SecurityStaticInfo put = 2; //看跌,不一定有该字段,由请求条件决定
	}

	message OptionChain
	{
		required string strikeTime = 1; //行权日
		repeated OptionItem option = 2; //期权信息
		optional double strikeTimestamp = 3; //行权日时间戳
	}

	message S2C
	{
		repeated OptionChain optionChain = 1; //期权链
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		optional S2C s2c = 4;
	}


.. note::
    
    * 股票结构参考 `Security <base_define.html#security>`_
    * 期权类型参考 `OptionType <base_define.html#optiontype>`_
    * 股票静态信息结构参考 `SecurityStaticInfo <base_define.html#securitystaticbasic>`_
    * 限频接口：30秒内最多10次
    * 目前仅支持美股

------------------------------------------

`Qot_GetWarrant.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetWarrant.proto>`_ - 3210获取窝轮
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetWarrant;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required int32 begin = 1; //数据起始点
		required int32 num =  2; //请求数据个数，最大200
		required int32 sortField = 3;//Qot_Common.SortField,根据哪个字段排序
		required bool ascend = 4;//升序true, 降序false
		
		//以下为筛选条件，可选字段，不填表示不过滤
		optional Qot_Common.Security owner = 5;	//所属正股
		repeated int32 typeList = 6; //Qot_Common.WarrantType,窝轮类型过滤列表
		repeated int32 issuerList = 7; //Qot_Common.Issuer,发行人过滤列表
		optional string maturityTimeMin = 8; //到期日,到期日范围的开始时间戳
		optional string maturityTimeMax = 9; //到期日范围的结束时间戳
		optional int32 ipoPeriod = 10; //Qot_Common.IpoPeriod,上市日
		optional int32 priceType = 11; //Qot_Common.PriceType, 价内/价外（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		optional int32 status = 12; //Qot_Common.WarrantStatus, 窝轮状态
		optional double curPriceMin = 13; //最新价的过滤下限（闭区间），不传代表下限为-∞
		optional double curPriceMax = 14; //最新价的过滤上限（闭区间），不传代表上限为+∞ 	
		optional double strikePriceMin = 15; //行使价的过滤下限（闭区间），不传代表下限为-∞
		optional double strikePriceMax = 16; //行使价的过滤上限（闭区间），不传代表上限为+∞ 
		optional double streetMin = 17; //街货占比的过滤下限（闭区间），该字段为百分比字段，默认不展示%，如20实际对应20%。不传代表下限为-∞
		optional double streetMax = 18; //街货占比的过滤上限（闭区间），该字段为百分比字段，默认不展示%，如20实际对应20%。不传代表上限为+∞
		optional double conversionMin = 19; //换股比率的过滤下限（闭区间），不传代表下限为-∞
		optional double conversionMax = 20; //换股比率的过滤上限（闭区间），不传代表上限为+∞
		optional uint64 volMin = 21; //成交量的过滤下限（闭区间），不传代表下限为-∞
		optional uint64 volMax = 22; //成交量的过滤上限（闭区间），不传代表上限为+∞
		optional double premiumMin = 23; //溢价的过滤下限（闭区间），该字段为百分比字段，默认不展示%，如20实际对应20%。不传代表下限为-∞
		optional double premiumMax = 24; //溢价的过滤上限（闭区间），该字段为百分比字段，默认不展示%，如20实际对应20%。不传代表上限为+∞
		optional double leverageRatioMin = 25; //杠杆比率的过滤下限（闭区间），不传代表下限为-∞
		optional double leverageRatioMax = 26; //杠杆比率的过滤上限（闭区间），不传代表上限为+∞
		optional double deltaMin = 27;//对冲值的过滤下限（闭区间）, 仅认购认沽支持该字段过滤，不传代表下限为-∞
		optional double deltaMax = 28;//对冲值的过滤上限（闭区间）, 仅认购认沽支持该字段过滤，不传代表上限为+∞
		optional double impliedMin = 29; //引伸波幅的过滤下限（闭区间）, 仅认购认沽支持该字段过滤，不传代表下限为-∞
		optional double impliedMax = 30; //引伸波幅的过滤上限（闭区间）, 仅认购认沽支持该字段过滤，不传代表上限为+∞	
		optional double recoveryPriceMin = 31; //收回价的过滤下限（闭区间）, 仅牛熊证支持该字段过滤，不传代表下限为-∞
		optional double recoveryPriceMax = 32; //收回价的过滤上限（闭区间）, 仅牛熊证支持该字段过滤，不传代表上限为+∞
		optional double priceRecoveryRatioMin = 33;//正股距收回价, 的过滤下限（闭区间）, 仅牛熊证支持该字段过滤。该字段为百分比字段，默认不展示%，如20实际对应20%。不传代表下限为-∞
		optional double priceRecoveryRatioMax = 34;//正股距收回价, 的过滤上限（闭区间）, 仅牛熊证支持该字段过滤。该字段为百分比字段，默认不展示%，如20实际对应20%。不传代表上限为+∞	
	}

	message WarrantData
	{
		//静态数据项
		required Qot_Common.Security stock = 1; //股票
		required Qot_Common.Security owner = 2; //所属正股
		required int32 type = 3; //Qot_Common.WarrantType,窝轮类型
		required int32 issuer = 4; //Qot_Common.Issuer,发行人
		required string maturityTime = 5; //到期日
		optional double maturityTimestamp = 6; //到期日时间戳
		required string listTime = 7; //上市时间
		optional double listTimestamp = 8; //上市时间戳
		required string lastTradeTime = 9; //最后交易日
		optional double lastTradeTimestamp = 10; //最后交易日时间戳
		optional double recoveryPrice = 11; //收回价,仅牛熊证支持该字段
		required double conversionRatio = 12; //换股比率
		required int32 lotSize = 13; //每手数量
		required double strikePrice = 14; //行使价	
		required double lastClosePrice = 15; //昨收价		
		required string name = 16; //名称	
		
		//动态数据项
		required double curPrice = 17; //当前价
		required double priceChangeVal = 18; //涨跌额
		required double changeRate = 19; //涨跌幅（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）	
		required int32 status = 20; //Qot_Common.WarrantStatus, 窝轮状态	
		required double bidPrice = 21; //买入价	
		required double askPrice = 22; //卖出价
		required int64 bidVol = 23; //买量
		required int64 askVol = 24; //卖量
		required int64 volume = 25; //成交量
		required double turnover = 26; //成交额	
		required double score = 27; //综合评分
		required double premium = 28; //溢价（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		required double breakEvenPoint = 29; //打和点	
		required double leverage = 30; //杠杆比率（倍）
		required double ipop = 31; //价内/价外（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）			
		optional double priceRecoveryRatio = 32; //正股距收回价，仅牛熊证支持该字段（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		required double conversionPrice = 33; //换股价
		required double streetRate = 34; //街货占比（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）	
		required int64 streetVol = 35; //街货量
		required double amplitude = 36; //振幅（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		required int64 issueSize = 37; //发行量	        
		required double highPrice = 39; //最高价
		required double lowPrice = 40; //最低价	
		optional double impliedVolatility = 41; //引伸波幅,仅认购认沽支持该字段
		optional double delta = 42; //对冲值,仅认购认沽支持该字段
		required double effectiveLeverage = 43; //有效杠杆		
		optional double upperStrikePrice = 44; //上限价，仅界内证支持该字段
		optional double lowerStrikePrice = 45; //下限价，仅界内证支持该字段
		optional int32 inLinePriceStatus = 46; //Qot_Common.PriceType, 界内界外，仅界内证支持该字段
	}

	message S2C
	{
		required bool lastPage = 1; //是否最后一页了,false:非最后一页,还有窝轮记录未返回; true:已是最后一页
		required int32 allCount = 2; //该条件请求所有数据的个数
		repeated WarrantData warrantDataList = 3; //窝轮数据
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		optional S2C s2c = 4;
	}

.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 排序类型参考 `SortField <base_define.html#sortfield>`_
	* 窝轮类型过滤列表参考 `WarrantType <base_define.html#warranttype>`_
	* 发行人过滤列表参考 `Issuer <base_define.html#issuer>`_
	* 上市日类型参考 `IpoPeriod <base_define.html#ipoperiod>`_
	* 价内价外类型参考 `PriceType <base_define.html#pricetype>`_
	* 窝轮状态类型参考 `WarrantStatus <base_define.html#warrantstatus>`_
	* 接口限制请参见 `获取窝轮限制 <intro.html#id43>`_
	* 目前仅支持港股
	* 使用类似最新价的排序字段获取数据的时候，多页获取的间隙，数据的排序有可能是变化的。

`Qot_RequestHistoryKLQuota.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_RequestHistoryKLQuota.proto>`_ - 3104拉取历史K线已经用掉的额度
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_RequestHistoryKLQuota;

	import "Common.proto";
	import "Qot_Common.proto";

	message DetailItem
	{
		required Qot_Common.Security security = 1;  //拉取的股票
		required string requestTime = 2;            //拉取的时间字符串
		optional int64 requestTimeStamp = 3;        //拉取的时间戳
	}

	message C2S
	{
		optional bool bGetDetail = 2;  //是否返回详细拉取过的历史纪录
	}

	message S2C
	{
		required int32 usedQuota = 1;	      //已使用过的额度，即当前周期内已经下载过多少只股票。
	    required int32 remainQuota = 2;       //剩余额度
	    repeated DetailItem detailList = 3;	  //每只拉取过的股票的下载时间
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}

-------------------------------------

`Qot_GetCapitalFlow.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetCapitalFlow.proto>`_ - 3211获取资金流向
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetCapitalFlow;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required Qot_Common.Security security = 1; //股票
	}

	message CapitalFlowItem
	{	
		required double inFlow = 1; //净流入的资金额度
		optional string time = 2; //开始时间字符串,以分钟为单位
		optional double timestamp = 3; //开始时间戳
	}

	message S2C
	{
		repeated CapitalFlowItem flowItemList = 1; //资金流向
		optional string lastValidTime = 2; //数据最后有效时间字符串
		optional double lastValidTimestamp = 3; //数据最后有效时间戳
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		optional S2C s2c = 4;
	}

.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 限频接口：30秒内最多30次	

-------------------------------------

`Qot_GetCapitalDistribution.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetCapitalDistribution.proto>`_ - 3212获取资金分布
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetCapitalDistribution;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required Qot_Common.Security security = 1; //股票
	}

	message S2C
	{
		required double capitalInBig = 1; //流入资金额度，大单
		required double capitalInMid = 2; //流入资金额度，中单
		required double capitalInSmall = 3; //流入资金额度，小单
		required double capitalOutBig = 4; //流出资金额度，大单
		required double capitalOutMid = 5; //流出资金额度，中单
		required double capitalOutSmall = 6; //流出资金额度，小单
		optional string updateTime = 7; //更新时间字符串
		optional double updateTimestamp = 8; //更新时间戳
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		optional S2C s2c = 4;
	}

.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 限频接口：30秒内最多30次	

-------------------------------------

`Qot_GetUserSecurity.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetUserSecurity.proto>`_ - 3213获取自选股分组下的股票
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetUserSecurity;

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required string groupName = 1; //分组名,有同名的返回最先创建的
	}

	message S2C
	{
		repeated Qot_Common.SecurityStaticInfo staticInfoList = 1; //自选股分组下的股票列表
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}

.. note::
	
	* 股票静态信息结构参考 `SecurityStaticInfo <base_define.html#securitystaticbasic>`_
	* 限频接口：30秒内最多10次	
	* 不支持持仓分组
	
-------------------------------------

`Qot_ModifyUserSecurity.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_ModifyUserSecurity.proto>`_ - 3214修改自选股分组下的股票
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_ModifyUserSecurity;

	import "Common.proto";
	import "Qot_Common.proto";

	enum ModifyUserSecurityOp
	{
		ModifyUserSecurityOp_Unknown = 0;
		ModifyUserSecurityOp_Add = 1; //新增
		ModifyUserSecurityOp_Del = 2; //删除自选
		ModifyUserSecurityOp_MoveOut = 3; //移出分组
	}

	message C2S
	{
		required string groupName = 1; //分组名,有同名的返回最先创建的
		required int32 op = 2; //ModifyUserSecurityOp,操作类型
		repeated Qot_Common.Security securityList = 3; //新增、删除或移出该分组下的股票
	}

	message S2C
	{
		
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}


.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 限频接口：30秒内最多10次	
	* 仅支持自定义分组
	
-------------------------------------

`Qot_StockFilter.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_StockFilter.proto>`_ - 3215获取条件选股
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_StockFilter;
	option java_package = "com.futu.openapi.pb";

	import "Common.proto";
	import "Qot_Common.proto";

	// 使用到以下 6 个结构体（BaseFilter，AccumulateFilter，FinancialFilter，BaseData， AccumulateData，FinancialData）的用户请注意，由于属性字段名“field”与 C # 的 protobuf 保留函数名产生冲突，Futu API 将从 3.18 版本开始将这一字段统一更名为“fieldName”，请注意修改使用到对应接口的字段名。

	// 简单属性
	enum StockField  
	{
		StockField_Unknown = 0; // 未知
		StockField_StockCode = 1; // 股票代码，不能填区间上下限值。
		StockField_StockName = 2; // 股票名称，不能填区间上下限值。
		StockField_CurPrice = 3; // 最新价 例如填写[10,20]值区间
		StockField_CurPriceToHighest52WeeksRatio = 4; // (现价 - 52周最高)/52周最高，对应PC端离52周高点百分比 例如填写[-30,-10]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		StockField_CurPriceToLowest52WeeksRatio = 5; // (现价 - 52周最低)/52周最低，对应PC端离52周低点百分比 例如填写[20,40]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%）
		StockField_HighPriceToHighest52WeeksRatio = 6; // (今日最高 - 52周最高)/52周最高 例如填写[-3,-1]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%）
		StockField_LowPriceToLowest52WeeksRatio = 7; // (今日最低 - 52周最低)/52周最低 例如填写[10,70]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%）
		StockField_VolumeRatio = 8; // 量比 例如填写[0.5,30]值区间
		StockField_BidAskRatio = 9; // 委比 例如填写[-20,80.5]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%）
		StockField_LotPrice = 10; // 每手价格 例如填写[40,100]值区间
		StockField_MarketVal = 11; // 市值 例如填写[50000000,3000000000]值区间
		StockField_PeAnnual = 12; // 市盈率(静态) 例如填写[-8,65.3]值区间
		StockField_PeTTM = 13; // 市盈率TTM 例如填写[-10,20.5]值区间
		StockField_PbRate = 14; // 市净率 例如填写[0.5,20]值区间
		StockField_ChangeRate5min = 15; // 五分钟价格涨跌幅 例如填写[-5,6.3]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%）
		StockField_ChangeRateBeginYear = 16; // 年初至今价格涨跌幅 例如填写[-50.1,400.7]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%）
		
		// 基础量价属性
		StockField_PSTTM = 17; // 市销率(TTM) 例如填写 [100, 500] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%） 
		StockField_PCFTTM = 18; // 市现率(TTM) 例如填写 [100, 1000] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
		StockField_TotalShare = 19; // 总股数 例如填写 [1000000000,1000000000] 值区间 (单位：股)
		StockField_FloatShare = 20; // 流通股数 例如填写 [1000000000,1000000000] 值区间 (单位：股)
		StockField_FloatMarketVal = 21; // 流通市值 例如填写 [1000000000,1000000000] 值区间 (单位：元)
	};

	// 累积属性
	enum AccumulateField
	{
		AccumulateField_Unknown = 0; // 未知
		AccumulateField_ChangeRate = 1; // 涨跌幅 例如填写[-10.2,20.4]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%）
		AccumulateField_Amplitude = 2; // 振幅 例如填写[0.5,20.6]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%）
		AccumulateField_Volume = 3; // 日均成交量 例如填写[2000,70000]值区间
		AccumulateField_Turnover = 4; // 日均成交额 例如填写[1400,890000]值区间
		AccumulateField_TurnoverRate = 5; // 换手率 例如填写[2,30]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%）
	}

	// 财务属性
	enum FinancialField
	{
		// 基础财务属性
		FinancialField_Unknown = 0; // 未知
		FinancialField_NetProfit = 1; // 净利润 例如填写[100000000,2500000000]值区间
		FinancialField_NetProfitGrowth = 2; // 净利润增长率 例如填写[-10,300]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%）
		FinancialField_SumOfBusiness = 3; // 营业收入 例如填写[100000000,6400000000]值区间
		FinancialField_SumOfBusinessGrowth = 4; // 营收同比增长率 例如填写[-5,200]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%）
		FinancialField_NetProfitRate = 5; // 净利率 例如填写[10,113]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%）
		FinancialField_GrossProfitRate = 6; // 毛利率 例如填写[4,65]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%）
		FinancialField_DebtAssetRate = 7; // 资产负债率 例如填写[5,470]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%）
		FinancialField_ReturnOnEquityRate = 8; // 净资产收益率 例如填写[20,230]值区间（该字段为百分比字段，默认不展示%，如20实际对应20%）
		
		// 盈利能力属性
		FinancialField_ROIC = 9; // 投入资本回报率 例如填写 [1.0,10.0] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_ROATTM = 10; // 资产回报率(TTM) 例如填写 [1.0,10.0] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%。仅适用于年报。）
		FinancialField_EBITTTM = 11; // 息税前利润(TTM) 例如填写 [1000000000,1000000000] 值区间（单位：元。仅适用于年报。）
		FinancialField_EBITDA = 12; // 税息折旧及摊销前利润 例如填写 [1000000000,1000000000] 值区间（单位：元）
		FinancialField_OperatingMarginTTM = 13; // 营业利润率(TTM) 例如填写 [1.0,10.0] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%。仅适用于年报。）
		FinancialField_EBITMargin = 14; // EBIT利润率 例如填写 [1.0,10.0] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_EBITDAMargin  = 15; // EBITDA利润率 例如填写 [1.0,10.0] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_FinancialCostRate = 16; // 财务成本率 例如填写 [1.0,10.0] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_OperatingProfitTTM  = 17; // 营业利润(TTM) 例如填写 [1000000000,1000000000] 值区间 （单位：元。仅适用于年报。）
		FinancialField_ShareholderNetProfitTTM = 18; // 归属于母公司的净利润 例如填写 [1000000000,1000000000] 值区间 （单位：元。仅适用于年报。）
		FinancialField_NetProfitCashCoverTTM = 19; // 盈利中的现金收入比例 例如填写 [1.0,60.0] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%。仅适用于年报。）
		
		// 偿债能力属性
		FinancialField_CurrentRatio = 20; // 流动比率 例如填写 [100,250] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_QuickRatio = 21; // 速动比率 例如填写 [100,250] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）	
		
		// 清债能力属性
		FinancialField_CurrentAssetRatio = 22; // 流动资产率 例如填写 [10,100] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_CurrentDebtRatio = 23; // 流动负债率 例如填写 [10,100] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_EquityMultiplier = 24; // 权益乘数 例如填写 [100,180] 值区间
		FinancialField_PropertyRatio = 25; // 产权比率 例如填写 [50,100] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%） 
		FinancialField_CashAndCashEquivalents = 26; // 现金和现金等价 例如填写 [1000000000,1000000000] 值区间（单位：元）	
		
		// 运营能力属性
		FinancialField_TotalAssetTurnover = 27; // 总资产周转率 例如填写 [50,100] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_FixedAssetTurnover = 28; // 固定资产周转率 例如填写 [50,100] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_InventoryTurnover = 29; // 存货周转率 例如填写 [50,100] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_OperatingCashFlowTTM = 30; // 经营活动现金流(TTM) 例如填写 [1000000000,1000000000] 值区间（单位：元。仅适用于年报。）
		FinancialField_AccountsReceivable = 31; // 应收账款净额 例如填写 [1000000000,1000000000] 值区间 例如填写 [1000000000,1000000000] 值区间 （单位：元）	
		
		// 成长能力属性
		FinancialField_EBITGrowthRate = 32 ; // EBIT同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_OperatingProfitGrowthRate = 33; // 营业利润同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_TotalAssetsGrowthRate = 34; // 总资产同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_ProfitToShareholdersGrowthRate = 35; // 归母净利润同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_ProfitBeforeTaxGrowthRate = 36; // 总利润同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_EPSGrowthRate = 37; // EPS同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_ROEGrowthRate = 38; // ROE同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_ROICGrowthRate = 39; // ROIC同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_NOCFGrowthRate = 40; // 经营现金流同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_NOCFPerShareGrowthRate = 41; // 每股经营现金流同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
		
		// 现金流属性
		FinancialField_OperatingRevenueCashCover = 42; // 经营现金收入比 例如填写 [10,100] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
		FinancialField_OperatingProfitToTotalProfit = 43; // 营业利润占比 例如填写 [10,100] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%） 	
		
		// 市场表现属性
		FinancialField_BasicEPS = 44; // 基本每股收益 例如填写 [0.1,10] 值区间 (单位：元)
		FinancialField_DilutedEPS = 45; // 稀释每股收益 例如填写 [0.1,10] 值区间 (单位：元)
		FinancialField_NOCFPerShare = 46; // 每股经营现金净流量 例如填写 [0.1,10] 值区间 (单位：元)
	}
	
	// 财报时间周期
	enum FinancialQuarter
	{
		FinancialQuarter_Unknown = 0; // 未知
		FinancialQuarter_Annual = 1; // 年报
		FinancialQuarter_FirstQuarter = 2; // 一季报
		FinancialQuarter_Interim = 3; // 中报
		FinancialQuarter_ThirdQuarter = 4; // 三季报
		FinancialQuarter_MostRecentQuarter = 5; // 最近季报
	}

	// 排序方向
	enum SortDir
	{
		SortDir_No = 0; // 不排序
		SortDir_Ascend = 1; // 升序
		SortDir_Descend = 2; // 降序
	};

	// 简单属性筛选
	message BaseFilter 
	{ 
		required int32 fieldName = 1; // StockField 简单属性
		optional double filterMin = 2; // 区间下限（闭区间），不传代表下限为-∞
		optional double filterMax = 3; // 区间上限（闭区间），不传代表上限为+∞
		optional bool isNoFilter = 4; // 该字段是否不需要筛选，True代表不筛选，False代表筛选。不传默认为不筛选
		optional int32 sortDir = 5; // SortDir 排序方向，默认不排序。
	};

	// 累积属性筛选
	message AccumulateFilter
	{
		required int32 fieldName = 1; // AccumulateField 累积属性
		optional double filterMin = 2; // 区间下限（闭区间），不传代表下限为-∞
		optional double filterMax = 3; // 区间上限（闭区间），不传代表上限为+∞
		optional bool isNoFilter = 4; // 该字段是否不需要筛选，True代表不筛选，False代表筛选。不传默认为不筛选
		optional int32 sortDir = 5; // SortDir 排序方向，默认不排序。
		required int32 days = 6; // 近几日，累积时间
	}

	// 财务属性筛选
	message FinancialFilter
	{
		required int32 fieldName = 1; // FinancialField 财务属性
		optional double filterMin = 2; // 区间下限（闭区间），不传代表下限为-∞
		optional double filterMax = 3; // 区间上限（闭区间），不传代表上限为+∞
		optional bool isNoFilter = 4; // 该字段是否不需要筛选，True代表不筛选，False代表筛选。不传默认为不筛选
		optional int32 sortDir = 5; // SortDir 排序方向，默认不排序。
		required int32 quarter = 6; // FinancialQuarter 财报累积时间
	}

	// 简单属性数据
	message BaseData 
	{ 
		required int32 fieldName = 1; // StockField 简单属性
		required double value = 2;
	};

	// 累积属性数据
	message AccumulateData
	{
		required int32 fieldName = 1; // AccumulateField 累积属性
		required double value = 2;
		required int32 days = 3; // 近几日，累积时间
	}

	// 财务属性数据
	message FinancialData
	{
		required int32 fieldName = 1; // FinancialField 财务属性
		required double value = 2;
		required int32 quarter = 3; // FinancialQuarter 财报累积时间
	}

	// 返回的股票数据
	message StockData
	{
		required Qot_Common.Security security = 1; // 股票
		required string name = 2; // 股票名称
		repeated BaseData baseDataList = 3; // 筛选后的简单属性数据
		repeated AccumulateData accumulateDataList = 4; // 筛选后的累积属性数据
		repeated FinancialData financialDataList = 5; // 筛选后的财务属性数据
	};

	message C2S
	{
		required int32 begin = 1; // 数据起始点
		required int32 num =  2;  // 请求数据个数，最大200
		required int32 market= 3; // Qot_Common::QotMarket股票市场，支持沪股和深股，且沪股和深股不做区分都代表A股市场。
		// 以下为筛选条件，可选字段，不填表示不过滤
		optional Qot_Common.Security plate = 4; // 板块
		repeated BaseFilter baseFilterList = 5; // 简单属性过滤器
		repeated AccumulateFilter accumulateFilterList = 6; // 累积属性过滤器
		repeated FinancialFilter financialFilterList = 7; // 财务属性过滤器
	}

	message S2C
	{
		required bool lastPage = 1; // 是否最后一页了,false:非最后一页,还有窝轮记录未返回; true:已是最后一页
		required int32 allCount = 2; // 该条件请求所有数据的个数
		repeated StockData dataList = 3; // 返回的股票数据列表
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; // RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		optional S2C s2c = 4;
	}

.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 市场类型参考 `QotMarket <base_define.html#qotmarket>`_
	* 简单属性筛选条件参考 `StockField <base_define.html#stockfield>`_
	* 累积属性筛选条件参考 `AccumulateField <base_define.html#accumulatefield>`_
	* 财务属性筛选条件参考 `FinancialField <base_define.html#financialfield>`_
	* 财报时间周期参考 `FinancialQuarter <base_define.html#financialquarter>`_
	* 排序方向参考 `SortDir <base_define.html#sortdir>`_
	* 限频接口：30秒内最多10次
	* 使用类似最新价的排序字段获取数据的时候，多页获取的间隙，数据的排序有可能是变化的。
		
----------------------------------------------------------------------------------------

`Qot_GetCodeChange.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetCodeChange.proto>`_ - 3216获取股票代码变更信息
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetCodeChange;
	option java_package = "com.futu.openapi.pb";

	import "Common.proto";
	import "Qot_Common.proto";

	enum CodeChangeType
	{
		CodeChangeType_Unkown = 0; //未知
		CodeChangeType_GemToMain = 1; //创业板转主板
		CodeChangeType_Unpaid = 2; //买卖未缴款供股权
		CodeChangeType_ChangeLot = 3; //更改买卖单位
		CodeChangeType_Split = 4; //拆股
		CodeChangeType_Joint = 5; //合股
		CodeChangeType_JointSplit = 6; //股份先并后拆
		CodeChangeType_SplitJoint = 7; //股份先拆后并
		CodeChangeType_Other = 8; //其他
	}

	message CodeChangeInfo
	{
		required int32 type = 1; //CodeChangeType,代码变化或者新增临时代码的事件类型
		required Qot_Common.Security security = 2; //主代码，在创业板转主板中表示主板
		required Qot_Common.Security relatedSecurity = 3; //关联代码，在创业板转主板中表示创业板，在剩余事件中表示临时代码
		optional string publicTime = 4; //公布时间
		optional double publicTimestamp = 5; //公布时间戳
		optional string effectiveTime = 6; //生效时间
		optional double effectiveTimestamp = 7; //生效时间戳
		optional string endTime = 8; //结束时间，在创业板转主板事件不存在该字段，在剩余事件表示临时代码交易结束时间
		optional double endTimestamp = 9; //结束时间戳，在创业板转主板事件不存在该字段，在剩余事件表示临时代码交易结束时间
	}

	enum TimeFilterType
	{
		TimeFilterType_Unknow = 0;
		TimeFilterType_Public = 1; //根据公布时间过滤
		TimeFilterType_Effective = 2; //根据生效时间过滤
		TimeFilterType_End = 3; //根据结束时间过滤
	}

	message TimeFilter
	{
		required int32 type = 1; //TimeFilterType, 过滤类型
		optional string beginTime = 2; //开始时间点
		optional string endTime = 3; //结束时间点
	}

	message C2S
	{
		optional int32 placeHolder = 1; //占位
		repeated Qot_Common.Security securityList = 2; //根据股票筛选
		repeated TimeFilter timeFilterList = 3; //根据时间筛选
		repeated int32 typeList = 4; //CodeChangeType，根据类型筛选
	}

	message S2C
	{
		repeated CodeChangeInfo codeChangeList = 1; //股票代码更换信息，目前仅有港股数据
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}



.. note::
  
  * 股票结构参考 `Security <base_define.html#security>`_

----------------------------------------------------------------------------------------

`Qot_GetIpoList.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetIpoList.proto>`_ - 3217获取IPO信息
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetIpoList;
	option java_package = "com.futu.openapi.pb";
	
	import "Common.proto";
	import "Qot_Common.proto";
	
	// Ipo基本数据
	message BasicIpoData
	{
		required Qot_Common.Security security = 1; // Qot_Common::QotMarket 股票市场，支持港股、美股和 A股。其中，A 股整体返回，不区分沪股和深股。
		required string name = 2; // 股票名称
		optional string listTime = 3; // 上市日期字符串
		optional double listTimestamp = 4; // 上市日期时间戳
	};
	
	// A股Ipo列表额外数据
	message CNIpoExData 
	{
		required string applyCode = 1; // 申购代码
		required int64 issueSize = 2; // 发行总数
		required int64 onlineIssueSize = 3; // 网上发行量
		required int64 applyUpperLimit = 4; // 申购上限
		required int64 applyLimitMarketValue = 5; // 顶格申购需配市值
		required bool isEstimateIpoPrice = 6; // 是否预估发行价
		required double ipoPrice = 7; // 发行价 预估值会因为募集资金、发行数量、发行费用等数据变动而变动，仅供参考。实际数据公布后会第一时间更新。
		required double industryPeRate = 8; // 行业市盈率
		required bool isEstimateWinningRatio = 9; // 是否预估中签率
		required double winningRatio = 10; // 中签率 该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%。预估值会因为募集资金、发行数量、发行费用等数据变动而变动，仅供参考。实际数据公布后会第一时间更新。
		required double issuePeRate = 11; // 发行市盈率
		optional string applyTime = 12; // 申购日期字符串
		optional double applyTimestamp = 13; // 申购日期时间戳
		optional string winningTime = 14; // 公布中签日期字符串
		optional double winningTimestamp = 15; // 公布中签日期时间戳
		required bool isHasWon = 16; // 是否已经公布中签号
		repeated WinningNumData winningNumData = 17; // Qot_GetIpoList::WinningNumData 中签号数据，对应PC中"公布中签日期的已公布"
	};
	
	// 中签号数据
	message WinningNumData
	{
		required string winningName = 1; // 分组名
	    required string winningInfo = 2; // 中签号信息
	}
	
	// 港股Ipo列表额外数据
	message HKIpoExData
	{
		required double ipoPriceMin = 1; // 最低发售价
		required double ipoPriceMax = 2; // 最高发售价
		required double listPrice = 3; // 上市价
		required int32 lotSize = 4; // 每手股数
		required double entrancePrice = 5; // 入场费
		required bool isSubscribeStatus = 6; // 是否为认购状态，True-认购中，False-待上市
		optional string applyEndTime = 7; // 截止认购日期字符串
		optional double applyEndTimestamp = 8; // 截止认购日期时间戳 因需处理认购手续，富途认购截止时间会早于交易所公布的日期。
	};
	
	// 美股Ipo列表额外数据
	message USIpoExData  
	{
		required double ipoPriceMin = 1; // 最低发行价
		required double ipoPriceMax = 2; // 最高发行价
		required int64 issueSize = 3; // 发行量
	};
	
	// 新股Ipo数据
	message IpoData
	{	
		required BasicIpoData basic = 1; // IPO基本数据	
		optional CNIpoExData cnExData = 2; // A股IPO额外数据
		optional HKIpoExData hkExData = 3; // 港股IPO额外数据
		optional USIpoExData usExData = 4; // 美股IPO额外数据
	};
	
	message C2S
	{
		required int32 market = 1; // Qot_Common::QotMarket股票市场，支持沪股和深股，且沪股和深股不做区分都代表A股市场。
	}
	
	message S2C
	{
		repeated IpoData ipoList = 1; // 新股IPO数据
	}
	
	message Request
	{
		required C2S c2s = 1;
	}
	
	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}


.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 限频接口：30秒内最多10次	
	
------------------------------------------------------------------------	

`Qot_GetFutureInfo.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetFutureInfo.proto>`_ - 3218获取期货合约资料
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetFutureInfo;
	option java_package = "com.futu.openapi.pb";
	
	import "Common.proto";
	import "Qot_Common.proto";

	//交易时间
	message TradeTime
	{
		optional double begin = 1; // 开始时间,以分钟为单位
		optional double end = 2; // 结束时间,以分钟为单位
	}	
    
	//期货合约资料的列表
	message FutureInfo
	{
		required string name = 1; // 合约名称
		required Qot_Common.Security security = 2; // 合约代码
		required string lastTradeTime = 3; //最后交易日，只有非主连期货合约才有该字段
		optional double lastTradeTimestamp = 4; //最后交易日时间戳，只有非主连期货合约才有该字段
		optional Qot_Common.Security owner = 5; //标的股 股票期货和股指期货才有该字段
		required string ownerOther = 6; //标的 
		required string exchange = 7; //交易所
		required string contractType = 8; //合约类型
		required double contractSize = 9; //合约规模
		required string contractSizeUnit = 10; //合约规模的单位
		required string quoteCurrency = 11; //报价货币
		required double minVar = 12; //最小变动单位
		required string minVarUnit = 13; //最小变动单位的单位
		optional string quoteUnit = 14; //报价单位
		repeated TradeTime tradeTime = 15; //交易时间
		required string timeZone = 16; //所在时区
		required string exchangeFormatUrl = 17; //交易所规格
	}

	message C2S
	{
		repeated Qot_Common.Security securityList = 1; //股票列表
	}

	message S2C
	{
		repeated FutureInfo futureInfoList = 1; //期货合约资料的列表
	}
	
	message Request
	{
		required C2S c2s = 1;
	}
	
	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}


.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 限频接口：30秒内最多10次	
	
------------------------------------------------------------------------

`Qot_RequestTradeDate.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_RequestTradeDate.proto>`_ - 3219在线请求交易日
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_RequestTradeDate;
	option java_package = "com.futu.openapi.pb";

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required int32 market = 1; //Qot_Common.TradeDateMarket,要查询的市场
		required string beginTime = 2; //开始时间字符串
		required string endTime = 3; //结束时间字符串
	}

	message TradeDate
	{
		required string time = 1; //时间字符串
		optional double timestamp = 2; //时间戳
		optional int32 tradeDateType = 3; //Qot_Common.TradeDateType,交易时间类型
	}

	message S2C
	{
		repeated TradeDate tradeDateList = 1; //交易日,注意该交易日是通过自然日去除周末以及节假日得到，不包括临时休市数据。
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
}



.. note::
	
	* 交易日市场类型参考 `TradeDateMarket <base_define.html#tradedatemarket>`_
	* 限频接口：30秒内最多30次	
	
------------------------------------------------------------------------

`Qot_SetPriceReminder.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_SetPriceReminder.proto>`_ - 3220设置到价提醒
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_SetPriceReminder;
	option java_package = "com.futu.openapi.pb";

	import "Common.proto";
	import "Qot_Common.proto";

	enum SetPriceReminderOp
	{
		SetPriceReminderOp_Unknown = 0;
		SetPriceReminderOp_Add = 1; //新增
		SetPriceReminderOp_Del = 2; //删除
		SetPriceReminderOp_Enable = 3; //启用
		SetPriceReminderOp_Disable = 4; //禁用
		SetPriceReminderOp_Modify = 5; //修改
		SetPriceReminderOp_DelAll = 6; //删除全部（删除指定股票下的所有到价提醒）
	}

	message C2S
	{
		required Qot_Common.Security security = 1; // 股票
		required int32 op = 2; // SetPriceReminderOp, 操作类型
		optional int64 key = 3; // 到价提醒的标识，GetPriceReminder协议可获得，用于指定要操作的到价提醒项，对于新增的情况不需要填
		optional int32 type = 4; // Qot_Common::PriceReminderType，提醒类型，删除、启用、禁用的情况下会忽略该字段
		optional int32 freq = 7; // Qot_Common::PriceReminderFreq，提醒频率类型，删除、启用、禁用的情况下会忽略该字段
		optional double value = 5; // 提醒值，删除、启用、禁用的情况下会忽略该字段
		optional string note = 6; // 用户设置到价提醒时的标注，最多10个字符，删除、启用、禁用的情况下会忽略该字段
	}
	//注意：
	//1. API 中成交量设置统一以股为单位。但是牛牛客户端中，A 股是以为手为单位展示
	//2. 到价提醒类型，存在最小精度，如下：
	// TURNOVER_UP：成交额最小精度为 10 元（人民币元，港元，美元）。传入的数值会自动向下取整到最小精度的整数倍。如果设置【00700成交额102元提醒】，设置后会得到【00700成交额100元提醒】；如果设置【00700 成交额 8 元提醒】，设置后会得到【00700 成交额 0 元提醒】
	// VOLUME_UP：A 股成交量最小精度为 1000 股，其他市场股票成交量最小精度为 10 股，期权成交量最小精度为0.001张。传入的数值会自动向下取整到最小精度的整数倍。
	// BID_VOL_UP、ASK_VOL_UP：A 股的买一卖一量最小精度为 100 股。传入的数值会自动向下取整到最小精度的整数倍。
	// 其余到价提醒类型精度支持到小数点后 3 位

	message S2C
	{
		required int64 key = 1; //设置成功的情况下返回对应的key，不成功返回0
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}

.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 提醒类型参考 `PriceReminderType <base_define.html#priceremindertype>`_
	* 提醒频率类型参考 `PriceReminderFreq <base_define.html#pricereminderfreq>`_
	* 每只股票每种提醒类型最多设置10个
	* 限频接口：30秒内最多60次	
	
------------------------------------------------------------------------

`Qot_GetPriceReminder.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetPriceReminder.proto>`_ - 3221获取到价提醒
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetPriceReminder;
	option java_package = "com.futu.openapi.pb";

	import "Common.proto";
	import "Qot_Common.proto";

	// 提醒信息列表
	message PriceReminderItem
	{
		required int64 key = 1; // 每个提醒的唯一标识
		required int32 type = 2; // Qot_Common::PriceReminderType 提醒类型
		required double value = 3; // 提醒参数值
		required string note = 4; // 用户设置到价提醒时的标注
		required int32 freq = 5; // Qot_Common::PriceReminderFreq 提醒频率类型
		required bool isEnable = 6; // 该提醒设置是否生效。false不生效，true生效
	}

	message PriceReminder
	{
		required Qot_Common.Security security = 1; // 股票
		repeated PriceReminderItem itemList = 2; // 提醒信息列表
	}

	message C2S
	{
		optional Qot_Common.Security security = 1; // 查询股票下的到价提醒项，security和market二选一，都存在的情况下security优先。
		optional int32 market = 2; //Qot_Common::QotMarket 市场，查询市场下的到价提醒项，不区分沪深
	}

	message S2C
	{
		repeated PriceReminder priceReminderList = 1; //到价提醒
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}

.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_
	* 市场类型参考 `QotMarket <base_define.html#qotmarket>`_
	* 提醒类型参考 `PriceReminderType <base_define.html#priceremindertype>`_
	* 提醒频率类型参考 `PriceReminderFreq <base_define.html#pricereminderfreq>`_
	* 限频接口：30秒内最多10次	
	
	
`Qot_UpdatePriceReminder.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_UpdatePriceReminder.proto>`_ - 3019到价提醒通知
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_UpdatePriceReminder;
	option java_package = "com.futu.openapi.pb";

	import "Common.proto";
	import "Qot_Common.proto";

	enum MarketStatus
	{
		MarketStatus_Unknow = 0;
		MarketStatus_Open = 1; // 盘中
		MarketStatus_USPre = 2;  // 美股盘前
		MarketStatus_USAfter = 3; // 美股盘后
	}

	message S2C
	{
		required Qot_Common.Security security = 1; //股票
		required double price = 2; //价格
		required double changeRate = 3; //涨跌幅
		required int32 marketStatus = 4; //市场状态
		required string content = 5; //内容
		required string note = 6; //备注
		optional int64 key = 7; // 到价提醒的标识
		optional int32 type = 8; // Qot_Common::PriceReminderType，提醒类型
		optional double setValue = 9; // 设置的提醒值
		optional double curValue = 10; // 设置的提醒类型触发时当前值
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}


.. note::
	
	* 股票结构参考 `Security <base_define.html#security>`_

`Qot_GetUserSecurityGroup.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetUserSecurityGroup.proto>`_ - 3222获取自选股分组列表
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetUserSecurityGroup;
	option java_package = "com.futu.openapi.pb";

	import "Common.proto";
	import "Qot_Common.proto";
	
	// 自选股分组类型
	enum GroupType
	{
		GroupType_Unknown = 0; // 未知
		GroupType_Custom = 1; // 自定义分组
		GroupType_System = 2; // 系统分组
		GroupType_All = 3; // 全部分组
	}
	
	message C2S
	{
		required int32 groupType = 1; // GroupType,自选股分组类型。
	}
	
	message GroupData
	{
		required string groupName = 1; // 自选股分组名字
		required int32 groupType = 2; // GroupType,自选股分组类型。
	}
	
	message S2C
	{
		repeated GroupData groupList = 1; // 自选股分组列表
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}


.. note::
	
	* 限频接口：30秒内最多10次

`Qot_GetMarketState.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_GetMarketState.proto>`_ - 3223获取指定品种的市场状态
------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Qot_GetMarketState;
	option go_package = "github.com/futuopen/ftapi4go/pb/qotgetmarketstate";

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		repeated Qot_Common.Security securityList = 1; //股票列表
	}
	

	message MarketInfo
	{
		required Qot_Common.Security security = 1; //股票代码
		required string name = 2; // 股票名称
		required int32 marketState = 3; //Qot_Common.QotMarketState,市场状态
	}
	
	message S2C
	{
		repeated MarketInfo marketInfoList = 1; // 市场状态信息
	}

	message Request
	{
		required C2S c2s = 1;
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}


.. note::
	
	* 限频接口：30秒内最多10次
	* 每次请求的数据个数最多 400 个