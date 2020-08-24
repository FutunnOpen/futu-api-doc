基础定义
==========
	.. _KeepAlive: #keepalive-proto-1004
	.. _QotMarketState: #qotmarket
	.. _InitConnect: #initconnect-proto-1001
	
	这里对FutuOpenD开放协议接口中用到基本数据结构作出归档说明。

.. note::

    *   XXX.proto表示协议文件名, 点击超链接可打开github上的协议文件，每条协议内容以github上的为准，此文档更新可能存在滞后
    *   为避免增删导致的版本兼容问题，所有enum枚举类型只用于值的定义，在protobuf结构体中声明类型时使用int32类型
    *   所有类型定义使用protobuf格式声明，不同语言对接时请自行通过相关工具转换成对应的接口头文件
    
    
--------------


`InitConnect.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/InitConnect.proto>`_ - 1001初始化连接
--------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf
 
	import "Common.proto";
	message C2S
	{
		required int32 clientVer = 1; //客户端版本号，clientVer = "."以前的数 * 100 + "."以后的，举例：1.1版本的clientVer为1 * 100 + 1 = 101，2.21版本为2 * 100 + 21 = 221
		required string clientID = 2; //客户端唯一标识，无生具体生成规则，客户端自己保证唯一性即可
		optional bool recvNotify = 3; //此连接是否接收市场状态、交易需要重新解锁等等事件通知，true代表接收，FutuOpenD就会向此连接推送这些通知，反之false代表不接收不推送
		//如果通信要加密，首先得在FutuOpenD和客户端都配置RSA密钥，不配置始终不加密
		//如果配置了RSA密钥且指定的加密算法不为PacketEncAlgo_None则加密(即便这里不设置，配置了RSA密钥，也会采用默认加密方式)，默认采用FTAES_ECB算法
		optional int32 packetEncAlgo = 4; //指定包加密算法，参见Common.PacketEncAlgo的枚举定义
		optional int32 pushProtoFmt = 5; //指定这条连接上的推送协议格式，若不指定则使用push_proto_type配置项
	}
	
	message S2C
	{
		required int32 serverVer = 1; //FutuOpenD的版本号
		required uint64 loginUserID = 2; //FutuOpenD登陆的牛牛用户ID
		required uint64 connID = 3; //此连接的连接ID，连接的唯一标识
		required string connAESKey = 4; //此连接后续AES加密通信的Key，固定为16字节长字符串
		required int32 keepAliveInterval = 5; //心跳保活间隔
	}
	
	message Request
	{
		required C2S c2s = 1;
	}
	
	message Response
	{
		required int32 retType = 1 [default = -400]; //返回结果，参见Common.RetType的枚举定义
		optional string retMsg = 2; //返回结果描述
		optional int32 errCode = 3; //错误码，客户端一般通过retType和retMsg来判断结果和详情，errCode只做日志记录，仅在个别协议失败时对账用
		
		optional S2C s2c = 4;
	}

.. note::

    *   请求其它协议前必须等InitConnect协议先完成
    *   若FutuOpenD配置了加密， "connAESKey"将用于后续协议加密
    *   keepAliveInterval 为建议client发起心跳 KeepAlive_ 的间隔

------------------------------------------------------


`GetGlobalState.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/GetGlobalState.proto>`_ - 1002获取全局状态
--------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	import "Common.proto";
	import "Qot_Common.proto";

	message C2S
	{
		required uint64 userID = 1; //历史原因，目前已废弃，填0即可
	}

	message S2C
	{
		required int32 marketHK = 1; //Qot_Common.QotMarketState,港股主板市场状态 
		required int32 marketUS = 2; //Qot_Common.QotMarketState,美股Nasdaq市场状态 
		required int32 marketSH = 3; //Qot_Common.QotMarketState,沪市状态 
		required int32 marketSZ = 4; //Qot_Common.QotMarketState,深市状态 
		required int32 marketHKFuture = 5; //Qot_Common.QotMarketState,港股期货市场状态 
		optional int32 marketUSFuture = 15; //Qot_Common.QotMarketState,美国期货市场状态 
		required bool qotLogined = 6; //是否登陆行情服务器
		required bool trdLogined = 7; //是否登陆交易服务器
		required int32 serverVer = 8; //版本号
		required int32 serverBuildNo = 9; //buildNo
		required int64 time = 10; //当前服务器时间
		optional double localTime = 11; //当前本地时间
		optional Common.ProgramStatus programStatus = 12; //当前程序状态
		optional string qotSvrIpAddr = 13;
		optional string trdSvrIpAddr = 14;
		optional uint64 connID = 16; //此连接的连接ID，连接的唯一标识
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

    *   市场状态参考 QotMarketState_
    
--------------------------------------------------


`Notify.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Notify.proto>`_ - 1003系统推送通知
--------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	syntax = "proto2";
	package Notify;

	import "Common.proto";

	enum NotifyType
	{
		NotifyType_None = 0; //无
		NotifyType_GtwEvent = 1; //OpenD运行事件通知
		NotifyType_ProgramStatus = 2; //程序状态
		NotifyType_ConnStatus = 3; //连接状态
		NotifyType_QotRight = 4; //行情权限
		NotifyType_APILevel = 5; //用户等级
	}

	enum GtwEventType
	{
		GtwEventType_None = 0; //正常无错
		GtwEventType_LocalCfgLoadFailed	= 1; //加载本地配置失败
		GtwEventType_APISvrRunFailed = 2; //服务器启动失败
		GtwEventType_ForceUpdate = 3; //客户端版本过低
		GtwEventType_LoginFailed = 4; //登录失败
		GtwEventType_UnAgreeDisclaimer = 5; //未同意免责声明
		GtwEventType_NetCfgMissing = 6; //缺少必要网络配置信息;例如控制订阅额度 //已优化，不会再出现该情况
		GtwEventType_KickedOut = 7; //牛牛帐号在别处登录
		GtwEventType_LoginPwdChanged = 8; //登录密码被修改
		GtwEventType_BanLogin = 9; //用户被禁止登录
		GtwEventType_NeedPicVerifyCode = 10; //需要图形验证码
		GtwEventType_NeedPhoneVerifyCode = 11; //需要手机验证码
		GtwEventType_AppDataNotExist = 12; //程序自带数据不存在
		GtwEventType_NessaryDataMissing = 13; //缺少必要数据
		GtwEventType_TradePwdChanged = 14; //交易密码被修改
		GtwEventType_EnableDeviceLock = 15; //启用设备锁
	}

	message GtwEvent
	{
		required int32 eventType = 1; //GtwEventType,事件类型
		required string desc = 2; //事件描述
	}

	message ProgramStatus
	{
		required Common.ProgramStatus programStatus = 1; //当前程序状态
	}

	message ConnectStatus
	{
		required bool qotLogined = 1; //是否登陆行情服务器
		required bool trdLogined = 2; //是否登陆交易服务器
	}

	message QotRight
	{
		required int32 hkQotRight = 4; //港股行情权限, QotRight
		required int32 usQotRight = 5; //美股行情权限, QotRight
		required int32 cnQotRight = 6; //A股行情权限, QotRight
		optional int32 hkOptionQotRight = 7; //港股期权行情权限, Qot_Common.QotRight
		optional bool hasUSOptionQotRight = 8; //是否有美股期权行情权限
	}

	message APILevel
	{
		required string apiLevel = 3; //api用户等级描述
	}

	message S2C
	{
		required int32 type = 1; //通知类型
		optional GtwEvent event = 2; //事件通息
		optional ProgramStatus programStatus = 3; //程序状态
		optional ConnectStatus connectStatus = 4; //连接状态
		optional QotRight qotRight = 5; //行情权限
		optional APILevel apiLevel = 6; //用户等级
	}

	message Response
	{
		required int32 retType = 1 [default = -400]; //RetType,返回结果
		optional string retMsg = 2;
		optional int32 errCode = 3;
		
		optional S2C s2c = 4;
	}

	
.. note::

    *   Notify是系统推送协议
    *   FutuOpenD将内部的一些重要运行状态通知到client前端，可用于前端的管理平台监控处理
   
---------------------------------------------
	
	
`KeepAlive.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/KeepAlive.proto>`_ - 1004保活心跳
--------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: protobuf

	import "Common.proto";
	message C2S
	{
		required int64 time = 1; //客户端发包时的格林威治时间戳，单位秒
	}

	message S2C
	{
		required int64 time = 1; //服务器回包时的格林威治时间戳，单位秒
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

    *   心跳时间间隔由 InitConnect_ 协议返回，若FutuOpenD在30秒内未收到一次心跳，将主动断开连接
	
-----------------------------------


`Common.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Common.proto>`_ - 通用定义
--------------------------------------------------------------------------------------------------------------------------------------------

RetType - 协议返回值
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: protobuf

	//返回结果
	enum RetType
	{
		RetType_Succeed = 0; //成功
		RetType_Failed = -1; //失败
		RetType_TimeOut = -100; //超时
		RetType_Unknown = -400; //未知结果
	}

.. note::

    *   RetType 定义协议请求返回值
    *   请求失败情况，除网络超时外，其它具体原因参见各协议定义的retMsg字段
 
-------------------------------------

PacketID - 请求包标识
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: protobuf

	//包的唯一标识，用于回放攻击的识别和保护
	message PacketID
	{
		required uint64 connID = 1; //当前TCP连接的连接ID，一条连接的唯一标识，InitConnect协议会返回
		required uint32 serialNo = 2; //包头中的包自增序列号
	}

.. note::

    *   PacketID 用于唯一标识一次请求
    *   serailNO 由请求方自定义填入包头，为防回放攻击要求自增，否则新的请求将被忽略
 
-------------------------------------

PacketEncAlgo - 包加密算法
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: protobuf

	//包加密算法
	enum PacketEncAlgo
	{
		PacketEncAlgo_FTAES_ECB = 0; //富途修改过的AES的ECB加密模式
		PacketEncAlgo_None = -1; //不加密
		PacketEncAlgo_AES_ECB = 1; //标准的AES的ECB加密模式
		PacketEncAlgo_AES_CBC = 2; //标准的AES的CBC加密模式
	}

-------------------------------------

ProtoFmt - 协议格式
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: protobuf

	//协议格式，请求协议在请求头中指定，推送协议在Init时指定
	enum ProtoFmt
	{
		ProtoFmt_Protobuf = 0; //Google Protobuf格式
		ProtoFmt_Json = 1; //Json格式
	}

-------------------------------------

ProgramStatus - 程序的当前状态
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: protobuf

	//程序的当前状态
	enum ProgramStatusType
	{
		ProgramStatusType_None = 0;
		ProgramStatusType_Loaded = 1; //已完成类似加载配置,启动服务器等操作,服务器启动之前的状态无需返回

		ProgramStatusType_Loging = 2; //登录中
		ProgramStatusType_NeedPicVerifyCode = 3; //需要图形验证码
		ProgramStatusType_NeedPhoneVerifyCode = 4; //需要手机验证码
		ProgramStatusType_LoginFailed = 5; //登录失败,详细原因在描述返回
		ProgramStatusType_ForceUpdate = 6; //客户端版本过低

		ProgramStatusType_NessaryDataPreparing = 7; //正在拉取类似免责声明等一些必要信息
		ProgramStatusType_NessaryDataMissing = 8; //缺少必要信息
		ProgramStatusType_UnAgreeDisclaimer = 9; //未同意免责声明
		ProgramStatusType_Ready = 10; //可以接收业务协议收发,正常可用状态
		
		//OpenD登录后被强制退出登录，会导致连接全部断开,需要重连后才能得到以下该状态（并且需要在ui模式下）
		ProgramStatusType_ForceLogout = 11; //被强制退出登录,例如修改了登录密码,中途打开设备锁等,详细原因在描述返回
	}

	message ProgramStatus
	{
		required ProgramStatusType type = 1; //当前状态
		optional string strExtDesc = 2; // 额外描述
	}
 
-------------------------------------


`Qot_Common.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Qot_Common.proto>`_ - 行情通用定义
--------------------------------------------------------------------------------------------------------------------------------------------

QotMarket - 行情市场
~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum QotMarket
	{
		QotMarket_Unknown = 0; //未知市场
		QotMarket_HK_Security = 1; //港股
		QotMarket_US_Security = 11; //美股
		QotMarket_CNSH_Security = 21; //沪股
		QotMarket_CNSZ_Security = 22; //深股
	}

 .. note::

    *   QotMarket定义一支证券所属的行情市场分类
    *   QotMarket_HK_Future 港股期货，目前仅支持 999010(恒指当月期货)、999011(恒指下月期货)
	
----------------------------------------------

SecurityType - 证券类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum SecurityType
	{
		SecurityType_Unknown = 0; //未知
		SecurityType_Bond = 1; //债券
		SecurityType_Bwrt = 2; //一揽子权证
		SecurityType_Eqty = 3; //正股
		SecurityType_Trust = 4; //信托,基金
		SecurityType_Warrant = 5; //窝轮
		SecurityType_Index = 6; //指数
		SecurityType_Plate = 7; //板块
		SecurityType_Drvt = 8; //期权
		SecurityType_PlateSet = 9; //板块集
	}
	
-----------------------------------------------

PlateSetType - 板块集合类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum PlateSetType
	{
		PlateSetType_All = 0; //所有板块
		PlateSetType_Industry = 1; //行业板块
		PlateSetType_Region = 2; //地域板块,港美股市场的地域分类数据暂为空
		PlateSetType_Concept = 3; //概念板块
		PlateSetType_Other = 4; //其他板块, 仅用于3207（获取股票所属板块）协议返回,不可作为其他协议的请求参数
	}

 .. note::

    *   Qot_GetPlateSet 请求参数类型
	
-----------------------------------------------
 
WarrantType - 窝轮类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum WarrantType
	{
		WarrantType_Unknown = 0; //未知
		WarrantType_Buy = 1; //认购
		WarrantType_Sell = 2; //认沽
		WarrantType_Bull = 3; //牛
		WarrantType_Bear = 4; //熊
		WarrantType_InLine = 5; //界内证
	};

-----------------------------------------------
 
OptionType - 期权类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum OptionType
	{
		OptionType_Unknown = 0; //未知
		OptionType_Call = 1; //认购
		OptionType_Put = 2; //认沽
	};
 
-----------------------------------------------
 
IndexOptionType - 指数期权类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum IndexOptionType
	{
		IndexOptionType_Unknown = 0; //未知
		IndexOptionType_Normal = 1; //正常普通的指数期权
		IndexOptionType_Small = 2; //小型指数期权
	};
 
-----------------------------------------------
 
OptionAreaType - 期权类型（按行权时间）
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum OptionAreaType
	{
		OptionAreaType_Unknown = 0; //未知
		OptionAreaType_American = 1; //美式
		OptionAreaType_European = 2; //欧式
		OptionAreaType_Bermuda = 3; //百慕大
	};
 
-----------------------------------------------

QotMarketState - 行情市场状态
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
	
	enum QotMarketState
	{
		QotMarketState_None = 0; // 无交易,美股未开盘
		QotMarketState_Auction = 1; // 竞价 
		QotMarketState_WaitingOpen = 2; // 早盘前等待开盘
		QotMarketState_Morning = 3; // 早盘 
		QotMarketState_Rest = 4; // 午间休市 
		QotMarketState_Afternoon = 5; // 午盘 
		QotMarketState_Closed = 6; // 收盘
		QotMarketState_PreMarketBegin = 8; // 盘前
		QotMarketState_PreMarketEnd = 9; // 盘前结束 
		QotMarketState_AfterHoursBegin = 10; // 盘后
		QotMarketState_AfterHoursEnd = 11; // 盘后结束 
		QotMarketState_NightOpen = 13; // 夜市开盘 
		QotMarketState_NightEnd = 14; // 夜市收盘 
		QotMarketState_FutureDayOpen = 15; // 期指日市开盘 
		QotMarketState_FutureDayBreak = 16; // 期指日市休市 
		QotMarketState_FutureDayClose = 17; // 期指日市收盘 
		QotMarketState_FutureDayWaitForOpen = 18; // 期指日市等待开盘 
		QotMarketState_HkCas = 19; // 盘后竞价,港股市场增加CAS机制对应的市场状态
		QotMarketState_FutureNightWait = 20; // 夜市等待开盘
		QotMarketState_FutureAfternoon = 21; // 期货下午开盘
		//美国期货新增加状态
		QotMarketState_FutureSwitchDate = 22; // 期货切交易日
		QotMarketState_FutureOpen = 23; // 期货开盘
		QotMarketState_FutureBreak = 24; // 期货中盘休息
		QotMarketState_FutureBreakOver = 25; // 期货休息后开盘
		QotMarketState_FutureClose = 26; // 期货收盘
		//科创板新增状态
		QotMarketState_StibAfterHoursWait = 27; // 科创板的盘后撮合时段
		QotMarketState_StibAfterHoursBegin = 28; // 科创板的盘后交易开始
		QotMarketState_StibAfterHoursEnd = 29; // 科创板的盘后交易结束
	}
	

-----------------------------------------------

TradeDateType - 交易时间类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum TradeDateType
	{
		TradeDateType_Whole = 0; //全天交易
		TradeDateType_Morning = 1; //上午交易，下午休市
		TradeDateType_Afternoon = 2; //下午交易，上午休市
	}

-----------------------------------------------

TradeDateMarket - 交易日查询市场
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: protobuf

	enum TradeDateMarket
	{
		TradeDateMarket_Unknown = 0; //未知
		TradeDateMarket_HK = 1; //港股市场
		TradeDateMarket_US = 2; //美股市场
		TradeDateMarket_CN = 3; //A股市场
		TradeDateMarket_NT = 4; //深（沪）股通
		TradeDateMarket_ST = 5; //港股通（深、沪）
	}

-----------------------------------------------

RehabType - K线复权类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum RehabType
	{
		RehabType_None = 0; //不复权
		RehabType_Forward = 1; //前复权
		RehabType_Backward = 2; //后复权
	}
	
-----------------------------------------------

KLType - K线类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	 //枚举值兼容旧协议定义
	 //新类型季K,年K,3分K暂时没有支持历史K线
	enum KLType
	{
		KLType_1Min = 1; //1分K
		KLType_Day = 2; //日K
		KLType_Week = 3; //周K
		KLType_Month = 4; //月K	
		KLType_Year = 5; //年K
		KLType_5Min = 6; //5分K
		KLType_15Min = 7; //15分K
		KLType_30Min = 8; //30分K
		KLType_60Min = 9; //60分K		
		KLType_3Min = 10; //3分K
		KLType_Quarter = 11; //季K
	}
	
-----------------------------------------------

KLFields - K线数据字段
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum KLFields
	{
		KLFields_High = 1; //最高价
		KLFields_Open = 2; //开盘价
		KLFields_Low = 4; //最低价
		KLFields_Close = 8; //收盘价
		KLFields_LastClose = 16; //昨收价
		KLFields_Volume = 32; //成交量
		KLFields_Turnover = 64; //成交额
		KLFields_TurnoverRate = 128; //换手率
		KLFields_PE = 256; //市盈率
		KLFields_ChangeRate = 512; //涨跌幅
	}
		
-----------------------------------------------

SubType - 行情定阅类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	 //订阅类型
	 //枚举值兼容旧协议定义
	enum SubType
	{
		SubType_None = 0;
		SubType_Basic = 1; //基础报价
		SubType_OrderBook = 2; //摆盘     
		SubType_Ticker = 4; //逐笔
		SubType_RT = 5; //分时
		SubType_KL_Day = 6; //日K
		SubType_KL_5Min = 7; //5分K
		SubType_KL_15Min = 8; //15分K
		SubType_KL_30Min = 9; //30分K
		SubType_KL_60Min = 10; //60分K
		SubType_KL_1Min = 11; //1分K
		SubType_KL_Week = 12; //周K
		SubType_KL_Month = 13; //月K
		SubType_Broker = 14; //经纪队列
		SubType_KL_Qurater = 15; //季K
		SubType_KL_Year = 16; //年K
		SubType_KL_3Min = 17; //3分K
	}
	
-----------------------------------------------

TickerDirection - 逐笔方向
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum TickerDirection
	{
		TickerDirection_Bid = 1; //外盘
		TickerDirection_Ask = 2; //内盘
		TickerDirection_Neutral = 3; //中性盘
	}
	
-----------------------------------------------

TickerType - 逐笔类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	 //逐笔类型
	enum TickerType
	{
		TickerType_Unknown = 0; //未知
		TickerType_Automatch = 1; //自动对盘
		TickerType_Late = 2; //开市前成交盘
		TickerType_NoneAutomatch = 3; //非自动对盘
		TickerType_InterAutomatch = 4; //同一证券商自动对盘
		TickerType_InterNoneAutomatch = 5; //同一证券商非自动对盘
		TickerType_OddLot = 6; //碎股交易
		TickerType_Auction = 7; //竞价交易
		TickerType_Bulk = 8; //批量交易
		TickerType_Crash = 9; //现金交易
		TickerType_CrossMarket = 10; //跨市场交易
		TickerType_BulkSold = 11; //批量卖出
		TickerType_FreeOnBoard = 12; //离价交易
		TickerType_Rule127Or155 = 13; //第127条交易（纽交所规则）或第155条交易
		TickerType_Delay = 14; //延迟交易
		TickerType_MarketCenterClosePrice = 15; //中央收市价
		TickerType_NextDay = 16; //隔日交易
		TickerType_MarketCenterOpening = 17; //中央开盘价交易
		TickerType_PriorReferencePrice = 18; //前参考价
		TickerType_MarketCenterOpenPrice = 19; //中央开盘价
		TickerType_Seller = 20; //卖方
		TickerType_T = 21; //T类交易(盘前和盘后交易)
		TickerType_ExtendedTradingHours = 22; //延长交易时段
		TickerType_Contingent = 23; //合单交易
		TickerType_AvgPrice = 24; //平均价成交
		TickerType_OTCSold = 25; //场外售出
		TickerType_OddLotCrossMarket = 26; //碎股跨市场交易
		TickerType_DerivativelyPriced = 27; //衍生工具定价
		TickerType_ReOpeningPriced = 28; //再开盘定价
		TickerType_ClosingPriced = 29; //收盘定价
		TickerType_ComprehensiveDelayPrice = 30; //综合延迟价格
		TickerType_Overseas = 31; //交易的一方不是香港交易所的成员，属于场外交易
	}
	
-----------------------------------------------

PushDataType - 推送数据来源分类，目前只有逐笔在使用
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	 //推送数据来源分类，目前只有逐笔在使用
	enum PushDataType
	{
		PushDataType_Unknow = 0;
		PushDataType_Realtime = 1; //实时推送的数据
		PushDataType_ByDisConn = 2; //对后台行情连接断开期间拉取补充的数据 最多50个
		PushDataType_Cache = 3; //非实时非连接断开补充数据
	}
	
-----------------------------------------------

DarkStatus - 暗盘交易状态
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	 //暗盘交易状态
	enum DarkStatus
	{
		DarkStatus_None = 0; //无暗盘交易
		DarkStatus_Trading = 1; //暗盘交易中
		DarkStatus_End = 2; //暗盘交易结束
	}
	
-----------------------------------------------

SecurityStatus - 股票状态
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	 //暗盘交易状态
	enum SecurityStatus
	{
		SecurityStatus_Unknown = 0; //未知
		SecurityStatus_Normal = 1; //正常状态
		SecurityStatus_Listing = 2; //待上市
		SecurityStatus_Purchasing = 3; //申购中
		SecurityStatus_Subscribing = 4; //认购中
		SecurityStatus_BeforeDrakTradeOpening = 5; //暗盘开盘前
		SecurityStatus_DrakTrading = 6; //暗盘交易中
		SecurityStatus_DrakTradeEnd = 7; //暗盘已收盘
		SecurityStatus_ToBeOpen = 8; //待开盘
		SecurityStatus_Suspended = 9; //停牌
		SecurityStatus_Called = 10; //已收回
		SecurityStatus_ExpiredLastTradingDate = 11; //已过最后交易日
		SecurityStatus_Expired = 12; //已过期
		SecurityStatus_Delisted = 13; //已退市
		SecurityStatus_ChangeToTemporaryCode = 14; //公司行动中，交易关闭，转至临时代码交易
		SecurityStatus_TemporaryCodeTradeEnd = 15; //临时买卖结束，交易关闭
		SecurityStatus_ChangedPlateTradeEnd = 16; //已转板，旧代码交易关闭
		SecurityStatus_ChangedCodeTradeEnd = 17; //已换代码，旧代码交易关闭
		SecurityStatus_RecoverableCircuitBreaker = 18; //可恢复性熔断
		SecurityStatus_UnRecoverableCircuitBreaker = 19; //不可恢复性熔断
		SecurityStatus_AfterCombination = 20; //盘后撮合
		SecurityStatus_AfterTransation = 21; //盘后交易
	}
	
-----------------------------------------------

CompanyAct - 公司行动
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	 //公司行动
	enum CompanyAct
	{
		CompanyAct_None = 0; //无
		CompanyAct_Split = 1; //拆股		
		CompanyAct_Join = 2; //合股
		CompanyAct_Bonus = 4; //送股
		CompanyAct_Transfer = 8; //转赠股
		CompanyAct_Allot = 16; //配股	
		CompanyAct_Add = 32; //增发股
		CompanyAct_Dividend = 64; //现金分红
		CompanyAct_SPDividend = 128; //特别股息	
	}
	
-----------------------------------------------

HolderCategory - 持有者类别
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum HolderCategory
	{
		HolderCategory_Unknow = 0; //未知
		HolderCategory_Agency = 1; //机构
		HolderCategory_Fund = 2; //基金
		HolderCategory_SeniorManager = 3; //高管
	}
	
-----------------------------------------------

SortField - 窝轮排序
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum SortField
	{
		SortField_Unknow = 0;
		SortField_Code = 1; //代码
		SortField_CurPrice = 2; //最新价
		SortField_PriceChangeVal = 3; //涨跌额
		SortField_ChangeRate = 4; //涨跌幅%
		SortField_Status = 5; //状态
		SortField_BidPrice = 6; //买入价
		SortField_AskPrice = 7; //卖出价
		SortField_BidVol = 8; //买量
		SortField_AskVol = 9; //卖量
		SortField_Volume = 10; //成交量
		SortField_Turnover = 11; //成交额
		SortField_Amplitude = 30; //振幅%

		//以下排序字段只支持用于Qot_GetWarrant协议
		SortField_Score = 12; //综合评分
		SortField_Premium = 13; //溢价%
		SortField_EffectiveLeverage = 14; //有效杠杆
		SortField_Delta = 15; //对冲值,仅认购认沽支持该字段
		SortField_ImpliedVolatility = 16; //引伸波幅,仅认购认沽支持该字段
		SortField_Type = 17; //类型
		SortField_StrikePrice = 18; //行权价
		SortField_BreakEvenPoint = 19; //打和点
		SortField_MaturityTime = 20; //到期日
		SortField_ListTime = 21; //上市日期
		SortField_LastTradeTime = 22; //最后交易日
		SortField_Leverage = 23; //杠杆比率
		SortField_InOutMoney = 24; //价内/价外%
		SortField_RecoveryPrice = 25; //收回价,仅牛熊证支持该字段
		SortField_ChangePrice = 26; // 换股价
		SortField_Change = 27; //换股比率
		SortField_StreetRate = 28; //街货比%
		SortField_StreetVol = 29; //街货量
		SortField_WarrantName = 31; // 窝轮名称
		SortField_Issuer = 32; //发行人
		SortField_LotSize = 33; // 每手
		SortField_IssueSize = 34; //发行量
		SortField_UpperStrikePrice = 45; //上限价，仅用于界内证
		SortField_LowerStrikePrice = 46; //下限价，仅用于界内证
		SortField_InLinePriceStatus = 47; //界内界外，仅用于界内证

		//以下排序字段只支持用于Qot_GetPlateSecurity协议，并仅支持美股
		SortField_PreCurPrice = 35; //盘前最新价
		SortField_AfterCurPrice = 36; //盘后最新价
		SortField_PrePriceChangeVal = 37; //盘前涨跌额
		SortField_AfterPriceChangeVal = 38; //盘后涨跌额
		SortField_PreChangeRate = 39; //盘前涨跌幅%
		SortField_AfterChangeRate = 40; //盘后涨跌幅%
		SortField_PreAmplitude = 41; //盘前振幅%
		SortField_AfterAmplitude = 42; //盘后振幅%
		SortField_PreTurnover = 43; //盘前成交额
		SortField_AfterTurnover = 44; //盘后成交额
	}
	
-----------------------------------------------

Issuer - 窝轮发行人
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum Issuer
	{
		Issuer_Unknow = 0; //未知
		Issuer_SG = 1; //法兴
		Issuer_BP = 2; //法巴
		Issuer_CS = 3; //瑞信
		Issuer_CT = 4; //花旗	
		Issuer_EA = 5; //东亚
		Issuer_GS = 6; //高盛
		Issuer_HS = 7; //汇丰
		Issuer_JP = 8; //摩通	
		Issuer_MB = 9; //麦银	
		Issuer_SC = 10; //渣打
		Issuer_UB = 11; //瑞银
		Issuer_BI = 12; //中银
		Issuer_DB = 13; //德银
		Issuer_DC = 14; //大和
		Issuer_ML = 15; //美林
		Issuer_NM = 16; //野村
		Issuer_RB = 17; //荷合
		Issuer_RS = 18; //苏皇	
		Issuer_BC = 19; //巴克莱
		Issuer_HT = 20; //海通
		Issuer_VT = 21; //瑞通
		Issuer_KC = 22; //比联
		Issuer_MS = 23; //摩利
	}
	
-----------------------------------------------

IpoPeriod - 窝轮上市日
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum IpoPeriod
	{
		IpoPeriod_Unknow = 0; //未知
		IpoPeriod_Today = 1; //今日上市
		IpoPeriod_Tomorrow = 2; //明日上市
		IpoPeriod_Nextweek = 3; //未来一周上市
		IpoPeriod_Lastweek = 4; //过去一周上市
		IpoPeriod_Lastmonth = 5; //过去一月上市
	}
	
-----------------------------------------------

PriceType - 窝轮价(界)外/内
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum PriceType
	{
		PriceType_Unknow = 0;
		PriceType_Outside = 1; //价外,界内证表示界外
		PriceType_WithIn = 2; //价内,界内证表示界内
	}
	
-----------------------------------------------

WarrantStatus - 窝轮状态
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum WarrantStatus
	{
		WarrantStatus_Unknow = 0; //未知
		WarrantStatus_Normal = 1; //正常状态
		WarrantStatus_Suspend = 2; //停牌
		WarrantStatus_StopTrade = 3; //终止交易
		WarrantStatus_PendingListing = 4; //等待上市
	}
			
-----------------------------------------------

QotRight - 行情权限
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum QotRight
	{
		QotRight_Unknow = 0; //未知
		QotRight_Bmp = 1; //Bmp，无法订阅
		QotRight_Level1 = 2; //Level1
		QotRight_Level2 = 3; //Level2
		QotRight_SF = 4; //SF高级行情
	}
				
-----------------------------------------------------------------------------

StockField - 条件选股的简单属性筛选条件
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum StockField 
	{
		StockField_Unknown = 0; // 未知
		StockField_StockCode = 1; // 股票代码，不能填区间上下限值。
		StockField_StockName = 2; // 股票名称，不能填区间上下限值。
		StockField_CurPrice = 3; // 最新价 例如填写[10,20]值区间
		StockField_CurPriceToHighest52WeeksRatio = 4; // (现价 - 52周最高)/52周最高，对应PC端离52周高点百分比 例如填写[-0.8,0]值区间
		StockField_CurPriceToLowest52WeeksRatio = 5; // (现价 - 52周最低)/52周最低，对应PC端离52周低点百分比 例如填写[0,100]值区间
		StockField_HighPriceToHighest52WeeksRatio = 6; // (今日最高 - 52周最高)/52周最高 例如填写[-0.8,0]值区间
		StockField_LowPriceToLowest52WeeksRatio = 7; // (今日最低 - 52周最低)/52周最低 例如填写[0,100]值区间
		StockField_VolumeRatio = 8; // 量比 例如填写[0.5,30]值区间
		StockField_BidAskRatio = 9; // 委比 例如填写[-20,85.01]值区间
		StockField_LotPrice = 10; // 每手价格 例如填写[40,100]值区间
		StockField_MarketVal = 11; // 市值 例如填写[50000000,3000000000]值区间
		StockField_PeAnnual = 12; // 市盈率 (静态) 例如填写[-8,65.3]值区间
		StockField_PeTTM = 13; // 市盈率TTM 例如填写[-10,20.5]值区间
		StockField_PbRate = 14; // 市净率 例如填写[0,0.8]值区间
		StockField_ChangeRate5min = 15; // 五分钟价格涨跌幅 例如填写[-5,6.3]值区间
		StockField_ChangeRateBeginYear = 16; // 年初至今价格涨跌幅 例如填写[-50.1,400.7]值区间
		
		// 基础量价属性
		StockField_PSTTM = 17; // 市销率(TTM) 例如填写 [100, 500] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%） 
		StockField_PCFTTM = 18; // 市现率(TTM) 例如填写 [100, 1000] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
		StockField_TotalShare = 19; // 总股数 例如填写 [1000000000,1000000000] 值区间 (单位：股)
		StockField_FloatShare = 20; // 流通股数 例如填写 [1000000000,1000000000] 值区间 (单位：股)
		StockField_FloatMarketVal = 21; // 流通市值 例如填写 [1000000000,1000000000] 值区间 (单位：元)
	}
			
-----------------------------------------------------------------------------

AccumulateField - 条件选股的累积属性筛选条件
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum AccumulateField
	{
		AccumulateField_Unknown = 0; // 未知
		AccumulateField_ChangeRate = 1; // 涨跌幅 例如填写[-10.2,20.4]值区间
		AccumulateField_Amplitude = 2; // 振幅 例如填写[0.5,20.6]值区间
		AccumulateField_Volume = 3; // 日均成交量 例如填写[2000,70000]值区间
		AccumulateField_Turnover = 4; // 日均成交额 例如填写[1400,890000]值区间
		AccumulateField_TurnoverRate = 5; // 换手率 例如填写[0.12,26.73]值区间
	}
			
-----------------------------------------------------------------------------

FinancialField - 条件选股的财务属性筛选条件
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum FinancialField
	{
		// 基础财务属性
		FinancialField_Unknown = 0; // 未知
		FinancialField_NetProfit = 1; // 净利润 例如填写[100000000,2500000000]值区间
		FinancialField_NetProfitGrowth = 2; // 净利润增长率 例如填写[-20.13,300]值区间
		FinancialField_SumOfBusiness = 3; // 营业收入 例如填写[100000000,6400000000]值区间
		FinancialField_SumOfBusinessGrowth = 4; // 营收同比增长率 例如填写[-5.13,200]值区间
		FinancialField_NetProfitRate = 5; // 净利率 例如填写[0.01,113]值区间
		FinancialField_GrossProfitRate = 6; // 毛利率 例如填写[0.12,65]值区间
		FinancialField_DebtAssetRate = 7; // 资产负债率 例如填写[0.05,470]值区间
		FinancialField_ReturnOnEquityRate = 8; // 净资产收益率 例如填写[0.01,230]值区间
		
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
		FinancialField_NetProfitCashCover = 19; // 盈利中的现金收入比例 例如填写 [1.0,60.0] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%。仅适用于年报。）
		
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
		FinancialField_AccountsReceivable = 31; // 应收帐款净额 例如填写 [1000000000,1000000000] 值区间 例如填写 [1000000000,1000000000] 值区间 （单位：元）	
		
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
-----------------------------------------------------------------------------

FinancialQuarter - 条件选股的财报时间周期
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum FinancialQuarter
	{
		FinancialQuarter_Unknown = 0; // 未知
		FinancialQuarter_Annual = 1; // 年报
		FinancialQuarter_FirstQuarter = 2; // 一季报
		FinancialQuarter_Interim = 3; // 中报
		FinancialQuarter_ThirdQuarter = 4; // 三季报
		FinancialQuarter_MostRecentQuarter = 5; // 最近季报
	}

-----------------------------------------------------------------------------

SortDir - 条件选股的排序方向
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum SortDir
	{
		SortDir_No = 0; // 不排序
		SortDir_Ascend = 1; // 升序
		SortDir_Descend = 2; // 降序
	}

-----------------------------------------------------------------------------

Security - 证券标识
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message Security
	{
		required int32 market = 1; //QotMarket,股票市场
		required string code = 2; //股票代码
	}

-----------------------------------------------

KLine - K线数据点
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message KLine
	{
		required string time = 1; //时间戳字符串
		required bool isBlank = 2; //是否是空内容的点,若为true则只有时间信息
		optional double highPrice = 3; //最高价
		optional double openPrice = 4; //开盘价
		optional double lowPrice = 5; //最低价
		optional double closePrice = 6; //收盘价
		optional double lastClosePrice = 7; //昨收价
		optional int64 volume = 8; //成交量
		optional double turnover = 9; //成交额
		optional double turnoverRate = 10; //换手率（该字段为百分比字段，展示为小数表示）
		optional double pe = 11; //市盈率
		optional double changeRate = 12; //涨跌幅（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		optional double timestamp = 13; //时间戳
	}

----------------------------------------------------------------------------------------------

OptionBasicQotExData - 基础报价的期权特有字段
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message OptionBasicQotExData
	{
		required double strikePrice = 1; //行权价
		required int32 contractSize = 2; //每份合约数(整型数据)
		optional double contractSizeFloat = 17; //每份合约数（浮点型数据）
		required int32 openInterest = 3; //未平仓合约数
		required double impliedVolatility = 4; //隐含波动率（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		required double premium = 5; //溢价（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		required double delta = 6; //希腊值 Delta
		required double gamma = 7; //希腊值 Gamma
		required double vega = 8; //希腊值 Vega
		required double theta = 9; //希腊值 Theta
		required double rho = 10; //希腊值 Rho
		
		//以下字段仅支持港股期权
		optional int32 netOpenInterest = 11; //净未平仓合约数
		optional int32 expiryDateDistance = 12; //距离到期日天数
		optional double contractNominalValue = 13; //合约名义金额
		optional double ownerLotMultiplier = 14; //相等正股手数，指数期权无该字段
		optional int32 optionAreaType = 15; //OptionAreaType, 期权类型（按行权时间）
		optional double contractMultiplier = 16; //合约乘数，指数期权特有字段
	}		

----------------------------------------------------------------------------------------------

FutureBasicQotExData - 基础报价的期货特有字段
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message FutureBasicQotExData
	{
		required double lastSettlePrice = 1; //昨结
		required int32 position = 2; //持仓量
		required int32 positionChange = 3; //日增仓
		optional int32 expiryDateDistance = 4; //距离到期日天数
	}		

----------------------------------------------------------------------------------------------

BasicQot - 基础报价
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message BasicQot
	{
		required Security security = 1; //股票
		required bool isSuspended = 2; //是否停牌
		required string listTime = 3; //上市日期字符串
		required double priceSpread = 4; //价差
		required string updateTime = 5; //最新价的更新时间字符串，对其他字段不适用
		required double highPrice = 6; //最高价
		required double openPrice = 7; //开盘价
		required double lowPrice = 8; //最低价
		required double curPrice = 9; //最新价
		required double lastClosePrice = 10; //昨收价
		required int64 volume = 11; //成交量
		required double turnover = 12; //成交额
		required double turnoverRate = 13; //换手率（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		required double amplitude = 14; //振幅（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		optional int32 darkStatus = 15; //DarkStatus, 暗盘交易状态	
		optional OptionBasicQotExData optionExData = 16; //期权特有字段
		optional double listTimestamp = 17; //上市日期时间戳
		optional double updateTimestamp = 18; //最新价的更新时间戳，对其他字段不适用
		optional PreAfterMarketData preMarket = 19; //盘前数据
		optional PreAfterMarketData afterMarket = 20; //盘后数据
		optional int32 secStatus = 21; //SecurityStatus, 股票状态
		optional FutureBasicQotExData futureExData = 22; //期货特有字段
	}
		
-----------------------------------------------------------------------------------------------------------

PreAfterMarketData - 盘前盘后数据
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 
 .. code-block:: protobuf
	
	//美股支持盘前盘后数据
	//科创板仅支持盘后数据：成交量，成交额
	message PreAfterMarketData
	{
	    optional double price = 1;  // 盘前或盘后 - 价格
	    optional double highPrice = 2;  // 盘前或盘后 - 最高价
	    optional double lowPrice = 3;  // 盘前或盘后 - 最低价
	    optional int64 volume = 4;  // 盘前或盘后 - 成交量
	    optional double turnover = 5;  // 盘前或盘后 - 成交额
	    optional double changeVal = 6;  // 盘前或盘后 - 涨跌额
	    optional double changeRate = 7;  // 盘前或盘后 - 涨跌幅（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
	    optional double amplitude = 8;  // 盘前或盘后 - 振幅（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
	}

-----------------------------------------------------------------------------------------------------------

TimeShare - 分时数据点
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message TimeShare
	{
		required string time = 1; //时间字符串
		required int32 minute = 2; //距离0点过了多少分钟
		required bool isBlank = 3; //是否是空内容的点,若为true则只有时间信息
		optional double price = 4; //当前价
		optional double lastClosePrice = 5; //昨收价
		optional double avgPrice = 6; //均价
		optional int64 volume = 7; //成交量
		optional double turnover = 8; //成交额
		optional double timestamp = 9; //时间戳
	}

-----------------------------------------------

SecurityStaticBasic - 证券基本静态信息
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message SecurityStaticBasic
	{
		required Qot_Common.Security security = 1; //股票
		required int64 id = 2; //股票ID
		required int32 lotSize = 3; //每手数量,期权类型表示一份合约的股数
		required int32 secType = 4; //Qot_Common.SecurityType,股票类型
		required string name = 5; //股票名字
		required string listTime = 6; //上市时间字符串
		optional bool delisting = 7; //是否退市
		optional double listTimestamp = 8; //上市时间戳
	}

-----------------------------------------------

WarrantStaticExData - 窝轮额外静态信息
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message WarrantStaticExData
	{
		required int32 type = 1; //Qot_Common.WarrantType,窝轮类型
		required Qot_Common.Security owner = 2; //所属正股
	}
			
-----------------------------------------------

OptionStaticExData - 期权额外静态信息
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message OptionStaticExData
	{
		required int32 type = 1; //Qot_Common.OptionType,期权
		required Qot_Common.Security owner = 2; //标的股
		required string strikeTime = 3; //行权日
		required double strikePrice = 4; //行权价
		required bool suspend = 5; //是否停牌
		required string market = 6; //发行市场名字
		optional double strikeTimestamp = 7; //行权日时间戳
		optional int32 indexOptionType = 8; //Qot_Common.IndexOptionType, 指数期权的类型，仅在指数期权有效
	}
			
-----------------------------------------------

FutureStaticExData - 期货额外静态信息
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message FutureStaticExData
	{
		required string lastTradeTime = 1; //最后交易日，只有非主连期货合约才有该字段
		optional double lastTradeTimestamp = 2; //最后交易日时间戳，只有非主连期货合约才有该字段
		required bool isMainContract = 3; //是否主连合约
	}
			
-----------------------------------------------

SecurityStaticInfo - 证券静态信息
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message SecurityStaticInfo
	{
		required SecurityStaticBasic basic = 1; //基本股票静态信息
		optional WarrantStaticExData warrantExData = 2; //窝轮额外股票静态信息
		optional OptionStaticExData optionExData = 3; //期权额外股票静态信息
		optional FutureStaticExData futureExData = 4; //期货额外股票静态信息
	}
	
-----------------------------------------------

Broker - 买卖经纪摆盘
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message Broker
	{
		required int64 id = 1; //经纪ID
		required string name = 2; //经纪名称
		required int32 pos = 3; //经纪档位
		
		//以下为SF行情特有字段
		optional int64 orderID = 4; //交易所订单ID，与交易接口返回的订单ID并不一样
		optional int64 volume = 5; //订单股数
	}
	
-----------------------------------------------


Ticker - 逐笔成交
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message Ticker
	{
		required string time = 1; //时间字符串
		required int64 sequence = 2; // 唯一标识
		required int32 dir = 3; //TickerDirection, 买卖方向
		required double price = 4; //价格
		required int64 volume = 5; //成交量
		required double turnover = 6; //成交额
		optional double recvTime = 7; //收到推送数据的本地时间戳，用于定位延迟
		optional int32 type = 8; //TickerType, 逐笔类型
		optional int32 typeSign = 9; //逐笔类型符号
		optional int32 pushDataType = 10; //用于区分推送情况，仅推送时有该字段
		optional double timestamp = 11; //时间戳
	}
	
-----------------------------------------------

OrderBookDetail - 买卖档明细
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message OrderBookDetail
	{
		required int64 orderID = 1; //交易所订单ID，与交易接口返回的订单ID并不一样
		required int64 volume = 2; //订单股数
	}
	
-----------------------------------------------

OrderBook - 买卖档
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message OrderBook
	{
		required double price = 1; //委托价格
		required int64 volume = 2; //委托数量
		required int32 orederCount = 3; //委托订单个数
		repeated OrderBookDetail detailList = 4; //订单信息，SF行情特有
	}
	
-----------------------------------------------

ShareHoldingChange - 持股变动
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message ShareHoldingChange
	{
		required string holderName = 1; //持有者名称（机构名称 或 基金名称 或 高管姓名）
		required double holdingQty = 2; //当前持股数量
		required double holdingRatio = 3; //当前持股百分比（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
		required double changeQty = 4; //较上一次变动数量
		required double changeRatio = 5; //较上一次变动百分比（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%。是相对于自身的比例，而不是总的。如总股本1万股，持有100股，持股百分比是1%，卖掉50股，变动比例是50%，而不是0.5%）
		required string time = 6; //发布时间(YYYY-MM-DD HH:MM:SS字符串)
		optional double timestamp = 7; //时间戳
	}
	
-----------------------------------------------

SubInfo - 单个定阅类型信息
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message SubInfo
	{
		required int32 subType = 1;  //Qot_Common.SubType,订阅类型
		repeated Qot_Common.Security securityList = 2; 	//订阅该类型行情的证券
	}
	
-----------------------------------------------

ConnSubInfo - 单条连接定阅信息
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message ConnSubInfo
	{
		repeated SubInfo subInfoList = 1; //该连接订阅信息
		required int32 usedQuota = 2; //该连接已经使用的订阅额度
		required bool isOwnConnData = 3; //用于区分是否是自己连接的数据
	}

 .. note::

    *   一条连接重复定阅其它连接已经订阅过的，不会额外消耗订阅额度
	
	-----------------------------------------------

PlateInfo - 单条连接定阅信息
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message PlateInfo
	{
		required Qot_Common.Security plate = 1; //板块
		required string name = 2; //板块名字
		optional int32 plateType = 3; //PlateSetType 板块类型, 仅3207（获取股票所属板块）协议返回该字段
	}
	
-----------------------------------------------

Rehab - 复权信息
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message Rehab
	{
		required string time = 1; //时间字符串
		required int64 companyActFlag = 2; //公司行动(CompanyAct)组合标志位,指定某些字段值是否有效
		required double fwdFactorA = 3; //前复权因子A
		required double fwdFactorB = 4; //前复权因子B
		required double bwdFactorA = 5; //后复权因子A
		required double bwdFactorB = 6; //后复权因子B
		optional int32 splitBase = 7; //拆股(例如，1拆5，Base为1，Ert为5)
		optional int32 splitErt = 8;	
		optional int32 joinBase = 9; //合股(例如，50合1，Base为50，Ert为1)
		optional int32 joinErt = 10;	
		optional int32 bonusBase = 11; //送股(例如，10送3, Base为10,Ert为3)
		optional int32 bonusErt = 12;	
		optional int32 transferBase = 13; //转赠股(例如，10转3, Base为10,Ert为3)
		optional int32 transferErt = 14;	
		optional int32 allotBase = 15; //配股(例如，10送2, 配股价为6.3元, Base为10, Ert为2, Price为6.3)
		optional int32 allotErt = 16;	
		optional double allotPrice = 17;	
		optional int32 addBase = 18; //增发股(例如，10送2, 增发股价为6.3元, Base为10, Ert为2, Price为6.3)
		optional int32 addErt = 19;	
		optional double addPrice = 20;	
		optional double dividend = 21; //现金分红(例如，每10股派现0.5元,则该字段值为0.05)
		optional double spDividend = 22; //特别股息(例如，每10股派特别股息0.5元,则该字段值为0.05)
		optional double timestamp = 23; //时间戳
	}
	
-----------------------------------------------

PriceReminderType - 提醒类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	//提醒类型
	enum PriceReminderType
	{
		PriceReminderType_Unknown = 0; // 未知
		PriceReminderType_PriceUp = 1; // 价格涨到
		PriceReminderType_PriceDown = 2; // 价格跌到
		PriceReminderType_ChangeRateUp = 3; // 日涨幅超，该字段为百分比字段，设置时填20表示20%
		PriceReminderType_ChangeRateDown = 4; // 日跌幅超，该字段为百分比字段，设置时填20表示20%
		PriceReminderType_5MinChangeRateUp = 5; // 5分钟涨幅超，该字段为百分比字段，设置时填20表示20%
		PriceReminderType_5MinChangeRateDown = 6; // 5分钟跌幅超，该字段为百分比字段，设置时填20表示20%
		PriceReminderType_VolumeUp = 7; // 成交量超过
		PriceReminderType_TurnoverUp = 8; // 成交额超过
		PriceReminderType_TurnoverRateUp = 9; // 换手率超过，该字段为百分比字段，设置时填20表示20%
		PriceReminderType_BidPriceUp = 10; // 买一价高于
		PriceReminderType_AskPriceDown = 11; // 卖一价低于
		PriceReminderType_BidVolUp = 12; // 买一量高于	
		PriceReminderType_AskVolUp = 13; // 卖一量高于
	}

-----------------------------------------------

PriceReminderFreq - 提醒频率
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	//提醒频率
	enum PriceReminderFreq
	{
		PriceReminderFreq_Unknown = 0; // 未知
		PriceReminderFreq_Always = 1; // 持续提醒
		PriceReminderFreq_OnceADay = 2; // 每日一次
		PriceReminderFreq_OnlyOnce = 3; // 仅提醒一次
	}

-----------------------------------------------

AssetClass - 资产类别
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	//资产类别
	enum AssetClass
	{
		AssetClass_Unknow = 0; //未知
		AssetClass_Stock = 1; //股票
		AssetClass_Bond = 2; //债券
		AssetClass_Commodity = 3; //商品
		AssetClass_CurrencyMarket = 4; //货币市场
		AssetClass_Future = 5; //期货
		AssetClass_Swap = 6; //掉期
	}

-----------------------------------------------

`Trd_Common.proto <https://github.com/FutunnOpen/py-futu-api/tree/master/futu/common/pb/Trd_Common.proto>`_ - 交易通用定义
--------------------------------------------------------------------------------------------------------------------------------------------

OrderStatus - 订单状态
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum OrderStatus
	{
		OrderStatus_Unsubmitted = 0; //未提交
		OrderStatus_Unknown = -1; //未知状态
		OrderStatus_WaitingSubmit = 1; //等待提交
		OrderStatus_Submitting = 2; //提交中
		OrderStatus_SubmitFailed = 3; //提交失败，下单失败
		OrderStatus_TimeOut = 4; //处理超时，结果未知
		OrderStatus_Submitted = 5; //已提交，等待成交
		OrderStatus_Filled_Part = 10; //部分成交
		OrderStatus_Filled_All = 11; //全部已成
		OrderStatus_Cancelling_Part = 12; //正在撤单_部分(部分已成交，正在撤销剩余部分)
		OrderStatus_Cancelling_All = 13; //正在撤单_全部
		OrderStatus_Cancelled_Part = 14; //部分成交，剩余部分已撤单
		OrderStatus_Cancelled_All = 15; //全部已撤单，无成交
		OrderStatus_Failed = 21; //下单失败，服务拒绝
		OrderStatus_Disabled = 22; //已失效
		OrderStatus_Deleted = 23; //已删除，无成交的订单才能删除
	};

-----------------------------------------------

TrdEnv - 交易环境
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	enum TrdEnv
	{
		TrdEnv_Simulate = 0; //仿真环境(模拟环境)
		TrdEnv_Real = 1; //真实环境
	}
	
-----------------------------------------------

TrdMarket - 交易市场
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	//交易市场，是大的市场，不是具体品种
	enum TrdMarket
	{
		TrdMarket_Unknown = 0; //未知市场
		TrdMarket_HK = 1; //香港市场
		TrdMarket_US = 2; //美国市场
		TrdMarket_CN = 3; //大陆市场
		TrdMarket_HKCC = 4; //香港A股通市场
		TrdMarket_Futures = 5; //期货市场
	}

-----------------------------------------------

TrdSide - 交易方向
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	enum TrdSide
	{
		//除了期货，其他股票都只支持传入买入和卖出
		TrdSide_Unknown = 0; //未知方向
		TrdSide_Buy = 1; //买入
		TrdSide_Sell = 2; //卖出
		TrdSide_SellShort = 3; //卖空
		TrdSide_BuyBack = 4; //买回
	}


-----------------------------------------------

OrderType - 订单类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	enum OrderType
	{
		OrderType_Unknown = 0; //未知类型
		OrderType_Normal = 1; //普通订单(港股的增强限价单、A股的限价单、美股的限价单)
		OrderType_Market = 2; //市价订单(目前支持美股、港股正股、涡轮、牛熊、界内证)
		
		OrderType_AbsoluteLimit = 5; //绝对限价订单(目前仅港股)，只有价格完全匹配才成交，否则下单失败，比如你下价格为5元的买单，卖单价格必须也要是5元才能成交，低于5元也不能成交，下单失败。卖出同理
		OrderType_Auction = 6; //竞价订单(目前仅港股)，仅港股早盘竞价和收盘竞价有效，A股的早盘竞价订单类型不变还是OrderType_Normal
		OrderType_AuctionLimit = 7; //竞价限价订单(目前仅港股)，仅早盘竞价和收盘竞价有效，参与竞价，且要求满足指定价格才会成交
		OrderType_SpecialLimit = 8; //特别限价订单(目前仅港股)，成交规则同增强限价订单，且部分成交后，交易所自动撤销订单
		OrderType_SpecialLimit_All = 9; //特别限价且要求全部成交订单(目前仅港股)，要么全部成交，要么自动撤单
	}


-----------------------------------------------

OrderFillStatus - 一笔成交的状态
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum OrderFillStatus
	{
		OrderFillStatus_OK = 0; //正常
		OrderFillStatus_Cancelled = 1; //成交被取消
		OrderFillStatus_Changed = 2; //成交被更改
	}

-----------------------------------------------

PositionSide - 持仓方向类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum PositionSide
	{
		PositionSide_Unknown = -1; //未知方向
		PositionSide_Long = 0; //多仓，默认情况是多仓
		PositionSide_Short = 1; //空仓
	};


-----------------------------------------------


ModifyOrderOp - 修改订单操作类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	enum ModifyOrderOp
	{
		//港股支持全部操作，美股目前仅支持ModifyOrderOp_Normal和ModifyOrderOp_Cancel
		ModifyOrderOp_Unknown = 0; //未知操作
		ModifyOrderOp_Normal = 1; //修改订单的价格、数量等，即以前的改单
		ModifyOrderOp_Cancel = 2; //撤单
		ModifyOrderOp_Disable = 3; //失效
		ModifyOrderOp_Enable = 4; //生效
		ModifyOrderOp_Delete = 5; //删除
	};

-----------------------------------------------

TrdAccType - 交易账户类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	enum TrdAccType
	{
		TrdAccType_Unknown = 0; //未知类型
		TrdAccType_Cash = 1;    //现金账户
		TrdAccType_Margin = 2;  //保证金账户
	};

-----------------------------------------------	

Currency - 货币种类
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	enum Currency
	{
		Currency_Unknown = 0;  //未知货币
		Currency_HKD = 1;   // 港币
		Currency_USD = 2;   // 美元
		Currency_CNH = 3;   // 离岸人民币
	};

-----------------------------------------------	

CltRiskLevel - 账户风险控制等级
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	enum CltRiskLevel
	{
		CltRiskLevel_Unknown = -1;		// 未知
		CltRiskLevel_Safe = 0;          // 安全
		CltRiskLevel_Warning = 1;       // 预警
		CltRiskLevel_Danger = 2;        // 危险
		CltRiskLevel_AbsoluteSafe = 3;  // 绝对安全
		CltRiskLevel_OptDanger = 4;     // 危险, 期权相关
	}

-----------------------------------------------	


ReconfirmOrderReason - 确认订单类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	enum ReconfirmOrderReason
	{
		ReconfirmOrderReason_Unknown = 0; //未知原因
		ReconfirmOrderReason_QtyTooLarge = 1; //订单数量太大，确认继续下单并否拆分成多个小订单
		ReconfirmOrderReason_PriceAbnormal = 2; //价格异常，偏离当前价太大，确认继续下单
	};

-----------------------------------------------	


TrdHeader - 交易公共参数头
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message TrdHeader
	{
		required int32 trdEnv = 1; //交易环境, 参见TrdEnv的枚举定义
		required uint64 accID = 2; //业务账号, 业务账号与交易环境、市场权限需要匹配，否则会返回错误
		required int32 trdMarket = 3; //交易市场, 参见TrdMarket的枚举定义
	}
	
-----------------------------------------------

TrdAcc - 交易账户
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message TrdAcc
	{
		required int32 trdEnv = 1; //交易环境，参见TrdEnv的枚举定义
		required uint64 accID = 2; //业务账号
		repeated int32 trdMarketAuthList = 3; //业务账户支持的交易市场权限，即此账户能交易那些市场, 可拥有多个交易市场权限，目前仅单个，取值参见TrdMarket的枚举定义
		optional int32 accType = 4;   //账户类型，取值见TrdAccType
		optional string cardNum = 5;  //卡号
	}

-----------------------------------------------

AccCashInfo - 账户现金信息，目前仅用于期货账户
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message AccCashInfo
	{
		optional int32 currency = 1;        // 货币类型，取值参考 Currency
		optional double cash = 2;           // 现金结余
		optional double availableBalance = 3;   // 现金可提金额
	}

-----------------------------------------------

Funds - 账户资金
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	message Funds
	{
		required double power = 1; //购买力，3位精度，下同
		required double totalAssets = 2; //资产净值
		required double cash = 3; //现金
		required double marketVal = 4; //证券市值
		required double frozenCash = 5; //冻结金额
		required double debtCash = 6; //欠款金额
		required double avlWithdrawalCash = 7; //可提金额

		optional int32 currency = 8;            //币种，本结构体资金相关的货币类型，取值参见 Currency，期货适用
		optional double availableFunds = 9;     //可用资金，期货适用
		optional double unrealizedPL = 10;      //未实现盈亏，期货适用
		optional double realizedPL = 11;        //已实现盈亏，期货适用
		optional int32 riskLevel = 12;           //风控状态，参见 CltRiskLevel, 期货适用
		optional double initialMargin = 13;      //初始保证金，期货适用
		optional double maintenanceMargin = 14;  //维持保证金，期货适用
		repeated AccCashInfo cashInfoList = 15;  //分币种的现金信息，期货适用
	}

-----------------------------------------------

Position - 账户持仓 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	message Position
	{
		required uint64 positionID = 1; //持仓ID，一条持仓的唯一标识
		required int32 positionSide = 2; //持仓方向，参见PositionSide的枚举定义
		required string code = 3; //代码
		required string name = 4; //名称
		required double qty = 5; //持有数量，2位精度，期权单位是"张"，下同
		required double canSellQty = 6; //可卖数量
		required double price = 7; //市价，3位精度
		optional double costPrice = 8; //成本价，无精度限制，如果没传，代表此时此值无效
		required double val = 9; //市值，3位精度
		required double plVal = 10; //盈亏金额，3位精度
		optional double plRatio = 11; //盈亏比例，无精度限制，如果没传，代表此时此值无效（该字段为百分比字段，默认不展示%，如20实际对应20%，如20实际对应20%）
	  
		//以下是此持仓今日统计
		optional double td_plVal = 21; //今日盈亏金额，3位精度，下同
		optional double td_trdVal = 22; //今日交易额
		optional double td_buyVal = 23; //今日买入总额
		optional double td_buyQty = 24; //今日买入总量
		optional double td_sellVal = 25; //今日卖出总额
		optional double td_sellQty = 26; //今日卖出总量

		optional double unrealizedPL = 28;       //未实现盈亏，期货适用
		optional double realizedPL = 29;         //已实现盈亏，期货适用
	}

-----------------------------------------------

Order - 订单
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	message Order
	{
		required int32 trdSide = 1; //交易方向, 参见TrdSide的枚举定义
		required int32 orderType = 2; //订单类型, 参见OrderType的枚举定义
		required int32 orderStatus = 3; //订单状态, 参见OrderStatus的枚举定义
		required uint64 orderID = 4; //订单号
		required string orderIDEx = 5; //扩展订单号(仅查问题时备用)
		required string code = 6; //代码
		required string name = 7; //名称
		required double qty = 8; //订单数量，2位精度，期权单位是"张"
		optional double price = 9; //订单价格，3位精度
		required string createTime = 10; //创建时间，严格按YYYY-MM-DD HH:MM:SS或YYYY-MM-DD HH:MM:SS.MS格式传
		required string updateTime = 11; //最后更新时间，严格按YYYY-MM-DD HH:MM:SS或YYYY-MM-DD HH:MM:SS.MS格式传
		optional double fillQty = 12; //成交数量，2位精度，期权单位是"张"
		optional double fillAvgPrice = 13; //成交均价，无精度限制
		optional string lastErrMsg = 14; //最后的错误描述，如果有错误，会有此描述最后一次错误的原因，无错误为空
		optional int32 secMarket = 15; //（2018/07/17新增）证券所属市场，参见TrdSecMarket的枚举定义
		optional double createTimestamp = 16; //创建时间戳
		optional double updateTimestamp = 17; //最后更新时间戳
		optional string remark = 18; //用户备注字符串，最大长度64字节
	}

-----------------------------------------------

OrderFill - 成交
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf

	message OrderFill
	{
		required int32 trdSide = 1; //交易方向, 参见TrdSide的枚举定义
		required uint64 fillID = 2; //成交号
		required string fillIDEx = 3; //扩展成交号(仅查问题时备用)
		optional uint64 orderID = 4; //订单号
		optional string orderIDEx = 5; //扩展订单号(仅查问题时备用)
		required string code = 6; //代码
		required string name = 7; //名称
		required double qty = 8; //成交数量，2位精度，期权单位是"张"
		required double price = 9; //成交价格，3位精度
		required string createTime = 10; //创建时间（成交时间），严格按YYYY-MM-DD HH:MM:SS或YYYY-MM-DD HH:MM:SS.MS格式传
		optional int32 counterBrokerID = 11; //对手经纪号，港股有效
		optional string counterBrokerName = 12; //对手经纪名称，港股有效
		optional int32 secMarket = 13; //（2018/07/17新增）证券所属市场，参见TrdSecMarket的枚举定义
		optional double createTimestamp = 14; //时间戳
		optional double updateTimestamp = 15; //最后更新时间戳
		optional int32 status = 16; //成交状态, 参见OrderFillStatus的枚举定义
	}

-----------------------------------------------

MaxTrdQtys - 最大交易数量
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	message MaxTrdQtys
	{
		//因目前服务器实现的问题，卖空需要先卖掉持仓才能再卖空，是分开两步卖的，买回来同样是逆向两步；而看多的买是可以现金加融资一起一步买的，请注意这个差异
		required double maxCashBuy = 1; //不使用融资，仅自己的现金最大可买整手股数，期货此字段值为0，期货不适用
		optional double maxCashAndMarginBuy = 2; //使用融资，自己的现金 + 融资资金总共的最大可买整手股数
		required double maxPositionSell = 3; //不使用融券(卖空)，仅自己的持仓最大可卖整手股数，期货不适用
		optional double maxSellShort = 4; //使用融券(卖空)，最大可卖空整手股数，不包括多仓
		optional double maxBuyBack = 5; //卖空后，需要买回的最大整手股数。因为卖空后，必须先买回已卖空的股数，还掉股票，才能再继续买多。期货不适用
	}
	 
-----------------------------------------------

TrdFilterConditions - 过滤条件
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	//过滤条件，条件组合是"与"不是"或"，用于获取订单、成交、持仓等时二次过滤
	message TrdFilterConditions
	{
		repeated string codeList = 1; //代码过滤，只返回包含这些代码的数据，没传不过滤
		repeated uint64 idList = 2; //ID主键过滤，只返回包含这些ID的数据，没传不过滤，订单是orderID、成交是fillID、持仓是positionID
		optional string beginTime = 3; //开始时间，严格按YYYY-MM-DD HH:MM:SS或YYYY-MM-DD HH:MM:SS.MS格式传，对持仓无效，拉历史数据必须填
		optional string endTime = 4; //结束时间，严格按YYYY-MM-DD HH:MM:SS或YYYY-MM-DD HH:MM:SS.MS格式传，对持仓无效，拉历史数据必须填
	}
	 
-----------------------------------------------

TrdSecMarket - 可交易证券所属市场
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 .. code-block:: protobuf
 
	//可交易证券所属市场，目前主要是区分A股的沪市和深市，香港和美国暂不需要细分
	enum TrdSecMarket
	{
		TrdSecMarket_Unknown = 0; //未知市场
		TrdSecMarket_HK = 1; //香港的(正股、窝轮等)
		TrdSecMarket_US = 2; //美国的(正股、期权等)
		TrdSecMarket_CN_SH = 31; //沪市的(正股)
		TrdSecMarket_CN_SZ = 32; //深市的(正股)
	}
	 
-----------------------------------------------

    