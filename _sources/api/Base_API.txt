基础API
========
 .. _ProtoFMT : #id2
 
------------------------------------

SysConfig - 系统配置
---------------------

..  py:class:: SysConfig

对python api系统参数进行配置

------------------------------------

set_client_info
~~~~~~~~~~~~~~~~~

..  py:function:: set_client_info(cls, client_id, client_ver)

  设置调用api的client信息, 非必调接口

  :param client_id: str, client的标识
  :param client_ver: int, client的版本号
  :return: None

  :example:

  .. code:: python

   from futu import *
   SysConfig.set_client_info("MyFutuAPI", 0)
   quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
   quote_ctx.close()
	
--------------------------------------------

set_proto_fmt
~~~~~~~~~~~~~~~~~

..  py:function:: set_proto_fmt(cls, proto_fmt)

  设置通讯协议body格式, 目前支持Protobuf | Json两种格式, 非必调接口

  :param proto_fmt: ProtoFMT_
  :return: None

  :example:

  .. code:: python

   from futu import *
   SysConfig.set_proto_fmt(ProtoFMT.Protobuf)
   quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
   quote_ctx.close()
         
--------------------------------------------
                 
enable_proto_encrypt
~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: enable_proto_encrypt(cls, is_encrypt)

  设置通讯协议是否加密，网关客户端和api需配置相同的RSA私钥文件，在连接初始化成功后，网关会下发随机生成的AES 加密密钥。

  每种context（例如OpenQuoteContext）可以单独设置自己是否加密。如果不设置，则使用这里的全局设置。即Context的加密设置优先级更高。

  如果FutuOpenD配置了RSA私钥文件（rsa_private_key），那么客户端不管是否在这里启用全局加密，还是每个context自己设置加密，都需要调用SysConfig.set_init_rsa_file来设置RSA私钥文件。

  :param is_encrypt: bool
  :return: None

  :example:

  .. code:: python

   from futu import *
   SysConfig.enable_proto_encrypt(True)
   SysConfig.set_init_rsa_file("conn_key.txt")   # rsa 私钥文件路径
   quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
   quote_ctx.close()

--------------------------------------------

set_init_rsa_file
~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: set_init_rsa_file(cls, file)

  设置RSA私钥文件, 要求1024位, 格式为PKCS#1

  :param file:  str, 文件路径
  :return: None

  :example:

  .. code:: python

   from futu import *
   SysConfig.enable_proto_encrypt(True)
   SysConfig.set_init_rsa_file("conn_key.txt")   # rsa 私钥文件路径
   quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
   quote_ctx.close()
   
   
--------------------------------------------

set_all_thread_daemon
~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: set_all_thread_daemon(cls, all_daemon)

  设置是否所有内部创建的线程都是daemon线程。在主线程退出后，如果其余线程都是daemon线程，则进程退出。否则进程仍会继续运行。如果不设置，默认内部会创建非daemon线程。默认情况下，行情和交易的context连接上FutuOpenD后，如果不调用close，即使主线程退出，进程也不会退出。因此，如果行情和交易的context设置了接收数据推送，并且也设置了daemon线程，则要自己保证主线程存活，否则进程将退出，也就不会再收到推送数据了。

  :param all_daemon:  bool, 是否所有内部线程都是daemon线程
  :return: None

  :example:

  .. code:: python

   from futu import *
   SysConfig.set_all_thread_daemon(True)
   quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
   # 不调用quote_ctx.close()，进程也会退出

--------------------------------------------


枚举常量
---------

AuType - K线复权类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~

K线复权定义

..  py:class:: AuType

 ..  py:attribute:: QFQ
 
  前复权
  
 ..  py:attribute:: HFQ
 
  后复权
  
 ..  py:attribute:: NONE
 
  不复权
  
--------------------------------------

DarkStatus - 暗盘状态
~~~~~~~~~~~~~~~~~~~~~~~~~~~

暗盘状态定义

..  py:class:: DarkStatus

 ..  py:attribute:: NONE
 
  无暗盘交易
  
 ..  py:attribute:: TRADING
 
  暗盘交易中
  
 ..  py:attribute:: END
 
  暗盘交易结束
  
--------------------------------------

DealStatus - 成交状态
~~~~~~~~~~~~~~~~~~~~~~~~~~~

成交状态

..  py:class:: DealStatus

 ..  py:attribute:: OK

   正常

 ..  py:attribute:: CANCELLED

   被取消

 ..  py:attribute:: CHANGED

  被更改

--------------------------------------

FinancialQuarter - 财务指标周期
~~~~~~~~~~~~~~~~~~~~~~~~~~~
财务指标周期定义

..  py:class:: FinancialQuarter

 ..  py:attribute:: NONE
 
  无
  
 ..  py:attribute:: ANNUAL
 
  年报
  
 ..  py:attribute:: FIRST_QUARTER
 
  Q1一季报，不支持美股市场
  
 ..  py:attribute:: INTERIM
 
  Q6中期报，不支持美股市场
  
 ..  py:attribute:: THIRD_QUARTER
 
  Q9三季报，不支持美股市场
    
 ..  py:attribute:: MOST_RECENT_QUARTER
 
  最近季报，仅支持美股市场
  
--------------------------------------


GtwEventType - 网关异步通知类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

网关异步通知类型定义

..  py:class:: GtwEventType

 ..  py:attribute:: LocalCfgLoadFailed
 
  本地配置文件加载失败
  
 ..  py:attribute:: APISvrRunFailed
 
  网关监听服务运行失败
  
 ..  py:attribute:: ForceUpdate
 
  强制升级网关
  
 ..  py:attribute:: LoginFailed
 
  登录牛牛服务器失败
  
 ..  py:attribute:: UnAgreeDisclaimer
 
  未同意免责声明，无法加运行
  
 ..  py:attribute:: NetCfgMissing
 
  缺少网络连接配置
  
 ..  py:attribute:: KickedOut
 
  登录被踢下线
  
 ..  py:attribute:: LoginPwdChanged
 
  登陆密码变更
  
 ..  py:attribute:: BanLogin
 
  牛牛后台不允许该账号登陆
  
 ..  py:attribute:: NeedPicVerifyCode
 
  登录需要输入图形验证码
  
 ..  py:attribute:: NeedPhoneVerifyCode
 
  登录需要输入手机验证码
  
 ..  py:attribute:: AppDataNotExist
 
  程序打包数据丢失
  
 ..  py:attribute:: NessaryDataMissing
 
  必要的数据没同步成功
  
 ..  py:attribute:: TradePwdChanged
 
  交易密码变更通知
  
 ..  py:attribute:: EnableDeviceLock
 
  需启用设备锁
  
--------------------------------------  

IpoPeriod - 窝轮上市日
~~~~~~~~~~~~~~~~~~~~~~~~~~~

窝轮上市日定义

..  py:class:: IpoPeriod

 ..  py:attribute:: NONE

  未知

 ..  py:attribute:: TODAY

  今日上市

 ..  py:attribute:: TOMORROW

  明日上市

 ..  py:attribute:: NEXTWEEK

  未来一周上市

 ..  py:attribute:: LASTWEEK

  过去一周上市

 ..  py:attribute:: LASTMONTH

  过去一月上市

--------------------------------------

Issuer - 发行人过滤列表
~~~~~~~~~~~~~~~~~~~~~~~~~~~

发行人过滤列表

..  py:class:: Issuer

 ..  py:attribute:: NONE

  未知

 ..  py:attribute:: SG

  法兴

 ..  py:attribute:: BP

  法巴

 ..  py:attribute:: CS

  瑞信

 ..  py:attribute:: CT

  花旗

 ..  py:attribute:: EA 

  东亚

 ..  py:attribute:: GS 

  高盛

 ..  py:attribute:: HS 

  汇丰


 ..  py:attribute:: JP 

  摩通


 ..  py:attribute:: MB 

  麦银

 ..  py:attribute:: SC 

  渣打

 ..  py:attribute:: UB 

  瑞银

 ..  py:attribute:: BI 

  中银

 ..  py:attribute:: DB 

  德银

 ..  py:attribute:: DC 

  大和

 ..  py:attribute:: ML 

  美林

 ..  py:attribute:: NM 

  野村

 ..  py:attribute:: RB 

  荷合

 ..  py:attribute:: RS 

  苏皇

 ..  py:attribute:: BC 

  巴克莱

 ..  py:attribute:: HT 

  海通

 ..  py:attribute:: VT 

  瑞通

 ..  py:attribute:: KC 

  比联

 ..  py:attribute:: MS

  摩利
--------------------------------------

KLDataStatus - k线数据状态
~~~~~~~~~~~~~~~~~~~~~~~~~~~

指定时间点取历史k线， 获得数据的实际状态

..  py:class:: KLDataStatus

 ..  py:attribute:: NONE
 
  无效数据
  
 ..  py:attribute:: CURRENT
 
  当前时间周期数据
  
 ..  py:attribute:: PREVIOUS
 
  前一时间周期数据
  
 ..  py:attribute:: BACK
 
  后一时间周期数据
  
  
--------------------------------------

KL_FIELD - K线数据字段
~~~~~~~~~~~~~~~~~~~~~~~~~~~

获取K线数据, 可指定需返回的字段

..  py:class:: KL_FIELD

 ..  py:attribute:: ALL
 
  所有字段
  
 ..  py:attribute:: DATE_TIME
 
  日期时间
  
 ..  py:attribute:: OPEN
 
  开盘价
  
 ..  py:attribute:: CLOSE
 
  收盘价
  
 ..  py:attribute:: HIGH
 
  最高价
  
 ..  py:attribute:: LOW
 
  最低价
  
 ..  py:attribute:: PE_RATIO
 
  市盈率
  
 ..  py:attribute:: TURNOVER_RATE
 
  换手率
  
 ..  py:attribute:: TRADE_VOL
 
  成交量
  
 ..  py:attribute:: TRADE_VAL
 
  成交额
  
 ..  py:attribute:: CHANGE_RATE
 
  涨跌比率
  
 ..  py:attribute:: LAST_CLOSE
 
  昨收价
  
--------------------------------------

KLNoDataMode - K线数据取值模式
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

指定时间为非交易日时，对应的k线数据取值模式

..  py:class:: KLNoDataMode

 ..  py:attribute:: NONE
 
  返回无数据
  
 ..  py:attribute:: FORWARD
 
  往前取数据
  
 ..  py:attribute:: BACKWARD
 
  往后取数据


--------------------------------------

KLType - k线类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~

k线类型定义

..  py:class:: KLType

 ..  py:attribute:: K_1M
 
  1分钟K线

 ..  py:attribute:: K_3M

  3分钟K线
  
 ..  py:attribute:: K_5M
 
  5分钟K线
  
 ..  py:attribute:: K_15M
 
  15分钟K线
  
 ..  py:attribute:: K_30M
 
  30分钟K线
  
 ..  py:attribute:: K_60M
 
  60分钟K线
  
 ..  py:attribute:: K_DAY
 
  日K线
  
 ..  py:attribute:: K_WEEK
 
  周K线
  
 ..  py:attribute:: K_MON
 
  月K线
  
 ..  py:attribute:: K_QUARTER

  季K线

 ..  py:attribute:: K_YEAR

  年K线

--------------------------------------

ModifyUserSecurityOp - 自选股操作类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

自选股操作类型定义

..  py:class:: ModifyUserSecurityOp

 ..  py:attribute:: NONE

  未知

 ..  py:attribute:: ADD

  新增

 ..  py:attribute:: DEL

  删除自选
  
 ..  py:attribute:: MOVE_OUT

  移出分组

--------------------------------------

Market - 行情市场
~~~~~~~~~~~~~~~~~

标识不同的行情市场，股票名称的前缀复用该字符串,如 **'HK.00700'**, **'US.AAPL'**

..  py:class:: Market

 ..  py:attribute:: HK    
    
  港股
  
 ..  py:attribute:: SH  
    
  沪市
  
 ..  py:attribute:: SZ
    
  深市
  
 ..  py:attribute:: HK_FUTURE  
    
  港股期货
  
 ..  py:attribute:: NONE
    
  未知

--------------------------------------

MarketState - 行情市场状态
~~~~~~~~~~~~~~~~~~~~~~~~~~~

行情市场状态定义

..  py:class:: MarketState

 ..  py:attribute:: NONE
 
  无交易,美股未开盘
  
 ..  py:attribute:: AUCTION
 
  竞价
  
 ..  py:attribute:: WAITING_OPEN
 
  早盘前等待开盘
  
 ..  py:attribute:: MORNING
 
  早盘
  
 ..  py:attribute:: REST
 
  午间休市
  
 ..  py:attribute:: AFTERNOON
 
  午盘
  
 ..  py:attribute:: CLOSED
 
  收盘
  
 ..  py:attribute:: PRE_MARKET_BEGIN
 
  盘前开始
  
 ..  py:attribute:: PRE_MARKET_END
 
  盘前结束
  
 ..  py:attribute:: AFTER_HOURS_BEGIN
 
  盘后开始

 ..  py:attribute:: AFTER_HOURS_END
 
  盘后结束
  
 ..  py:attribute:: NIGHT_OPEN
 
  夜市开盘
  
 ..  py:attribute:: NIGHT_END
 
  夜市收盘
  
 ..  py:attribute:: FUTURE_DAY_OPEN
 
  期指日市开盘
  
 ..  py:attribute:: FUTURE_DAY_BREAK
 
  期指日市休市
  
 ..  py:attribute:: FUTURE_DAY_CLOSE
 
  期指日市收盘
  
 ..  py:attribute:: FUTURE_DAY_WAIT_OPEN
 
  期指日市等待开盘
  
 ..  py:attribute:: HK_CAS
 
  港股盘后竞价
  
--------------------------------------

ModifyOrderOp - 修改订单操作类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

修改订单操作类型定义

..  py:class:: ModifyOrderOp

 ..  py:attribute:: NONE
 
  未知
  
 ..  py:attribute:: NORMAL
  
  修改订单的数量、价格
  
 ..  py:attribute:: CANCEL
 
  取消订单
  
 ..  py:attribute:: DISABLE
 
  使订单失效
  
 ..  py:attribute:: ENABLE
 
  使订单生效
  
 ..  py:attribute:: DELETE
 
  删除订单
  
--------------------------------------

OptionCondType - 价内价外
~~~~~~~~~~~~~~~~~~~~~~~~~~~

价内价外定义

..  py:class:: OptionType

 ..  py:attribute:: ALL
 
  全部
  
 ..  py:attribute:: WITHIN
 
  价内
  
 ..  py:attribute:: OUTSIDE
 
  价外

--------------------------------------

OptionType - 期权类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~

期权类型定义

..  py:class:: OptionType

 ..  py:attribute:: ALL
 
  全部
  
 ..  py:attribute:: CALL
 
  涨
  
 ..  py:attribute:: PUT
 
  跌
  
--------------------------------------

IndexOptionType - 指数期权类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~

指数期权类型定义

..  py:class:: IndexOptionType

 ..  py:attribute:: NONE
 
  未知
  
 ..  py:attribute:: NORMAL 
 
  正常
  
 ..  py:attribute:: SMALL 
 
  小型
  
--------------------------------------

OptionAreaType - 期权地区类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~

期权地区类型定义

..  py:class:: OptionAreaType

 ..  py:attribute:: NONE
 
  未知
  
 ..  py:attribute:: AMERICAN
 
  美式
  
 ..  py:attribute:: EUROPEAN
 
  欧式
  
 ..  py:attribute:: BERMUDA
 
  百慕大
  
--------------------------------------

OrderType - 订单类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~

订单类型定义

..  py:class:: OrderType

 ..  py:attribute:: NONE
 
  未知
  
 ..  py:attribute:: NORMAL
  
  普通订单(港股的增强限价单、港股期权的限价单，A股限价委托、美股的限价单，港股期货的限价单)。目前港股期权只能指定此订单类型。
  
 ..  py:attribute:: MARKET
 
  市价，目前支持美股、港股（正股、涡轮、牛熊证，界内证）
  
 ..  py:attribute:: ABSOLUTE_LIMIT
 
  港股限价单(只有价格完全匹配才成交)
  
 ..  py:attribute:: AUCTION
 
  港股竞价单，港股期货的竞价市价单
  
 ..  py:attribute:: AUCTION_LIMIT
 
  港股竞价限价单。不支持期货。
  
 ..  py:attribute:: SPECIAL_LIMIT
 
  港股特别限价(即市价IOC, 订单到达交易所后，或全部成交， 或部分成交再撤单， 或下单失败)。不支持期货。
  
 .. py:attribute:: SPECIAL_LIMIT_ALL

  港股特别限价(要么全部成交，否则下单失败)。不支持期货。
--------------------------------------

OrderStatus - 订单状态定义
~~~~~~~~~~~~~~~~~~~~~~~~~~~

订单状态定义

..  py:class:: OrderStatus

 ..  py:attribute:: NONE
 
  未知
  
 ..  py:attribute:: UNSUBMITTED
  
  未提交
  
 ..  py:attribute:: WAITING_SUBMIT
 
  等待提交
  
 ..  py:attribute:: SUBMITTING
 
  提交中
  
 ..  py:attribute:: SUBMIT_FAILED
 
  提交失败，下单失败
  
 ..  py:attribute:: SUBMITTED
 
  已提交，等待成交
  
 ..  py:attribute:: FILLED_PART
 
  部分成交
  
 ..  py:attribute:: FILLED_ALL
 
  全部已成
  
 ..  py:attribute:: CANCELLING_PART
 
  正在撤单部分(部分已成交，正在撤销剩余部分)
  
 ..  py:attribute:: CANCELLING_ALL
 
  正在撤单全部
  
 ..  py:attribute:: CANCELLED_PART
 
  部分成交，剩余部分已撤单
  
 ..  py:attribute:: CANCELLED_ALL
 
  全部已撤单，无成交
  
 ..  py:attribute:: FAILED
 
  下单失败，服务拒绝
  
 ..  py:attribute:: DISABLED
 
  已失效
  
 ..  py:attribute:: DELETED
 
  已删除(无成交的订单才能删除)
  
--------------------------------------

PriceReminderFreq - 到价提醒频率
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

到价提醒频率

..  py:class:: PriceReminderFreq

 ..  py:attribute:: NONE

  未知类型
  
 ..  py:attribute:: ALWAYS

  持续提醒
  
 ..  py:attribute:: ONCE_A_DAY 

  每日一次
  
 ..  py:attribute:: ONCE

  仅提醒一次
  
--------------------------------------

PriceReminderType - 到价提醒类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

到价提醒的类型

..  py:class:: PriceReminderType

 ..  py:attribute:: NONE

  未知类型

 ..  py:attribute:: PRICE_UP

  价格涨到
  
 ..  py:attribute::  PRICE_DOWN 

  价格跌到
  
 ..  py:attribute:: CHANGE_RATE_UP

  日涨幅超
  
 ..  py:attribute:: CHANGE_RATE_DOWN

  日跌幅超
  
 ..  py:attribute:: FIVE_MIN_CHANGE_RATE_UP 

  5分钟涨幅超
  
 ..  py:attribute:: FIVE_MIN_CHANGE_RATE_DOWN

  5分钟跌幅超
  
 ..  py:attribute:: VOLUME_UP

  成交量超过
  
 ..  py:attribute::  TURNOVER_UP

  成交额超过
  
 ..  py:attribute:: TURNOVER_RATE_UP 

  换手率超过
  
 ..  py:attribute:: BID_PRICE_UP

  买一价高于
  
 ..  py:attribute:: ASK_PRICE_DOWN

  卖一价低于
  
 ..  py:attribute:: BID_VOL_UP 

  买一量高于
  
 ..  py:attribute:: ASK_VOL_UP

  卖一量高于
  
--------------------------------------

ProgramStatusType - 程序运行状态通知类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

程序运行状态通知类型定义

..  py:class:: ProgramStatusType

 ..  py:attribute:: NONE

  未知类型

 ..  py:attribute:: LOADED

  已完成类似加载配置,启动服务器等操作,服务器启动之前的状态无需返回

 ..  py:attribute:: LOGING

  登录中

 ..  py:attribute:: NEED_PIC_VERIFY_CODE

  需要图形验证码

 ..  py:attribute:: NEED_PHONE_VERIFY_CODE

  需要手机验证码

 ..  py:attribute:: LOGIN_FAILED

  登录失败,详细原因在描述返回

 ..  py:attribute:: FORCE_UPDATE

  客户端版本过低

 ..  py:attribute:: NESSARY_DATA_PREPARING

  正在拉取类似免责声明等一些必要信息

 ..  py:attribute:: NESSARY_DATA_MISSING

  缺少必要信息

 ..  py:attribute:: UN_AGREE_DISCLAIMER

  未同意免责声明

 ..  py:attribute:: READY

  可以接收业务协议收发,正常可用状态

--------------------------------------  

Plate - 板块集合分类
~~~~~~~~~~~~~~~~~~~~~~~~~~~

板块集合分类定义

..  py:class:: Plate

 ..  py:attribute:: ALL
 
  所有板块
  
 ..  py:attribute:: INDUSTRY
 
  行业板块
  
 ..  py:attribute:: REGION
 
  地域板块
  
 ..  py:attribute:: CONCEPT
 
  概念板块

--------------------------------------

PositionSide - 持仓方向类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~

持仓方向类型定义

..  py:class:: PositionSide

 ..  py:attribute:: NONE
 
  未知
  
 ..  py:attribute:: LONG
 
  多仓
  
 ..  py:attribute:: SHORT
 
  空仓
  
--------------------------------------

PriceType - 窝轮价(界)内外
~~~~~~~~~~~~~~~~~~~~~~~~~~~

上市日

..  py:class:: PriceType

 ..  py:attribute:: Unknown

  未知

 ..  py:attribute:: Outside

  价外,界内证表示界外

 ..  py:attribute:: WithIn

  价内,界内证表示界内

--------------------------------------

ProtoFMT - 协议格式
~~~~~~~~~~~~~~~~~~~~~~

    协议格式类型
    
    ..  py:class:: ProtoFMT
    
     ..  py:attribute:: Protobuf
     
      google的protobuf格式
      
     ..  py:attribute:: Json
     
      json格式
      
------------------------------------

PushDataType - 推送数据类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~

推送数据类型定义

..  py:class:: PushDataType

 ..  py:attribute:: REALTIME
 
  实时推送数据
  
 ..  py:attribute:: BYDISCONN
 
  行情连接断开重连后，OpenD拉取补充断开期间的数据，最多50根
  
 ..  py:attribute:: CACHE
 
  非实时推送数据，非连接断开补充数据
  
--------------------------------------

ret_code - 接口返回值
~~~~~~~~~~~~~~~~~~~~~~

接口返回值定义

 ..  py:attribute:: RET_OK = 0
 
 ..  py:attribute:: RET_ERROR = -1

------------------------------------

SecurityType - 证券类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~
  
证券类型定义

..  py:class:: SecurityType

 ..  py:attribute:: STOCK
 
  股票
  
 ..  py:attribute:: IDX
 
  指数
  
 ..  py:attribute:: ETF
 
  交易所交易基金(Exchange Traded Funds)
  
 ..  py:attribute:: WARRANT
 
  港股窝轮牛熊界内证
  
 ..  py:attribute:: BOND
 
  债券

 ..  py:attribute:: DRVT
 
  期权
   
 ..  py:attribute:: NONE
 
  未知
  
--------------------------------------

SecurityStatus - 股票状态
~~~~~~~~~~~~~~~~~~~~~~~~~~~

股票状态定义

..  py:class:: SecurityStatus

 ..  py:attribute:: NONE
 
  未知
  
 ..  py:attribute:: NORMAL
 
  正常状态
  
 ..  py:attribute:: PURCHASING 
 
   申购中

 ..  py:attribute:: SUBSCRIBING 
 
   认购中
  
 ..  py:attribute:: BEFORE_DARK_TRADE_OPEING 
 
   暗盘开盘前
   
 ..  py:attribute:: DARK_TRADING 
 
   暗盘交易中
   
 ..  py:attribute:: DARK_TRAD_END 
 
   暗盘已收盘
   
 ..  py:attribute:: TO_BE_OPEN 
 
   待开盘
   
 ..  py:attribute:: SUSPENDED 
 
   停牌
   
 ..  py:attribute:: CALLED  
 
   已收回
   
 ..  py:attribute:: EXPIRED_LAST_TRADING_DATE 
 
   已过最后交易日
   
 ..  py:attribute:: EXPIRED 
 
   已过期
   
 ..  py:attribute:: DELISTED 
 
   已退市
   
 ..  py:attribute:: CHANGE_TO_TEMPORARY_CODE 
 
   公司行动中，交易关闭，转至临时代码交易
   
 ..  py:attribute:: TEMPORARY_CODE_TRADE_END 
 
   临时代码交易结束，交易关闭
   
 ..  py:attribute:: CHANGED_PLATE_TRADE_END 
 
   已转板，旧代码交易关闭
   
 ..  py:attribute:: CHANGED_CODE_TRAD_END 
 
   已换代码，旧代码交易关闭
   
 ..  py:attribute:: RECOVERABLE_CIRCUIT_BREAKER 
 
   可恢复性熔断
   
 ..  py:attribute:: UNRECOVERABLE_CIRCUIT_BREAKER 
 
   不可恢复性熔断
   
 ..  py:attribute:: AFTER_COMBINATION 
 
   盘后撮合
   
 ..  py:attribute:: AFTER_TRANSACTION 
 
   盘后交易
   
--------------------------------------

SecurityReferenceType - 股票关联数据类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

股票关联数据类型

..  py:class:: SecurityReferenceType
 
 ..  py:attribute:: NONE
  
  未知
   
 ..  py:attribute:: WARRANT
  
  相关窝轮
   
 ..  py:attribute:: FUTURE
  
  期货主连相关合约

--------------------------------------

SetPriceReminderOp - 设置到价提醒操作
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

设置到价提醒操作

..  py:class:: SetPriceReminderOp
 
 ..  py:attribute:: NONE
  
  未知
   
 ..  py:attribute:: ADD
  
  新增
   
 ..  py:attribute:: DELETE
  
  删除
  
 ..  py:attribute:: ENABLE
  
  启用
   
 ..  py:attribute:: DISABLE
  
  禁用
   
 ..  py:attribute:: MODIFY
  
  修改
  
--------------------------------------

StockHolder - 持有者类别
~~~~~~~~~~~~~~~~~~~~~~~~~~~

持有者类别定义

..  py:class:: StockHolder

 ..  py:attribute:: INSTITUTE
 
  机构
  
 ..  py:attribute:: FUND
 
  基金
  
 ..  py:attribute:: EXECUTIVE
 
  高管
  
--------------------------------------

SubType - 实时数据定阅类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~

实时数据定阅类型定义

..  py:class:: SubType

 ..  py:attribute:: TICKER
 
  逐笔
  
 ..  py:attribute:: QUOTE
 
  报价
  
 ..  py:attribute:: ORDER_BOOK
 
  买卖摆盘
  
 ..  py:attribute:: K_1M
 
  1分钟K线

 ..  py:attribute:: K_3M

  3分钟K线

 ..  py:attribute:: K_5M

  5分钟K线
  
 ..  py:attribute:: K_15M
  
  15分钟K线
  
 ..  py:attribute:: K_30M
 
  30分钟K线
  
 ..  py:attribute:: K_60M
 
  60分钟K线
  
 ..  py:attribute:: K_DAY
 
  日K线
  
 ..  py:attribute:: K_WEEK
 
  周K线
  
 ..  py:attribute:: K_MON
 
  月K线

 ..  py:attribute:: K_QUARTER

  季K线

 ..  py:attribute:: K_YEAR

  年K线
  
 ..  py:attribute:: RT_DATA

  分时
  
 ..  py:attribute:: BROKER
 
   买卖经纪

--------------------------------------

StockField - 条件选股筛选条件字段(SimpleFilter)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

条件选股的筛选条件简单筛选定义，仅适用于SimpleFilter类型筛选，不能用于累积或财务筛选。

..  py:class:: StockField

 ..  py:attribute:: NONE

  未知

 ..  py:attribute:: STOCK_CODE

  股票代码

 ..  py:attribute:: STOCK_NAME

  股票名称

 ..  py:attribute:: CUR_PRICE

  最新价

 ..  py:attribute:: CUR_PRICE_TO_HIGHEST52_WEEKS_RATIO

  (现价 - 52周最高)/52周最高

 ..  py:attribute:: CUR_PRICE_TO_LOWEST52_WEEKS_RATIO

  (现价 - 52周最低)/52周最低

 ..  py:attribute:: HIGH_PRICE_TO_HIGHEST52_WEEKS_RATIO

  (今日最高 - 52周最高)/52周最高

 ..  py:attribute:: LOW_PRICE_TO_LOWEST52_WEEKS_RATIO

  (今日最低 - 52周最低)/52周最低

 ..  py:attribute:: VOLUME_RATIO

  量比

 ..  py:attribute:: BID_ASK_RATIO

  委比

 ..  py:attribute:: LOT_PRICE

  每手价格

 ..  py:attribute:: MARKET_VAL 

  市值

 ..  py:attribute:: PE_ANNUAL

  市盈率 (静态)

 ..  py:attribute:: PE_TTM

  市盈率TTM

 ..  py:attribute:: PB_RATE

  市净率
  
 ..  py:attribute:: CHANGE_RATE_5MIN

  五分钟价格涨跌幅

 ..  py:attribute:: CHANGE_RATE_BEGIN_YEAR

  年初至今价格涨跌幅

--------------------------------------------------------------------------------

StockField - 条件选股筛选条件字段(AccumulateFilter)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

条件选股的筛选条件累积字段定义，仅适用于AccumulateFilter类型筛选，不能用于简单或财务筛选。

..  py:class:: StockField

 ..  py:attribute:: CHANGE_RATE

  涨跌幅

 ..  py:attribute:: AMPLITUDE

  振幅

 ..  py:attribute:: VOLUME

  日均成交量

 ..  py:attribute:: TURNOVER

  日均成交额

 ..  py:attribute:: TURNOVER_RATE

  换手率

----------------------------------------------------------------------------------------------

StockField - 条件选股筛选条件字段(FinancialFilter)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

条件选股的筛选条件财务字段定义，仅适用于FinancialFilter类型筛选，不能用于简单或累积筛选。

财务筛选字段的使用说明：

1. 港股和A股：使用累计季报数据；

2. 美股：使用单季报数据；

3. 净利润增长率及营收增长率：均为同比数据（本期与去年同期作对比）。


..  py:class:: StockField

 ..  py:attribute:: NET_PROFIT

  净利润

 ..  py:attribute:: NET_PROFIX_GROWTH

  净利润增长率

 ..  py:attribute:: SUM_OF_BUSINESS

  营业收入

 ..  py:attribute:: SUM_OF_BUSINESS_GROWTH

  营业同比增长率

 ..  py:attribute:: NET_PROFIT_RATE

  净利率

 ..  py:attribute:: GROSS_PROFIT_RATE

  毛利率

 ..  py:attribute:: DEBT_ASSET_RATE

  资产负债率

 ..  py:attribute:: RETURN_ON_EQUITY_RATE

  净资产收益率

--------------------------------------------------------------------------------

SortField - 窝轮排序
~~~~~~~~~~~~~~~~~~~~~~~~~~~

窝轮排序定义

..  py:class:: SortField

 ..  py:attribute:: NONE

  未知

 ..  py:attribute:: CODE

  代码

 ..  py:attribute:: CUR_PRICE

  最新价

 ..  py:attribute:: PRICE_CHANGE_VAL

  涨跌额

 ..  py:attribute:: CHANGE_RATE

  涨跌幅%

 ..  py:attribute:: STATUS

  状态

 ..  py:attribute:: BID_PRICE

  买入价

 ..  py:attribute:: ASK_PRICE

  卖出价

 ..  py:attribute:: BID_VOL

  买量

 ..  py:attribute:: ASK_VOL

  卖量

 ..  py:attribute:: VOLUME

  成交量

 ..  py:attribute:: TURNOVER

  成交额

 ..  py:attribute:: SCORE

  综合评分

 ..  py:attribute:: PREMIUM

  溢价%

 ..  py:attribute:: EFFECTIVE_LEVERAGE

  有效杠杆

 ..  py:attribute:: DELTA

  对冲值,仅认购认沽支持该字段

 ..  py:attribute:: IMPLIED_VOLATILITY

  引伸波幅,仅认购认沽支持该字段

 ..  py:attribute:: TYPE

  类型

 ..  py:attribute:: STRIKE_PRICE

  行权价

 ..  py:attribute:: BREAK_EVEN_POINT

  打和点

 ..  py:attribute:: MATURITY_TIME

  到期日

 ..  py:attribute:: LIST_TIME

  上市日期

 ..  py:attribute:: LAST_TRADE_TIME

  最后交易日

 ..  py:attribute:: LEVERAGE

  杠杆比率

 ..  py:attribute:: IN_OUT_MONEY

  价内/价外%

 ..  py:attribute:: RECOVERY_PRICE

  收回价,仅牛熊证支持该字段

 ..  py:attribute:: CHANGE_PRICE

  换股价

 ..  py:attribute:: CHANGE

  换股比率

 ..  py:attribute:: STREET_RATE

  街货比%

 ..  py:attribute:: STREET_VOL

  街货量

 ..  py:attribute:: AMPLITUDE

  振幅%

 ..  py:attribute:: WARRANT_NAME

  名称

 ..  py:attribute:: ISSUER

  发行人

 ..  py:attribute:: LOT_SIZE

  每手

 ..  py:attribute:: ISSUE_SIZE

  发行量

 ..  py:attribute:: UPPER_STRIKE_PRICE

  上限价，仅界内证支持该字段

 ..  py:attribute:: LOWER_STRIKE_PRICE

  下限价，仅界内证支持该字段
  
 ..  py:attribute:: INLINE_PRICE_STATUS

  界内界外，仅界内证支持该字段

 ..  py:attribute:: PRE_CUR_PRICE

  盘前最新价

 ..  py:attribute:: AFTER_CUR_PRICE

  盘后最新价

 ..  py:attribute:: PRE_PRICE_CHANGE_VAL

  盘前涨跌额

 ..  py:attribute:: AFTER_PRICE_CHANGE_VAL

  盘后涨跌额

 ..  py:attribute:: PRE_CHANGE_RATE

  盘前涨跌幅%

 ..  py:attribute:: AFTER_CHANGE_RATE

  盘后涨跌幅%

 ..  py:attribute:: PRE_AMPLITUDE

  盘前振幅%
  
 ..  py:attribute:: AFTER_AMPLITUDE

  盘后振幅%

 ..  py:attribute:: PRE_TURNOVER

  盘前成交额
  
 ..  py:attribute:: AFTER_TURNOVER

  盘后成交额
  
 ..  py:attribute:: LAST_SETTLE_PRICE

  期货昨结
  
 ..  py:attribute:: POSITION

  期货持仓量
  
 ..  py:attribute:: POSITION_CHANGE

  期货日持仓

--------------------------------------

SysNotifyType - 系统异步通知类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

系统异步通知类型定义

..  py:class:: SysNotifyType

 ..  py:attribute:: NONE

  未知

 ..  py:attribute:: GTW_EVENT

  网关事件

 ..  py:attribute:: PROGRAM_STATUS
  
  程序状态变化

 ..  py:attribute:: CONN_STATUS

  与Server的连接状态变化

 ..  py:attribute:: QOT_RIGHT

  行情权限变化

 ..  py:attribute:: API_LEVEL

  API等级变化

--------------------------------------

SortDir - 条件选股的排序方向
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

条件选股的排序方向定义

..  py:class:: SortDir

 ..  py:attribute:: NONE

  不排序

 ..  py:attribute:: ASCEND

  升序

 ..  py:attribute:: DESCEND

  降序

--------------------------------------------------------------------------------

TrdAccType - 交易账户类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 

交易账户类型定义

..  py:class:: TrdAccType

 ..  py:attribute:: NONE

  未知

 ..  py:attribute:: CASH

  现金账户

 ..  py:attribute:: MARGIN

  保证金账户

--------------------------------------------------------------------------------

TickerDirect - 逐笔方向
~~~~~~~~~~~~~~~~~~~~~~~~~~~

逐笔方向定义

..  py:class:: TickerDirect

 ..  py:attribute:: BUY
 
  买
  
 ..  py:attribute:: SELL
 
  卖
  
 ..  py:attribute:: NEUTRAL
 
  中性
  
  
--------------------------------------

TickerType - 逐笔类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~

逐笔类型定义

..  py:class:: TickerType

 ..  py:attribute:: AUTO_MATCH
 
  自动对盘

 ..   py:attribute:: LATE
 
  开市前成交盘

 ..  py:attribute:: NON_AUTO_MATCH
 
  非自动对盘

 ..   py:attribute:: INTER_AUTO_MATCH
 
  同一证券商自动对盘

 ..  py:attribute:: INTER_NON_AUTO_MATCH
 
  同一证券商非自动对盘

 ..  py:attribute:: ODD_LOT
 
  碎股交易

 ..  py:attribute:: AUCTION
 
  竞价交易

 ..  py:attribute:: BULK
 
  批量交易

 ..  py:attribute:: CRASH
 
  现金交易

 ..  py:attribute:: CROSS_MARKET
 
  跨市场交易

 ..  py:attribute:: BULK_SOLD
 
  批量卖出

 ..  py:attribute:: FREE_ON_BOARD
 
  离价交易

 ..  py:attribute:: RULE127_OR_155
 
  第127条交易（纽交所规则）或第155条交易

 ..  py:attribute:: DELAY
 
  延迟交易

 ..  py:attribute:: MARKET_CENTER_CLOSE_PRICE
 
  中央收市价

 ..  py:attribute:: NEXT_DAY
 
  隔日交易

 ..  py:attribute:: MARKET_CENTER_OPENING
 
  中央开盘价交易

 ..  py:attribute:: PRIOR_REFERENCE_PRICE
 
  前参考价

 ..  py:attribute:: MARKET_CENTER_OPEN_PRICE
 
  中央开盘价

 ..  py:attribute:: SELLER
 
  卖方

 ..  py:attribute:: T
 
  T类交易(盘前和盘后交易)

 ..  py:attribute:: EXTENDED_TRADING_HOURS
 
  延长交易时段

 ..  py:attribute:: CONTINGENT
 
  合单交易

 ..  py:attribute:: AVERAGE_PRICE
 
  平均价成交

 ..  py:attribute:: OTC_SOLD
 
  场外售出

 ..  py:attribute:: ODD_LOT_CROSS_MARKET
 
  碎股跨市场交易

 ..  py:attribute:: DERIVATIVELY_PRICED
 
  衍生工具定价

 ..  py:attribute:: REOPENINGP_RICED
 
  再开盘定价

 ..  py:attribute:: CLOSING_PRICED
 
  收盘定价

 ..  py:attribute:: COMPREHENSIVE_DELAY_PRICE
 
  综合延迟价格
  
--------------------------------------

TradeDateType - 交易时间类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

交易时间类型定义

..  py:class:: TradeDateType

 ..  py:attribute:: WHOLE

  全天交易

 ..  py:attribute:: MORNING

  上午交易，下午休市

 ..  py:attribute:: AFTERNOON

  下午交易，上午休市
  
--------------------------------------

TradeDateMarket - 交易日市场类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

交易日市场类型定义

..  py:class:: TradeDateMarket

 ..  py:attribute:: NONE

  未知

 ..  py:attribute:: HK

  港股市场

 ..  py:attribute:: US

  美股市场
  
 ..  py:attribute:: CN

  A股市场

 ..  py:attribute:: NT

  深（沪）股通
  
 ..  py:attribute:: ST

  港股通（深、沪）
  
--------------------------------------

TrdEnv - 交易环境类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~

交易环境类型定义

..  py:class:: TrdEnv

 ..  py:attribute:: REAL
 
  真实环境
  
 ..  py:attribute:: SIMULATE
 
  模拟环境

--------------------------------------

TrdMarket - 交易市场类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~

交易市场类型定义

..  py:class:: TrdMarket

 ..  py:attribute:: NONE
 
  未知
  
 ..  py:attribute:: HK
 
  港股交易
  
 ..  py:attribute:: US

  美股交易
  
 ..  py:attribute:: CN

  A股交易
  
 ..  py:attribute:: HKCC

  香港的A股通交易  

 ..  py:attribute:: FUTURES

  期货市场
 
--------------------------------------

TrdSide - 交易方向类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

交易方向类型定义(除了期货，其他股票都只支持传入买入和卖出)

..  py:class:: TrdSide

 ..  py:attribute:: NONE
 
  未知
  
 ..  py:attribute:: BUY
  
  买
  
 ..  py:attribute:: SELL
 
  卖
  
 ..  py:attribute:: SELL_SHORT
 
  卖空
  
 ..  py:attribute:: BUY_BACK
 
  买回
  
--------------------------------------

WarrantStatus - 窝轮状态
~~~~~~~~~~~~~~~~~~~~~~~~~~~

窝轮状态定义

..  py:class:: WarrantStatus

 ..  py:attribute:: NONE

  未知

 ..  py:attribute:: NORMAL

  正常状态

 ..  py:attribute:: SUSPEND

  停牌

 ..  py:attribute:: STOP_TRADE

  终止交易

 ..  py:attribute:: PENDING_LISTING

  等待上市

--------------------------------------


WrtType - 港股窝轮类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~

港股窝轮类型定义

..  py:class:: WrtType

 ..  py:attribute:: CALL

  认购

 ..  py:attribute:: PUT

  认沽

 ..  py:attribute:: BULL

  牛证

 ..  py:attribute:: BEAR

  熊证
  
 ..  py:attribute:: INLINE

  界内证

 ..  py:attribute:: NONE

  未知

--------------------------------------


Currency - 交易相关的货币类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

交易相关的货币类型

..  py:class:: Currency

 ..  py:attribute:: NONE

  未知

 ..  py:attribute:: HKD

  港币

 ..  py:attribute:: USD

  美元

 ..  py:attribute:: CNH

  离岸人民币

--------------------------------------

CltRiskLevel - 账户风控状态
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

账户风控状态

..  py:class:: CltRiskLevel

 ..  py:attribute:: NONE

  未知

 ..  py:attribute:: SAFE

  安全

 ..  py:attribute:: WARNING

  预警

 ..  py:attribute:: DANGER

  危险

 ..  py:attribute:: ABSOLUTE_SAFE

  绝对安全

 ..  py:attribute:: OPT_DANGER

  危险，期权相关
  
--------------------------------------