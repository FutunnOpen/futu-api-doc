.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen


========
行情API
========

 .. _Market: Base_API.html#market
 
 .. _MarketState: Base_API.html#marketstate
 
 .. _SecurityType: Base_API.html#securitytype

 .. _WrtType: Base_API.html#wrttype
 
 .. _SubType: Base_API.html#subtype
 
 .. _KLType: Base_API.html#kltype-k
 
 .. _KLDataStatus: Base_API.html#kldatastatus-k
 
 .. _AuType: Base_API.html#autype-k
 
 .. _KLNoDataMode: Base_API.html#klnodatamode-k
 
 .. _KL_FIELD : Base_API.html#kl-field-k
 
 .. _TickerDirect: Base_API.html#tickerdirect
 
 .. _Plate: Base_API.html#plate
  
 .. _StockHolder: Base_API.html#stockholder

 .. _OptionType: Base_API.html#optiontype

 .. _OptionCondType: Base_API.html#optioncondtype
 
 .. _SysNotifyType: Base_API.html#sysnotifytype
 
 .. _GtwEventType: Base_API.html#gtweventtype

 .. _ProgramStatusType: Base_API.html#programstatustype

 .. _TradeDateType: Base_API.html#tradedatetype
 
 .. _SecurityReferenceType: Base_API.html#securityreferencetype
 
 .. _PushDataType: Base_API.html#pushdatatype
 
 .. _TickerType: Base_API.html#tickertype

 .. _DarkStatus: Base_API.html#darkstatus
 
 .. _SecurityStatus: Base_API.html#securitystatus
 
 .. _OptionAreaType: Base_API.html#optionareatype
 
 .. _IndexOptionType: Base_API.html#indexoptiontype
 
 .. _WarrantType: Base_API.html#warranttype

 .. _Issuer: Base_API.html#issuer

 .. _IpoPeriod: Base_API.html#ipoperiod

 .. _PriceType: Base_API.html#pricetype

 .. _WarrantStatus: Base_API.html#warrantstatus

 .. _ModifyUserSecurityOp: Base_API.html#modifyusersecurityop

 .. _SortField: Base_API.html#sortfield

 .. _SysConfig.enable_proto_encrypt: Base_API.html#enable_proto_encrypt

 .. _SortDir: Base_API.html#sortdir
 
 .. _FinancialQuarter: Base_API.html#financialquarter

 .. _TradeDateMarket: Base_API.html#tradedatemarket
 
一分钟上手
============

如下范例，创建api行情对象，调用get_market_snapshot获取港股腾讯00700的报价快照数据,最后关闭对象

.. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_market_snapshot('HK.00700'))
    quote_ctx.close()
    
----------------------------


接口类对象
==========

OpenQuoteContext - 行情上下文
-------------------------------------------

__init__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: __init__(host='127.0.0.1', port=11111, is_encrypt=None)

 构造函数

 :param host: str FutuOpenD监听的IP地址
 :param port: int FutuOpenD监听的IP端口
 :param is_encrypt: bool 是否启用加密。默认为None，表示使用 SysConfig.enable_proto_encrypt_ 的设置。

.. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111, is_encrypt=False)
    quote_ctx.close()


close
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: close

关闭上下文对象。默认情况下，futu-api内部创建的线程会阻止进程退出，只有当所有context都close后，进程才能正常退出。但通过SysConfig.set_all_thread_daemon可以设置所有内部线程为daemon线程，这时即使没有调用context的close，进程也可以正常退出。

.. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    quote_ctx.close()
    
    
start
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: start

启动异步接收推送数据


stop
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: stop

停止异步接收推送数据


set_handler
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: set_handler(self, handler)

 设置异步回调处理对象

 :param handler: 回调处理对象，必须是以下类的子类实例

            ===============================    =========================
             类名                                 说明
            ===============================    =========================
            SysNotifyHandlerBase				OpenD通知处理基类
            StockQuoteHandlerBase               报价处理基类
            OrderBookHandlerBase                摆盘处理基类
            CurKlineHandlerBase                 实时k线处理基类
            TickerHandlerBase                   逐笔处理基类
            RTDataHandlerBase                   分时数据处理基类
            BrokerHandlerBase                   经济队列处理基类
            ===============================    =========================
 :return ret: RET_OK: 设置成功

        其它: 设置失败

:strike:`get_trading_days`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_trading_days(self, market, start=None, end=None)

 获取交易日,注意该交易日是通过自然日去除周末以及节假日得到，不包括临时休市数据

 :param market: 市场类型，Market_
 :param start: 起始日期。例如'2018-01-01'。
 :param end: 结束日期。例如'2018-01-01'。
         start和end的组合如下：
            
            ==========    ==========    ========================================
            start类型      end类型       说明
            ==========    ==========    ========================================
            str            str           start和end分别为指定的日期
            None           str           start为end往前365天
            str            None          end为start往后365天
            None           None          end为当前日期，start为end往前365天
            ==========    ==========    ========================================
 :return: (ret_code, content)

        成功时返回(RET_OK, content)，content为字典列表，失败时返回(RET_ERROR, content)，其中content是错误描述字符串


        =================   ===========   ==============================================================================
        参数                  类型                        说明
        =================   ===========   ==============================================================================
        time                str            时间
        trade_date_type     str            标志是一天、上午半天、下午半天，参见 TradeDateType_
        =================   ===========   ==============================================================================

 .. code:: python

        [{'time': '2018-12-22', 'trade_date_type': 'WHOLE'},
         {'time': '2018-12-23', 'trade_date_type': 'WHOLE'},
         {'time': '2018-12-24', 'trade_date_type': 'MORNING'}]

..



        
 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_trading_days(Market.HK, start='2018-01-01', end='2018-01-10'))
    quote_ctx.close()

get_stock_basicinfo
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_stock_basicinfo(self, market, stock_type=SecurityType.STOCK, code_list=None)

 获取指定市场中特定类型的股票基本信息
 
 :param market: 市场类型 Market_
 :param stock_type: 股票类型，参见 SecurityType_，但不支持SecurityType.DRVT 
 :param code_list: 如果不为None，应该是股票code的iterable类型，将只返回指定的股票信息
 :return: (ret_code, content)

        ret_code 等于RET_OK时， content为Pandas.DataFrame数据, 否则为错误原因字符串, 数据列格式如下
        
        =================   ===========   ==============================================================================
        参数                  类型                        说明
        =================   ===========   ==============================================================================
        code                str            股票代码
        name                str            名字
        lot_size            int            每手股数，期权表示每份合约股数（指数期权无该字段），期货表示合约乘数
        stock_type          str            股票类型，参见 SecurityType_
        stock_child_type    str            窝轮子类型，参见 WrtType_
        stock_owner         str            窝轮所属正股的代码，或期权标的股的代码
        option_type         str            期权类型，查看 OptionType_
        strike_time         str            期权行权日（港股A股默认是北京时间）
        strike_price        float          期权行权价
        suspension          bool           期权是否停牌(True表示停牌)
        listing_date        str            上市时间
        stock_id            int            股票id
        delisting           bool           是否退市
        index_option_type   str            指数期权类型
        main_contract       bool           是否主连合约  
        last_trade_time     string         最后交易时间，主连，当月，下月等期货没有该字段
        =================   ===========   ==============================================================================

 :Example:

 .. code-block:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_stock_basicinfo(Market.HK, SecurityType.WARRANT))
    print(quote_ctx.get_stock_basicinfo(Market.HK, SecurityType.STOCK, 'HK.00700'))
    quote_ctx.close()

.. note::

    * 当传入程序无法识别的股票时（包括很久之前退市的股票和不存在的股票），仍然返回股票信息，用静态信息标志来该股票不存在。统一处理为：code正常显示，name显示为“未知股票”，delisting显示为“true”，其他字段均为默认值（整型默认是0，字符串默认是空字符串）。
    * 跟其他的行情接口不同，其他接口遇到程序无法识别的股票时，会拒绝请求并返回错误描述“未知股票”。

:strike:`get_multiple_history_kline`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_multiple_history_kline(self, codelist, start=None, end=None, ktype=KLType.K_DAY, autype=AuType.QFQ)

 获取多只股票的本地历史k线数据

 :param codelist: 股票代码列表，list或str。例如：['HK.00700', 'HK.00001']，'HK.00700,SZ.399001'
 :param start: 起始时间，，例如'2017-06-20'
 :param end: 结束时间，例如'2017-07-20'
 :param ktype: k线类型，参见 KLType_
 :param autype: 复权类型，参见 AuType_
 :return: 成功时返回(RET_OK, [data])，data是DataFrame数据, 数据列格式如下

    =================   ===========   ==============================================================================
    参数                  类型                        说明
    =================   ===========   ==============================================================================
    code                str            股票代码
    time_key            str            k线时间（港股A股默认是北京时间）
    open                float          开盘价
    close               float          收盘价
    high                float          最高价
    low                 float          最低价
    pe_ratio            float          市盈率
    turnover_rate       float          换手率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
    volume              int            成交量
    turnover            float          成交额
    change_rate         float          涨跌幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
    last_close          float          昨收价
    =================   ===========   ==============================================================================

	失败时返回(RET_ERROR, data)，其中data是错误描述字符串
	
 :Example:

 .. code-block:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_multiple_history_kline(['HK.00700'], '2017-06-20', '2017-06-25', KLType.K_DAY, AuType.QFQ))
    quote_ctx.close()

:strike:`get_history_kline`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_history_kline(self, code, start=None, end=None, ktype=KLType.K_DAY, autype=AuType.QFQ, fields=[KL_FIELD.ALL])

 :strike:`得到本地历史k线，需先参照帮助文档下载k线`

 :param code: 股票代码
 :param start: 开始时间，例如'2017-06-20'。
 :param end:  结束时间，例如'2017-06-30'。
            start和end的组合如下：
			
              ==========    ==========    ========================================
              start类型      end类型       说明
              ==========    ==========    ========================================
                str            str           start和end分别为指定的日期
                None           str           start为end往前365天
                str            None          end为start往后365天
                None           None          end为当前日期，start为end往前365天
              ==========    ==========    ========================================
 :param ktype: k线类型， 参见 KLType_ 定义
 :param autype: 复权类型, 参见 AuType_ 定义
 :param fields: 需返回的字段列表，参见 KL_FIELD_ 定义 KL_FIELD.ALL  KL_FIELD.OPEN ....
 :return: (ret, data)

        ret == RET_OK 返回pd Dataframe数据, 数据列格式如下

        ret != RET_OK 返回错误字符串

    =================   ===========   ==============================================================================
    参数                  类型                        说明
    =================   ===========   ==============================================================================
    code                str            股票代码
    time_key            str            k线时间（港股A股默认是北京时间）
    open                float          开盘价
    close               float          收盘价
    high                float          最高价
    low                 float          最低价
    pe_ratio            float          市盈率
    turnover_rate       float          换手率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
    volume              int            成交量
    turnover            float          成交额
    change_rate         float          涨跌幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
    last_close          float          昨收价
    =================   ===========   ==============================================================================

	
 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_history_kline('HK.00700', start='2017-06-20', end='2017-06-22'))
    quote_ctx.close()

request_history_kline
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: request_history_kline(self, code, start=None, end=None, ktype=KLType.K_DAY, autype=AuType.QFQ, fields=[KL_FIELD.ALL], max_count=1000, page_req_key=None)

 获取k线，不需要事先下载k线数据。

 :param code: 股票代码
 :param start: 开始时间，例如'2017-06-20'
 :param end:  结束时间，例如'2017-07-20'。
              start和end的组合如下：
			  
              ==========    ==========    ========================================
              start类型      end类型       说明
              ==========    ==========    ========================================
                str            str           start和end分别为指定的日期
                None           str           start为end往前365天
                str            None          end为start往后365天
                None           None          end为当前日期，start为end往前365天
              ==========    ==========    ========================================
			  
 :param ktype: k线类型， 参见 KLType_ 定义
 :param autype: 复权类型, 参见 AuType_ 定义
 :param fields: 需返回的字段列表，参见 KL_FIELD_ 定义 KL_FIELD.ALL  KL_FIELD.OPEN ....
 :param max_count: 本次请求最大返回的数据点个数，传None表示返回start和end之间所有的数据。
 :param page_req_key: 分页请求的key。如果start和end之间的数据点多于max_count，那么后续请求时，要传入上次调用返回的page_req_key。初始请求时应该传None。
 :return: (ret, data, page_req_key)

        ret == RET_OK 返回pd dataframe数据，data.DataFrame数据, 数据列格式如下。page_req_key在分页请求时（即max_count>0）可能返回，并且需要在后续的请求中传入。如果没有更多数据，page_req_key返回None。

        ret != RET_OK 返回错误字符串

    =================   ===========   ==============================================================================
    参数                  类型                        说明
    =================   ===========   ==============================================================================
    code                str            股票代码
    time_key            str            k线时间（港股A股默认是北京时间）
    open                float          开盘价
    close               float          收盘价
    high                float          最高价
    low                 float          最低价
    pe_ratio            float          市盈率（该字段为比例字段，默认不展示%）
    turnover_rate       float          换手率
    volume              int            成交量
    turnover            float          成交额
    change_rate         float          涨跌幅
	last_close          float          昨收价
    =================   ===========   ==============================================================================

	
 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    ret, data, page_req_key = quote_ctx.request_history_kline('HK.00700', start='2017-06-20', end='2018-06-22', max_count=50) #请求开头50个数据
    print(ret, data)
    ret, data, page_req_key = quote_ctx.request_history_kline('HK.00700', start='2017-06-20', end='2018-06-22', max_count=50, page_req_key=page_req_key) #请求下50个数据
    print(ret, data)
    quote_ctx.close()

.. note::

    * 接口限制请参见 `在线获取单只股票一段历史K线限制 <../protocol/intro.html#id41>`_
	
:strike:`get_autype_list`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_autype_list(self, code_list)

 获取给定股票列表的复权因子

 :param code_list: 股票列表，例如['HK.00700']
 :return: (ret, data)

        ret == RET_OK 返回pd dataframe数据，data.DataFrame数据, 数据列格式如下

        ret != RET_OK 返回错误字符串

 =====================   ===========   ====================================================================================
 参数                      类型                        说明
 =====================   ===========   ====================================================================================
 code                    str            股票代码
 ex_div_date             str            除权除息日
 split_ratio             float          拆合股比例（该字段为比例字段，展示为小数表示）例如，对于5股合1股为5.0，对于1股拆5股为0.2
 per_cash_div            float          每股派现
 per_share_div_ratio     float          每股送股比例（该字段为比例字段，展示为小数表示）
 per_share_trans_ratio   float          每股转增股比例（该字段为比例字段，展示为小数表示）
 allotment_ratio         float          每股配股比例（该字段为比例字段，展示为小数表示）
 allotment_price         float          配股价
 stk_spo_ratio           float          增发比例（该字段为比例字段，展示为小数表示）
 stk_spo_price           float          增发价格
 forward_adj_factorA     float          前复权因子A
 forward_adj_factorB     float          前复权因子B
 backward_adj_factorA    float          后复权因子A
 backward_adj_factorB    float          后复权因子B
 =====================   ===========   ====================================================================================
		
 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_autype_list(["HK.00700"]))
    quote_ctx.close()
	
.. note::

	* 复权后价格 = 复权前价格 * 复权因子a + 复权因子b
	
	
get_market_snapshot
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_market_snapshot(self, code_list)

获取市场快照

 :param code_list: 股票列表，股票个数限制参考 `OpenAPI用户等级权限 <../protocol/intro.html#api>`_
 :return: (ret, data)

        ret == RET_OK ,返回pd dataframe数据，data.DataFrame数据, 数据列格式如下

        ret != RET_OK 返回错误字符串

 ===============================   =============   ===================================================================
 参数                                类型                       说明
 ===============================   =============   ===================================================================
 code                               str            股票代码
 update_time                        str            更新时间(yyyy-MM-dd HH:mm:ss)（港股A股默认是北京时间）
 last_price                         float          最新价格
 open_price                         float          今日开盘价
 high_price                         float          最高价格
 low_price                          float          最低价格
 prev_close_price                   float          昨收盘价格
 volume                             int            成交数量
 turnover                           float          成交金额
 turnover_rate                      float          换手率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
 suspension                         bool           是否停牌(True表示停牌)
 listing_date                       str            上市日期 (yyyy-MM-dd)
 equity_valid                       bool           是否正股（为true时以下正股相关字段才有合法数值）
 issued_shares                      int            发行股本
 total_market_val                   float          总市值
 net_asset                          int            资产净值
 net_profit                         int            净利润
 earning_per_share                  float          每股盈利
 outstanding_shares                 int            流通股本
 net_asset_per_share                float          每股净资产
 circular_market_val                float          流通市值
 ey_ratio                           float          收益率（该字段为比例字段，默认不展示%）
 pe_ratio                           float          市盈率（该字段为比例字段，默认不展示%）
 pb_ratio                           float          市净率（该字段为比例字段，默认不展示%）
 pe_ttm_ratio                       float          市盈率TTM（该字段为比例字段，默认不展示%）
 dividend_ttm                       float          股息TTM，派息
 dividend_ratio_ttm                 float          股息率TTM（该字段为百分比字段，默认不展示%，如20实际对应20%。）
 dividend_lfy                       float          股息LFY，上一年度派息
 dividend_lfy_ratio                 float          股息率LFY（该字段为百分比字段，默认不展示%，如20实际对应20%。）
 stock_owner                        str            窝轮所属正股的代码或期权的标的股代码
 wrt_valid                          bool           是否是窝轮（为true时以下窝轮相关的字段才有合法数据）
 wrt_conversion_ratio               float          换股比率
 wrt_type                           str            窝轮类型，参见 WrtType_
 wrt_strike_price                   float          行使价格
 wrt_maturity_date                  str            格式化窝轮到期时间
 wrt_end_trade                      str            格式化窝轮最后交易时间
 wrt_leverage                       float          杠杆比率（倍）
 wrt_ipop                           float          价内/价外（该字段为百分比字段，默认不展示%，如20实际对应20%。）
 wrt_break_even_point               float          打和点
 wrt_conversion_price               float          换股价
 wrt_price_recovery_ratio           float          正股距收回价（该字段为百分比字段，默认不展示%，如20实际对应20%。）
 wrt_score                          float          窝轮综合评分
 wrt_code                           str            窝轮对应的正股（此字段已废除,修改为stock_owner）
 wrt_recovery_price                 float          窝轮收回价
 wrt_street_vol                     float          窝轮街货量
 wrt_issue_vol                      float          窝轮发行量
 wrt_street_ratio                   float          窝轮街货占比（该字段为百分比字段，默认不展示%，如20实际对应20%。）
 wrt_delta                          float          窝轮对冲值
 wrt_implied_volatility             float          窝轮引伸波幅
 wrt_premium                        float          窝轮溢价（该字段为百分比字段，默认不展示%，如20实际对应20%。）
 wrt_upper_strike_price             float          上限价，仅界内证支持该字段
 wrt_lower_strike_price             float          下限价，仅界内证支持该字段
 wrt_inline_price_status            str            界内界外, 参见 PriceType_ ，仅界内证支持该字段
 option_valid                       bool           是否是期权（为true时以下期权相关的字段才有合法数值）
 option_type                        str            期权类型，参见 OptionType_
 strike_time                        str            期权行权日（港股A股默认是北京时间）
 option_strike_price                float          行权价
 option_contract_size               int            每份合约数
 option_open_interest               int            总未平仓合约数
 option_implied_volatility          float          隐含波动率
 option_premium                     float          溢价
 option_delta                       float          希腊值 Delta
 option_gamma                       float          希腊值 Gamma
 option_vega                        float          希腊值 Vega
 option_theta                       float          希腊值 Theta
 option_rho                         float          希腊值 Rho
 option_net_open_interest           int            净未平仓合约数
 option_expiry_date_distance        int            距离到期日天数，负数表示已过期
 option_contract_nominal_value      float          合约名义金额
 option_owner_lot_multiplier        float          相等正股手数，指数期权无该字段
 option_area_type                   str            期权地区类型，见 OptionAreaType_
 option_contract_multiplier         float          合约乘数，指数期权特有字段
 plate_valid                        bool           是否为板块类型（为true时以下板块类型字段才有合法数值）
 plate_raise_count                  int            板块类型上涨支数
 plate_fall_count                   int            板块类型下跌支数
 plate_equal_count                  int            板块类型平盘支数
 index_valid                        bool           是否有指数类型（为true时以下指数类型字段才有合法数值）
 index_raise_count                  int            指数类型上涨支数
 index_fall_count                   int            指数类型下跌支数
 index_equal_count                  int            指数类型平盘支数 
 lot_size                           int            每手股数，期权表示每份合约股数（指数期权无该字段），期货表示合约乘数
 price_spread                       float          当前向上的摆盘价差,亦即摆盘数据的卖档的相邻档位的报价差
 ask_price                          float          卖价
 bid_price                          float          买价
 ask_vol                            float          卖量
 bid_vol                            float          买量
 enable_margin                      bool           是否可融资，如果为true，后两个字段才有意义
 mortgage_ratio                     float          股票抵押率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
 long_margin_initial_ratio          float          融资初始保证金率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
 enable_short_sell                  bool           是否可卖空，如果为true，后三个字段才有意义
 short_sell_rate                    float          卖空参考利率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
 short_available_volume             int            剩余可卖空数量
 short_margin_initial_ratio         float          卖空（融券）初始保证金率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
 sec_status                         str            股票状态，见 SecurityStatus_ 
 amplitude                          float          振幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
 avg_price                          float          平均价
 bid_ask_ratio                      float          委比（该字段为百分比字段，默认不展示%，如20实际对应20%。）
 volume_ratio                       float          量比
 highest52weeks_price               float          52周最高价
 lowest52weeks_price                float          52周最低价
 highest_history_price              float          历史最高价
 lowest_history_price               float          历史最低价
 pre_price                          float          盘前价格
 pre_high_price                     float          盘前最高价
 pre_low_price                      float          盘前最低价
 pre_volume                         int            盘前成交量
 pre_turnover                       float          盘前成交额
 pre_change_val                     float          盘前涨跌额
 pre_change_rate                    float          盘前涨跌幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
 pre_amplitude                      float          盘前振幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
 after_price                        float          盘后价格
 after_high_price                   float          盘后最高价
 after_low_price                    float          盘后最低价
 after_volume                       int            盘后成交量，科创板支持该数据。
 after_turnover                     float          盘后成交额，科创板支持该数据。
 after_change_val                   float          盘后涨跌额
 after_change_rate                  float          盘后涨跌幅（该字段为百分比字段，默认不展示%，如20实际对应20%。） 
 after_amplitude                    float          盘后振幅（该字段为百分比字段，默认不展示%，如20实际对应20%。） 
 future_valid                       bool           是否期货
 future_last_settle_price           float          昨结
 future_position                    float          持仓量
 future_position_change             float          日增仓
 future_main_contract               bool           是否主连合约
 future_last_trade_time             string         最后交易时间，主连，当月，下月等期货没有该字段
 ===============================   =============   ===================================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_market_snapshot(['SH.600000', 'HK.00700']))
    quote_ctx.close()

.. note::

    * 接口限制请参见 `获取股票快照限制 <../protocol/intro.html#id43>`_

get_rt_data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_rt_data(self, code)

 获取指定股票的分时数据

 :param code: 股票代码，例如，HK.00700，US.AAPL
 :return (ret, data): ret == RET_OK 返回pd Dataframe数据, 数据列格式如下

        ret != RET_OK 返回错误字符串

        =====================   ===========   ===================================================================
        参数                      类型                        说明
        =====================   ===========   ===================================================================
        code                    str            股票代码
        time                    str            时间(yyyy-MM-dd HH:mm:ss)（港股A股默认是北京时间）
        is_blank                bool           数据状态；正常数据为False，伪造数据为True
        opened_mins             int            零点到当前多少分钟
        cur_price               float          当前价格
        last_close              float          昨天收盘的价格
        avg_price               float          平均价格（对于期权，该字段为None）
        volume                  float          成交量
        turnover                float          成交金额
        =====================   ===========   ===================================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    quote_ctx.subscribe(['HK.00700'], [SubType.RT_DATA])
    print(quote_ctx.get_rt_data('HK.00700'))
    quote_ctx.close()
	
get_plate_stock
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_plate_stock(self, plate_code, sort_field=SortField.CODE, ascend=True)

 获取特定板块下的股票列表

 :param plate_code: 板块代码，string，例如，“SH.BK0001”，“SH.BK0002”，先利用获取子板块列表函数获取子板块代码
 :param sort_field: 排序字段，SortField，根据哪些字段排序 SortField_
 :param ascend: 排序方向，bool，True升序，False降序

 :return (ret, data): ret == RET_OK 返回pd dataframe数据，data.DataFrame数据, 数据列格式如下

        ret != RET_OK 返回错误字符串

        =====================   ===========   ==============================================================
        参数                      类型                        说明
        =====================   ===========   ==============================================================
        code                    str            股票代码
        lot_size                int            每手股数，期货表示合约乘数
        stock_name              str            股票名称
        stock_type              str            股票类型，参见 SecurityType_
        list_time               str            上市时间（港股A股默认是北京时间）
        stock_id                int            股票id
        main_contract           bool           是否主连合约（期货特有字段）
        last_trade_time         string         最后交易时间（期货特有字段，主连，当月，下月等期货没有该字段）
        =====================   ===========   ==============================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_plate_stock('HK.BK1001'))
    quote_ctx.close()

.. note::

    *   该接口也可用于获取指数成份股, 如获取上证指数成份股:
    * 	接口限制请参见 `获取板块下的股票限制 <../protocol/intro.html#id45>`_
		 .. code:: python
		
		    from futu import *
		    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
		    print(quote_ctx.get_plate_stock('SH.000001'))
		    quote_ctx.close()		
			    
    *   部分常用的板块或指数代码如下:
    
        =====================  ==============================================================
            代码                      说明
        =====================  ==============================================================
        HK.HSI Constituent         恒指成份股
        HK.HSCEI Stock             国指成份股
        HK.Motherboard             港股主板
        HK.GEM                     港股创业板
        HK.BK1910                  所有港股
        HK.BK1911                  主板H股
        HK.BK1912                  创业板H股
        HK.Fund                    港股基金
        HK.BK1600                  富途热门(港)
        HK.BK1986                  港股期货全类型
        HK.BK1987                  港股股指期货
        HK.BK1988                  港股汇率期货
        HK.BK1989                  港股金属期货
        HK.BK1990                  港股股票期货
        SH.3000000                 上海主板
        SH.BK0901                  上证B股
        SH.BK0902                  深证B股 
        SH.3000002                 沪深指数
        SH.3000005                 沪深全部A股
        SH.BK0600                  富途热门(沪深)
        SH.BK0992                  科创板
        SZ.3000001                 深证主板
        SZ.3000003                 中小企业板块
        SZ.3000004                 深证创业板
        US.USAALL                  所有美股
        US.BK2989                  美股期货全类型
        US.BK2990                  美股农产品期货
        US.BK2991                  美股利率期货
        US.BK2992                  美股外汇期货
        US.BK2993                  美股数字货币期货
        US.BK2994                  美股股指期货
        US.BK2995                  美股能源化工期货
        US.BK2996                  美股贵金属期货
        =====================  ==============================================================
   
        
get_plate_list
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_plate_list(self, market, plate_class)

 获取板块集合下的子板块列表

 :param market: 市场标识，注意这里不区分沪和深，输入沪或者深都会返回沪深市场的子板块（这个是和客户端保持一致的）参见 Market_
 :param plate_class: 板块分类，参见 Plate_
 :return (ret, data): ret == RET_OK 返回pd Dataframe数据，数据列格式如下

        ret != RET_OK 返回错误字符串

        =====================   ===========   ==============================================================
        参数                      类型                        说明
        =====================   ===========   ==============================================================
        code                    str            股票代码
        plate_name              str            板块名字
        plate_id                str            板块id
        =====================   ===========   ==============================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_plate_list(Market.HK, Plate.ALL))
    quote_ctx.close()
	
.. note::

    * 	接口限制请参见 `获取板块下的股票限制 <../protocol/intro.html#id45>`_    
	
get_broker_queue
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_broker_queue(self, code)

 获取股票的经纪队列

 :param code: 股票代码
 :return: (ret, bid_frame_table, ask_frame_table)或(ret, err_message, err_message)

        ret == RET_OK，bid_frame_table，ask_frame_table 返回pd dataframe数据，数据列格式如下

        ret != RET_OK 返回错误字符串

        bid_frame_table 经纪买盘数据
        
        =====================   ===========   ==============================================================
        参数                      类型                        说明
        =====================   ===========   ==============================================================
        code                    str             股票代码
        bid_broker_id           int             经纪买盘id
        bid_broker_name         str             经纪买盘名称
        bid_broker_pos          int             经纪档位
        =====================   ===========   ==============================================================

        ask_frame_table 经纪卖盘数据
        
        =====================   ===========   ==============================================================
        参数                      类型                        说明
        =====================   ===========   ==============================================================
        code                    str             股票代码
        ask_broker_id           int             经纪卖盘id
        ask_broker_name         str             经纪卖盘名称
        ask_broker_pos          int             经纪档位
        =====================   ===========   ==============================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    quote_ctx.subscribe(['HK.00700'], [SubType.BROKER])
    print(quote_ctx.get_broker_queue('HK.00700'))
    quote_ctx.close()
		
subscribe
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: subscribe(self, code_list, subtype_list, is_first_push=True, subscribe_push=True)

 订阅注册需要的实时信息，指定股票和订阅的数据类型即可，港股订阅需要Lv2行情。 

 :param code_list: 需要订阅的股票代码列表
 :param subtype_list: 需要订阅的数据类型列表，参见 SubType_
 :param is_first_push: 订阅成功之后是否马上推送一次数据
 :param subscribe_push: 订阅后推送
 :return: (ret, err_message)

        ret == RET_OK err_message为None
        
        ret != RET_OK err_message为错误描述字符串
        
 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.subscribe(['HK.00700'], [SubType.QUOTE]))
    quote_ctx.close()

.. note::

    * 接口限制请参见 `订阅反订阅限制 <../protocol/intro.html#id39>`_
	
		
unsubscribe
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: unsubscribe(self, code_list, subtype_list, unsubscribe_all=False)

 取消订阅
 
 :param code_list: 取消订阅的股票代码列表
 :param subtype_list: 取消订阅的类型，参见 SubType_
 :param unsubscribe_all: 取消所有订阅，为True时忽略其他参数，或可使用 `unsubscribe_all <./Quote_API.html#unsubscribe_all>`_ 接口
 :return: (ret, err_message)
        
        ret == RET_OK err_message为None
        
        ret != RET_OK err_message为错误描述字符串
     
 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.unsubscribe(['HK.00700'], [SubType.QUOTE]))
    quote_ctx.close()	 
  
.. note::

    * 接口限制请参见 `订阅反订阅限制 <../protocol/intro.html#id39>`_

unsubscribe_all
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: unsubscribe_all(self)

 取消所有订阅

 :return: (ret, err_message)

        ret == RET_OK err_message为None

        ret != RET_OK err_message为错误描述字符串

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.unsubscribe_all())
    quote_ctx.close()

.. note::

    * 接口限制请参见 `订阅反订阅限制 <../protocol/intro.html#id39>`_
  
query_subscription
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: query_subscription(self, is_all_conn=True)

 查询已订阅的实时信息

 :param is_all_conn: 是否返回所有连接的订阅状态,不传或者传False只返回当前连接数据
 :return: (ret, data)  
        
        ret != RET_OK 返回错误字符串
        
        ret == RET_OK 返回 定阅信息的字典数据 ，格式如下:
        
 .. code:: python

        {
            'total_used': 4,    # 所有连接已使用的定阅额度
            'own_used': 0,       # 当前连接已使用的定阅额度
            'remain': 496,       #  剩余的定阅额度
            'sub_list':          #  每种定阅类型对应的股票列表
            {
                'BROKER': ['HK.00700', 'HK.02318'],
                'RT_DATA': ['HK.00700', 'HK.02318']
            }
        }

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.query_subscription())
    quote_ctx.close()
        
		
get_global_state
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_global_state(self)

 获取全局状态

 :return: (ret, data)

		ret == RET_OK data为包含全局状态的字典，含义如下

		ret != RET_OK data为错误描述字符串

		=====================   ===========   ==============================================================
		key                      value类型                        说明
		=====================   ===========   ==============================================================
		market_sz               str            深圳市场状态，参见 MarketState_
		market_sh               str            上海市场状态，参见 MarketState_
		market_hk               str            香港市场状态，参见 MarketState_
		market_hkfuture         str            香港期货市场状态，参见 MarketState_
		market_us               str            美国市场状态，参见 MarketState_
		server_ver              str            FutuOpenD版本号
		trd_logined             bool           True：已登录交易服务器，False: 未登录交易服务器
		qot_logined             bool           True：已登录行情服务器，False: 未登录行情服务器
		timestamp               str            当前格林威治时间戳(秒）
		local_timestamp         float          FutuOpenD运行机器的当前时间戳(秒)
		=====================   ===========   ==============================================================
 
 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_global_state())
    quote_ctx.close()

get_stock_quote
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_stock_quote(self, code_list)

 获取订阅股票报价的实时数据，有订阅要求限制

 :param code_list: 股票代码列表，必须确保code_list中的股票均订阅成功后才能够执行
 :return: (ret, data)

        ret == RET_OK 返回pd dataframe数据，数据列格式如下

        ret != RET_OK 返回错误字符串

======================  ===========   ==============================================================
参数                      类型                        说明
======================  ===========   ==============================================================
code                    str            股票代码
data_date               str            日期
data_time               str            时间（港股A股默认是北京时间）
last_price              float          最新价格
open_price              float          今日开盘价
high_price              float          最高价格
low_price               float          最低价格
prev_close_price        float          昨收盘价格
volume                  int            成交数量
turnover                float          成交金额
turnover_rate           float          换手率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
amplitude               int            振幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
suspension              bool           是否停牌(True表示停牌)
listing_date            str            上市日期 (yyyy-MM-dd)
price_spread            float          当前向上的价差，亦即摆盘数据的卖档的相邻档位的报价差
dark_status             str            暗盘交易状态，见 DarkStatus_
sec_status              str            股票状态，见 SecurityStatus_ 
strike_price            float          行权价
contract_size           int            每份合约数
open_interest           int            未平仓合约数
implied_volatility      float          隐含波动率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
premium                 float          溢价（该字段为百分比字段，默认不展示%，如20实际对应20%。）
delta                   float          希腊值 Delta
gamma                   float          希腊值 Gamma
vega                    float          希腊值 Vega
theta                   float          希腊值 Theta
rho                     float          希腊值 Rho
net_open_interest       int            净未平仓合约数
expiry_date_distance    int            距离到期日天数，负数表示已过期
contract_nominal_value  float          合约名义金额
owner_lot_multiplier    float          相等正股手数，指数期权无该字段
option_area_type        str            期权地区类型，见 OptionAreaType_
contract_multiplier     float          合约乘数，指数期权特有字段
pre_price               float          盘前价格
pre_high_price          float          盘前最高价
pre_low_price           float          盘前最低价
pre_volume              int            盘前成交量
pre_turnover            float          盘前成交额
pre_change_val          float          盘前涨跌额
pre_change_rate         float          盘前涨跌幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
pre_amplitude           float          盘前振幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
after_price             float          盘后价格
after_high_price        float          盘后最高价
after_low_price         float          盘后最低价
after_volume            int            盘后成交量，科创板支持此数据。
after_turnover          float          盘后成交额，科创板支持此数据。
after_change_val        float          盘后涨跌额
after_change_rate       float          盘后涨跌幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
after_amplitude         float          盘后振幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
last_settle_price       float          昨结，期货特有字段
position                float          持仓量，期货特有字段
position_change         float          日增仓，期货特有字段
======================  ===========   ==============================================================
        
 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    code_list = ['US.AAPL210115C185000']
    print(quote_ctx.subscribe(code_list, [SubType.QUOTE]))
    print(quote_ctx.get_stock_quote(code_list))
    quote_ctx.close()
        
get_rt_ticker
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_rt_ticker(self, code, num=500)

 获取指定股票的实时逐笔。取最近num个逐笔

 :param code: 股票代码
 :param num: 最近ticker个数，最多可获取1000个
 :return: (ret, data)

        ret == RET_OK 返回pd dataframe数据，数据列格式如下

        ret != RET_OK 返回错误字符串

        =====================   ===========   ==============================================================
        参数                      类型                        说明
        =====================   ===========   ==============================================================
        code                     str            股票代码
        sequence                 int            逐笔序号
        time                     str            成交时间（港股A股默认是北京时间）
        price                    float          成交价格
        volume                   int            成交数量（股数）
        turnover                 float          成交金额
        ticker_direction         str            逐笔方向
        type                     str            逐笔类型，参见 TickerType_
        =====================   ===========   ==============================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    quote_ctx.subscribe(['HK.00700'], [SubType.TICKER])
    print(quote_ctx.get_rt_ticker('HK.00700', 10))
    quote_ctx.close()
	
.. note::

    * 接口限制请参见 `获取逐笔限制 <../protocol/intro.html#id40>`_
	
get_cur_kline
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_cur_kline(self, code, num, ktype=SubType.K_DAY, autype=AuType.QFQ)

 实时获取指定股票最近num个K线数据

 :param code: 股票代码
 :param num:  k线数据个数，最多1000根
 :param ktype: k线类型，参见 KLType_
 :param autype: 复权类型，参见 AuType_
 :return: (ret, data)

        ret == RET_OK 返回pd dataframe数据，数据列格式如下

        ret != RET_OK 返回错误字符串

        =====================   ===========   ====================================================================
        参数                      类型                        说明
        =====================   ===========   ====================================================================
        code                     str            股票代码
        time_key                 str            时间（港股A股默认是北京时间）
        open                     float          开盘价
        close                    float          收盘价
        high                     float          最高价
        low                      float          最低价
        volume                   int            成交量
        turnover                 float          成交额
        pe_ratio                 float          市盈率
        turnover_rate            float          换手率（该字段为百分比字段，展示为小数表示）
        last_close               float          昨收价（即前一个时间的收盘价。为了效率原因，第一个数据的昨收价可能为0）
        =====================   ===========   ====================================================================
		
 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    quote_ctx.subscribe(['HK.00700'], [SubType.K_DAY])
    print(quote_ctx.get_cur_kline('HK.00700', 10, SubType.K_DAY, AuType.QFQ))
    quote_ctx.close()

.. note::

    * 接口限制请参见 `获取K线限制 <../protocol/intro.html#k>`_
	
get_order_book
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_order_book(self, code)

 获取实时摆盘数据

 :param code: 股票代码
 :return: (ret, data)

 ret == RET_OK 返回字典，数据格式如下::
 
  {
  'code': 股票代码
  'svr_recv_time_bid': 富途服务器从交易所收到数据的时间(for bid) 部分数据的接收时间为零，例如服务器重启或第一次推送的缓存数据。
  'svr_recv_time_ask': 富途服务器从交易所收到数据的时间(for ask)
  'Ask': [ (ask_price1, ask_volume1，order_num), (ask_price2, ask_volume2, order_num),…]
  'Bid': [ (bid_price1, bid_volume1, order_num), (bid_price2, bid_volume2, order_num),…]
  }

 | 'Ask'：卖盘
 | 'Bid': 买盘
 | 每个元组的含义是(委托价格，委托数量，委托订单数)

 ret != RET_OK 返回错误字符串
    
        
 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    quote_ctx.subscribe(['HK.00700'], [SubType.ORDER_BOOK])
    print(quote_ctx.get_order_book('HK.00700'))
    quote_ctx.close()



:strike:`get_multi_points_history_kline`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_multi_points_history_kline(self, code_list, dates, fields, ktype=KLType.K_DAY, autype=AuType.QFQ, no_data_mode=KLNoDataMode.FORWARD)

 从本地历史K线中获取多支股票多个时间点的指定数据列

 :param code_list: 单个或多个股票 'HK.00700'  or  ['HK.00700', 'HK.00001']
 :param dates: 单个或多个日期 '2017-01-01' or ['2017-01-01', '2017-01-02']，最多5个时间点
 :param fields: 单个或多个数据列 KL_FIELD.ALL or [KL_FIELD.DATE_TIME, KL_FIELD.OPEN]
 :param ktype: K线类型 KLType_
 :param autype: 复权类型 AuType_ 
 :param no_data_mode: 指定时间为非交易日时，对应的k线数据取值模式，参见 KLNoDataMode_
 :return: (ret, data)

        ret == RET_OK 返回pd dataframe数据，固定表头包括'code'(代码) 'time_point'(指定的日期) 'data_status' (KLDataStatus)。数据列格式如下

        ret != RET_OK 返回错误字符串

    =================   ===========   ==============================================================================
    参数                  类型                        说明
    =================   ===========   ==============================================================================
    code                str            股票代码
    time_point          str            请求的时间（港股A股默认是北京时间）
    data_status         str            数据点是否有效，参见 KLDataStatus_
    time_key            str            k线时间（港股A股默认是北京时间）
    open                float          开盘价
    close               float          收盘价
    high                float          最高价
    low                 float          最低价
    pe_ratio            float          市盈率
    turnover_rate       float          换手率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
    volume              int            成交量
    turnover            float          成交额
    change_rate         float          涨跌幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
    last_close          float          昨收价
    =================   ===========   ==============================================================================
    
 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_multi_points_history_kline(['HK.00700'], ['2017-06-20', '2017-06-25'], KL_FIELD.ALL, KLType.K_DAY, AuType.QFQ))
    quote_ctx.close()	
	
	
	
get_referencestock_list
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_referencestock_list(self, code, reference_type)


 获取证券的关联数据
 
 :param code: 证券id，str，例如HK.00700
 :param reference_type: 要获得的相关数据，参见 SecurityReferenceType_ 。例如WARRANT，表示获取正股相关的窝轮
 :return: (ret, data)

		ret == RET_OK 返回pd dataframe数据，数据列格式如下

		ret != RET_OK 返回错误字符串
		
    ========================   ===========   ==============================================================================
    参数                        类型           说明
    ========================   ===========   ==============================================================================
    code                        str            证券代码
    lot_size                    int            每手股数，期货表示合约乘数
    stock_type                  str            证券类型，参见 SecurityType_
    stock_name                  str            证券名字
    list_time                   str            上市时间（港股A股默认是北京时间）
    wrt_valid                   bool           是否是窝轮，如果为True，下面wrt开头的字段有效
    wrt_type                    str            窝轮类型，参见 WrtType_
    wrt_code                    str            所属正股
    future_valid                bool           是否是期货，如果为True，下面future开头的字段有效
    future_main_contract        bool           是否主连合约（期货特有字段）
    future_last_trade_time      string         最后交易时间（期货特有字段，主连，当月，下月等期货没有该字段）
    ========================   ===========   ==============================================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_referencestock_list('HK.00700', SecurityReferenceType.WARRANT))
    quote_ctx.close()	

.. note::

    * 	接口限制请参见 `获取股票关联数据限制 <../protocol/intro.html#id47>`_  
	
get_owner_plate
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_owner_plate(self, code_list)

 获取单支或多支股票的所属板块信息列表

 :param code_list: 股票代码列表，仅支持正股、指数。list或str。例如：['HK.00700', 'HK.00001']或者'HK.00700,HK.00001'，最多可传入200只股票
 :return: (ret, data)

        ret == RET_OK 返回pd dataframe数据，data.DataFrame数据, 数据列格式如下

        ret != RET_OK 返回错误字符串

        =====================   ===========   ==============================================================
        参数                      类型                        说明
        =====================   ===========   ==============================================================
        code                    str            证券代码
        plate_code              str            板块代码
        plate_name              str            板块名字
        plate_type              str            板块类型（行业板块或概念板块），查看 Plate_
        =====================   ===========   ==============================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    code_list = ['HK.00700', 'HK.00001']
    print(quote_ctx.get_owner_plate(code_list))
    quote_ctx.close()

.. note::

    * 	接口限制请参见 `获取股票所属板块限制 <../protocol/intro.html#id48>`_  
	
get_holding_change_list
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_holding_change_list(self, code, holder_type, start, end=None)

 获取大股东持股变动列表,只提供美股数据,并最多只返回前100个

 :param code: 股票代码. 例如：'US.AAPL'
 :param holder_type: 持有者类别，查看 StockHolder_
 :param start: 开始时间. 例如：'2016-10-01'
 :param end: 结束时间，例如：'2017-10-01'。
           start与end的组合如下：

           ==========    ==========    ========================================
           start类型      end类型       说明
           ==========    ==========    ========================================
             str            str           start和end分别为指定的日期
             None           str           start为end往前365天
             str            None          end为start往后365天
             None           None          end为当前日期，start为end往前365天
           ==========    ==========    ========================================
			
 :return: (ret, data)

        ret == RET_OK 返回pd dataframe数据，data.DataFrame数据, 数据列格式如下

        ret != RET_OK 返回错误字符串

=====================   ===========   ==============================================================
参数                      类型                        说明
=====================   ===========   ==============================================================
holder_name             str           高管名称
holding_qty             float         持股数
holding_ratio           float         持股比例（该字段为百分比字段，默认不展示%，如20实际对应20%。）
change_qty              float         变动数
change_ratio            float         变动比例（该字段为百分比字段，默认不展示%，如20实际对应20%。是相对于自身的比例，而不是总的。如总股本1万股，持有100股，持股百分比是1%，卖掉50股，变动比例是50%，而不是0.5%）
time                    str           发布时间（美股的时间默认是美东）
=====================   ===========   ==============================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_holding_change_list('US.AAPL', StockHolder.INSTITUTE, '2018-10-01'))
    quote_ctx.close()

.. note::
	* 	接口限制请参见 `获取持股变化列表限制 <../protocol/intro.html#id49>`_  
	
get_option_chain
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_option_chain(self, code, index_option_type=IndexOptionType.NORMAL, start=None, end=None, option_type=OptionType.ALL, option_cond_type=OptionCondType.ALL)

 通过标的股查询期权

 :param code: 股票代码,例如：'HK.02318'
 :param index_option_type: 指数期权类型，查看 IndexOptionType_。正股和其它类型股票忽略该参数。
 :param start: 开始日期，该日期指到期日，例如'2017-08-01'
 :param end: 结束日期（包括这一天），该日期指到期日，例如'2017-08-30'。 注意，时间范围最多30天。
             start和end的组合如下：
			 
                ==========    ==========    ========================================
                 start类型      end类型       说明
                ==========    ==========    ========================================
                 str            str           start和end分别为指定的日期
                 None           str           start为end往前30天
                 str            None          end为start往后30天
                 None           None          start为当前日期，end往后30天
                ==========    ==========    ========================================
				
 :param option_type: 期权类型,,默认全部,全部/看涨/看跌，查看 OptionType_
 :param option_cond_type: 默认全部,全部/价内/价外，查看 OptionCondType_
 :return: (ret, data)

        ret == RET_OK 返回pd dataframe数据，数据列格式如下

        ret != RET_OK 返回错误字符串

        ==================   ===========   ==============================================================
        参数                      类型                        说明
        ==================   ===========   ==============================================================
        code                 str           股票代码
        name                 str           名字
        lot_size             int           每手股数，期权表示每份合约股数（指数期权无该字段）
        stock_type           str           股票类型，参见 SecurityType_
        option_type          str           期权类型，查看 OptionType_
        stock_owner          str           标的股
        strike_time          str           行权日（港股A股默认是北京时间）
        strike_price         float         行权价
        suspension           bool          是否停牌(True表示停牌)
        stock_id             int           股票id
        index_option_type    str           指数期权类型
        ==================   ===========   ==============================================================
	
.. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_option_chain('HK.00700', IndexOptionType.NONE,'2018-08-01', '2018-08-18', OptionType.ALL, OptionCondType.OUTSIDE))
    quote_ctx.close()
	
.. note::

    * 	接口限制请参见 :ref:`获取期权链限制 <get-option-chain-limit>`

get_history_kl_quota
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_history_kl_quota(self, get_detail=False)

 获取已使用过的额度，即当前周期内已经下载过多少只股票

 :param get_detail: 是否返回详细拉取过的历史纪录

        =====================   ===========   ==============================================================
        参数                      类型                        说明
        =====================   ===========   ==============================================================
        code                    str           拉取的股票代码
        request_time            str           最后一次拉取的时间字符串
        =====================   ===========   ==============================================================

 :return: (ret, data)

        ret != RET_OK 返回错误字符串

        ret == RET_OK 返回(used_quota, remain_quota, detail_list)

        =====================   ===========   ==============================================================
        参数                      类型                        说明
        =====================   ===========   ==============================================================
        used_quota              int32           已使用过的额度，即当前周期内已经下载过多少只股票
        remain_quota            int32           剩余额度，30天后额度会恢复
        detail_list             dict list       get_detail为True时返回，每只拉取过的股票的下载时间
        =====================   ===========   ==============================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_history_kl_quota())
    quote_ctx.close()


get_rehab
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_rehab(self, code)

 获取给定股票的复权因子

 :param code: 需要查询的股票代码.

 :return: (ret, data)

        ret != RET_OK 返回错误字符串

        ret == RET_OK 返回pd dataframe数据

=====================   ===========   ====================================================================================
参数                      类型                        说明
=====================   ===========   ====================================================================================
ex_div_date             str            除权除息日
split_ratio             float          拆合股比例（该字段为比例字段，展示为小数表示）例如，对于5股合1股为5.0，对于1股拆5股为0.2
per_cash_div            float          每股派现
per_share_div_ratio     float          每股送股比例（该字段为比例字段，展示为小数表示）
per_share_trans_ratio   float          每股转增股比例（该字段为比例字段，展示为小数表示）
allotment_ratio         float          每股配股比例（该字段为比例字段，展示为小数表示）
allotment_price         float          配股价
stk_spo_ratio           float          增发比例（该字段为比例字段，展示为小数表示）
stk_spo_price           float          增发价格
forward_adj_factorA     float          前复权因子A
forward_adj_factorB     float          前复权因子B
backward_adj_factorA    float          后复权因子A
backward_adj_factorB    float          后复权因子B
=====================   ===========   ====================================================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_rehab("HK.00700"))
    quote_ctx.close()

.. note::

    * 接口限制请参见 `在线获取单只股票复权信息限制 <../protocol/intro.html#id42>`_
	* 复权后价格 = 复权前价格 * 复权因子a + 复权因子b

get_warrant
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_warrant(self, stock_owner='', req=None)

 通过标的股查询窝轮

 :param stock_owner: 所属正股的股票代码,例如：'HK.00700'，会去找腾讯的窝轮，注意有些股票没有对应窝轮牛熊。
 :param req: 请求参数组合，from futu.quote.quote_get_warrant import Request


==========================  ==============    ====================================================================================
参数                          类型               说明
==========================  ==============    ====================================================================================
begin                       int               数据起始点
num                         int               请求数据个数，最大200
sort_field                  SortField         根据哪个字段排序 SortField_
ascend                      bool              升序True, 降序False
type_list                   list              窝轮类型过滤列表 参见 WrtType_
issuer_list                 list              发行人过滤列表 参见 Issuer_
maturity_time_min           str               到期日, 到期日范围的开始时间
maturity_time_max           str               到期日范围的结束时间
ipo_period                  str               上市日 参见 IpoPeriod_
price_type                  str               价内/价外（该字段为百分比字段，默认不展示%，如20实际对应20%。）参见 PriceType_ , 界内证暂不支持界内外筛选
status                      str               窝轮状态 参见 WarrantStatus_
cur_price_min               float             最新价过滤起点
cur_price_max               float             最新价过滤终点
strike_price_min            float             行使价过滤起点
strike_price_max            float             行使价过滤终点
street_min                  float             街货占比, 过滤起点（该字段为百分比字段，默认不展示%，如20实际对应20%。）
street_max                  float             街货占比, 过滤终点（该字段为百分比字段，默认不展示%，如20实际对应20%。）
conversion_min              float             换股比率过滤起点
conversion_max              float             换股比率过滤终点
vol_min                     unsigned int      成交量过滤起点
vol_max                     unsigned int      成交量过滤终点
premium_min                 float             溢价, 过滤起点（该字段为百分比字段，默认不展示%，如20实际对应20%。）
premium_max                 float             溢价, 过滤终点（该字段为百分比字段，默认不展示%，如20实际对应20%。）
leverage_ratio_min          float             杠杆比率过滤起点
leverage_ratio_max          float             杠杆比率过滤终点
delta_min                   float             对冲值过滤起点, 仅认购认沽支持该字段过滤
delta_max                   float             对冲值过滤终点, 仅认购认沽支持该字段过滤
implied_min                 float             引伸波幅过滤起点, 仅认购认沽支持该字段过滤
implied_max                 float             引伸波幅过滤终点, 仅认购认沽支持该字段过滤
recovery_price_min          float             收回价过滤起点, 仅牛熊证支持该字段过滤
recovery_price_max          float             收回价过滤终点, 仅牛熊证支持该字段过滤
price_recovery_ratio_min    float             正股距收回价, 过滤起点, 仅牛熊证支持该字段过滤（该字段为百分比字段，默认不展示%，如20实际对应20%。）
price_recovery_ratio_max    float             正股距收回价, 过滤终点, 仅牛熊证支持该字段过滤（该字段为百分比字段，默认不展示%，如20实际对应20%。）
==========================  ==============    ====================================================================================


 :return: (ret, data)

        ret != RET_OK 返回错误字符串

        ret == RET_OK 返回（warrant_data_list,last_page, all_count）

        warrant_data_list pd dataframe数据，数据列格式如下:

        last_page 是否是最后一页

        all_count 列表总数量



==========================    ================    ====================================================================================
参数                            类型                        说明
==========================    ================    ====================================================================================
stock                          str                窝轮代码
stock_owner                    str                所属正股
type                           str                窝轮类型 参见 WrtType_
issuer                         Issuer             发行人 参见 Issuer_
maturity_time                  str                到期日
maturity_timestamp             float              :strike:`到期日时间戳`
list_time                      str                上市时间
list_timestamp                 float              :strike:`上市时间戳`
last_trade_time                str                最后交易日
last_trade_timestamp           float              :strike:`最后交易日时间戳`
recovery_price                 float              收回价，仅牛熊证支持该字段
conversion_ratio               float              换股比率
lot_size                       int                每手数量
strike_price                   float              行使价
last_close_price               float              昨收价
name                           str                名称
cur_price                      float              当前价
price_change_val               float              涨跌额
status                         str                窝轮状态 参见 WarrantStatus_
bid_price                      float              买入价
ask_price                      float              卖出价
bid_vol                        int                买量
ask_vol                        int                卖量
volume                         unsigned int       成交量
turnover                       float              成交额
score                          float              综合评分
premium                        float              溢价（该字段为百分比字段，默认不展示%，如20实际对应20%。）
break_even_point               float              打和点
leverage                       float              杠杆比率（倍）
ipop                           float              价内/价外（该字段为百分比字段，默认不展示%，如20实际对应20%。）
price_recovery_ratio           float              正股距收回价，仅牛熊证支持该字段（该字段为百分比字段，默认不展示%，如20实际对应20%。）
conversion_price               float              换股价
street_rate                    float              街货占比（该字段为百分比字段，默认不展示%，如20实际对应20%。）
street_vol                     int                街货量
amplitude                      float              振幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
issue_size                     int                发行量
high_price                     float              最高价
low_price                      float              最低价
implied_volatility             float              引伸波幅，仅认购认沽支持该字段
delta                          float              对冲值，仅认购认沽支持该字段
effective_leverage             float              有效杠杆
upper_strike_price             float              上限价，仅界内证支持该字段
lower_strike_price             float              下限价，仅界内证支持该字段
inline_price_status            str                界内界外 参见 PriceType_ ，仅界内证支持该字段
==========================    ================    ====================================================================================

 :Example:

 .. code:: python

    from futu import *
    from futu.quote.quote_get_warrant import Request
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    req=Request()
    req.sort_field=SortField.TURNOVER
    print(quote_ctx.get_warrant("HK.00700",req))
    quote_ctx.close()


.. note::
    * 	接口限制请参见 `获取窝轮限制 <../protocol/intro.html#id51>`_

get_capital_flow
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_capital_flow(self, code)

 获取个股资金流向

 :param code: 需要查询的股票代码.

 :return: (ret, data)

        ret != RET_OK 返回错误字符串

        ret == RET_OK 返回pd dataframe数据

        ========================   ===========   ====================================================================================
        参数                       类型                        说明
        ========================   ===========   ====================================================================================
        in_flow                    float          净流入的资金额度
        capital_flow_item_time     string         开始时间字符串,以分钟为单位
        last_valid_time            string         数据最后有效时间字符串
        ========================   ===========   ====================================================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_capital_flow("HK.00700"))
    quote_ctx.close()

.. note::

    * 	接口限制请参见 `获取资金流向限制 <../protocol/intro.html#id52>`_

get_capital_distribution
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_capital_distribution(self, code)

 获取个股资金分布

 :param code: 需要查询的股票代码.

 :return: (ret, data)

        ret != RET_OK 返回错误字符串

        ret == RET_OK 返回pd dataframe数据

        =====================   ===========   ====================================================================================
        参数                      类型                        说明
        =====================   ===========   ====================================================================================
        capital_in_big          float          流入资金额度，大单
        capital_in_mid          float          流入资金额度，中单
        capital_in_small        float          流入资金额度，小单
        capital_out_big         float          流出资金额度，大单
        capital_out_mid         float          流出资金额度，中单
        capital_out_small       float          流出资金额度，小单
        update_time             str            更新时间字符串
        =====================   ===========   ====================================================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_capital_distribution("HK.00700"))
    quote_ctx.close()

.. note::

    * 	接口限制请参见 `获取资金分布限制 <../protocol/intro.html#id53>`_

get_user_security
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_user_security(self, group_name)

 获取指定分组的自选股列表（不支持持仓分组）

 :param group_name: 需要查询的自选股分组名称.

 :return: (ret, data)

        ret != RET_OK 返回错误字符串

        ret == RET_OK 返回pd dataframe数据

        =================   ===========   ==============================================================================
        参数                  类型                        说明
        =================   ===========   ==============================================================================
        code                str            股票代码
        name                str            名字
        lot_size            int            每手股数，期权表示每份合约股数，期货表示合约乘数
        stock_type          str            股票类型，参见 SecurityType_
        stock_child_type    str            窝轮子类型，参见 WrtType_
        stock_owner         str            窝轮所属正股的代码，或期权标的股的代码
        option_type         str            期权类型，查看 OptionType_
        strike_time         str            期权行权日（港股A股默认是北京时间）
        strike_price        float          期权行权价
        suspension          bool           期权是否停牌(True表示停牌)
        listing_date        str            上市时间
        stock_id            int            股票id
        delisting           bool           是否退市
        main_contract       bool           是否主连合约  
        last_trade_time     string         最后交易时间，主连，当月，下月等期货没有该字段
        =================   ===========   ==============================================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_user_security("MyGroup"))
    quote_ctx.close()

.. note::

    * 	接口限制请参见 `获取指定分组的自选股列表 <../protocol/intro.html#id54>`_

modify_user_security
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: modify_user_security(self, group_name, op, code_list)

 修改指定分组的自选股列表（不支持系统分组）

 :param group_name: 需要修改的自选股分组名称.
 :param op: 操作枚举值.查看 ModifyUserSecurityOp_
 :param code_list: 股票列表，['HK.00700','HK.00701']

 :return: (ret, data)

        ret != RET_OK 返回错误字符串

        ret == RET_OK 'success'

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.modify_user_security("MyGroup", ModifyUserSecurityOp.ADD, ['HK.00700']))
    quote_ctx.close()

.. note::

    * 接口限制请参见 `修改指定分组的自选股列表 <../protocol/intro.html#id55>`_

get_stock_filter
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_stock_filter(self, market, filter_list, plate_code=None, begin=0, num=200)

 获取条件选股

 :param market: 市场标识，注意这里不区分沪和深，输入沪或者深都会返回沪深市场的股票（这个是和客户端保持一致的）参见 Market_
 :param filter_list: 
        | 简单属性筛选条件的枚举值，筛选条件是SimpleFilter，AccumulateFilter或FinancialFilter类型数据的list对象field。
        
        | SimpleFilter对象field的相关参数如下：
        
        ============================================   ===========   ================================================
        参数                                            类型           说明
        ============================================   ===========   ================================================
        stock_field                                    str            StockField 简单属性，取值见 `StockField <Base_API.html#stockfield-simplefilter>`_ 
        filter_min                                     float          区间下限，闭区间
        filter_max                                     float          区间上限，闭区间
        is_no_filter                                   bool           该字段是否需要筛选。
        sort                                           str            SortDir 排序方向，默认不排序，取值见 SortDir_ 
        ============================================   ===========   ================================================
        
        | AccumulateFilter对象field的相关参数如下：
        
        ============================================   ===========   ================================================
        参数                                            类型           说明
        ============================================   ===========   ================================================
        stock_field                                    str            StockField 累积字段，取值见 `StockField <Base_API.html#stockfield-accumulatefilter>`_ 
        filter_min                                     float          区间下限，闭区间
        filter_max                                     float          区间上限，闭区间
        is_no_filter                                   bool           该字段是否需要筛选。
        sort                                           str            SortDir 排序方向，默认不排序，取值见 SortDir_ 
        days                                           int            所筛选的数据的累计天数
        ============================================   ===========   ================================================
        
        | FinancialFilter对象field的相关参数如下：
        
        ============================================   ===========   ================================================
        参数                                            类型           说明
        ============================================   ===========   ================================================
        stock_field                                    str            StockField 财务字段，取值见 `StockField <Base_API.html#stockfield-financialfilter>`_ 
        filter_min                                     float          区间下限，闭区间
        filter_max                                     float          区间上限，闭区间
        is_no_filter                                   bool           该字段是否需要筛选。
        sort                                           str            SortDir 排序方向，默认不排序，取值见 SortDir_ 
        quarter                                        str            财报累积时间，取值见 FinancialQuarter_ 
        ============================================   ===========   ================================================

 :param plate_code: 板块代码，string，例如，“SH.BK0001”，“SH.BK0002”，先利用获取子板块列表函数获取子板块代码。支持的板块代码详情请查看下面的Note。
 :param begin: 数据起始点
 :param num: 请求数据个数，最大200

 :return: 
        | (ret, data)
        | **ret** - ret != RET_OK 返回错误字符串, ret == RET_OK 返回（last_page, all_count, stock_list）。对于不支持的板块，返回的数据是(True, 0, [])。
        | **last_page** - 是否是最后一页
        | **all_count** - 列表总数量
        | **stock_list** - 返回的是SimpleFilter类型数据的list对象ret_list，对象ret_list中stock_code和stock_name默认都会返回，同时filter_list中设置的字段也会返回。返回的数据列字段如下:

============================================   ===========   ==============================================================================
参数                                            类型           说明
============================================   ===========   ==============================================================================
stock_code                                     str            股票代码
stock_name                                     str            股票名字
cur_price                                      float          最新价
cur_price_to_highest_52weeks_ratio             float          (现价 - 52周最高)/52周最高（该字段为百分比字段，默认不展示%，如20实际对应20%。）
cur_price_to_lowest_52weeks_ratio              float          (现价 - 52周最低)/52周最低（该字段为百分比字段，默认不展示%，如20实际对应20%。）
high_price_to_highest_52weeks_ratio            float          (今日最高 - 52周最高)/52周最高（该字段为百分比字段，默认不展示%，如20实际对应20%。）
low_price_to_lowest_52weeks_ratio              float          (今日最低 - 52周最低)/52周最低（该字段为百分比字段，默认不展示%，如20实际对应20%。）
volume_ratio                                   float          量比
bid_ask_ratio                                  float          委比（该字段为百分比字段，默认不展示%，如20实际对应20%。）
lot_price                                      float          每手价格
market_val                                     float          市值
pe_annual                                      float          市盈率
pe_ttm                                         float          市盈率TTM
pb_rate                                        float          市净率
change_rate_5min                               float          五分钟价格涨跌幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
change_rate_begin_year                         float          年初至今价格涨跌幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
\ 
change_rate                                    float          涨跌幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
amplitude                                      float          振幅（该字段为百分比字段，默认不展示%，如20实际对应20%。）
volume                                         float          日均成交量
turnover                                       float          日均成交额
turnover_rate                                  float          换手率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
\ 
net_profit                                     float          净利润
net_profix_growth                              float          净利润增长率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
sum_of_business                                float          营业收入
sum_of_business_growth                         float          营业同比增长率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
net_profit_rate                                float          净利率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
gross_profit_rate                              float          毛利率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
debt_asset_rate                                float          资产负债率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
return_on_equity_rate                          float          净资产收益率（该字段为百分比字段，默认不展示%，如20实际对应20%。）
============================================   ===========   ==============================================================================

 :Example:

 .. code:: python

    from futu import *
    from futu.quote.quote_stockfilter_info import *  
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)     
    simple_filter = SimpleFilter()
    simple_filter.filter_min = 100
    simple_filter.filter_max = 1000
    simple_filter.stock_field = StockField.CUR_PRICE
    simple_filter.is_no_filter = False
    simple_filter.sort = SortDir.ASCEND
    
    acc_filter = AccumulateFilter()
    acc_filter.filter_min = 50
    acc_filter.filter_max = 100
    acc_filter.days = 2
    acc_filter.stock_field = StockField.CHANGE_RATE
    acc_filter.is_no_filter = False
    acc_filter.sort = SortDir.None

    ret, ls = quote_ctx.get_stock_filter(Market.HK, [simple_filter, acc_filter])
    if ret == RET_OK:
        last_page, all_count, ret_list = ls
        print(len(ret_list), all_count, ret_list)
    else:
        print('error: ', ls)

.. note::

    *   接口限制请参见 `获取条件选股 <../protocol/intro.html#id54>`_
    *   条件选股支持的板块或指数代码如下:
    
        =====================  ==============================================================
            代码                      说明
        =====================  ==============================================================
        HK.Motherboard             港股主板
        HK.GEM                     港股创业板
        HK.BK1911                  主板H股
        HK.BK1912                  创业板H股
        US.NYSE                    纽交所
        US.AMEX                    美交所
        US.NASDAQ                  纳斯达克
        SH.3000000                 上海主板
        SZ.3000001                 深证主板
        SZ.3000004                 深证创业板
        =====================  ==============================================================
    *   利用获取子板块列表函数get_plate_list获取子板块代码，条件选股支持的板块分别为1.港股的行业板块和概念板块。2.美股的行业板块。3.沪深的行业板块，概念板块和地域板块。



get_ipo_list
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_ipo_list(self, market)

 获取指定市场的ipo列表

 :param market: 市场标识，注意这里不区分沪和深，输入沪或者深都会返回沪深市场的股票（这个是和客户端保持一致的）参见 Market_

 :return: (ret, data)

        ret != RET_OK 返回错误字符串
  
        ret == RET_OK data为DataFrame类型，字段如下:

==========================================   ===========   ==================================================================================================================================================
参数                                          类型           说明
==========================================   ===========   ==================================================================================================================================================
code                                         str            股票代码，例如'HK.12345'   
name                                         str            股票名称
list_time                                    str            上市日期，美股是预计上市日期
list_timestamp                               float          上市日期时间戳，美股是预计上市日期时间戳
apply_code                                   str            申购代码，A股适用
issue_size                                   int            发行总数，A股适用；发行量，美股适用
online_issue_size                            int            网上发行量，A股适用
apply_upper_limit                            int            申购上限，A股适用
apply_limit_market_value                     int            顶格申购需配市值，A股适用
is_estimate_ipo_price                        bool           是否预估发行价，A股适用
ipo_price                                    float          发行价 预估值会因为募集资金、发行数量、发行费用等数据变动而变动，仅供参考。实际数据公布后会第一时间更新。A股适用
industry_pe_rate                             float          行业市盈率，A股适用
is_estimate_winning_ratio                    bool           是否预估中签率，A股适用
winning_ratio                                float          中签率 该字段为百分比字段，默认不展示%，如20实际对应20%。预估值会因为募集资金、发行数量、发行费用等数据变动而变动，仅供参考。实际数据公布后会第一时间更新。A股适用
issue_pe_rate                                float          发行市盈率，A股适用
apply_time                                   str            申购日期字符串，A股适用
apply_timestamp                              float          申购日期时间戳，A股适用
winning_time                                 str            公布中签日期字符串，A股适用
winning_timestamp                            float          公布中签日期时间戳，A股适用
is_has_won                                   bool           是否已经公布中签号，A股适用
winning_num_data                             str            中签号，A股适用。格式为类似：末"五"位数:12345,12346\n末"六"位数:123456
ipo_price_min                                float          最低发售价，港股适用；最低发行价，美股适用
ipo_price_max                                float          最高发售价，港股适用；最高发行价，美股适用
list_price                                   float          上市价，港股适用
lot_size                                     int            每手股数
entrance_price                               float          入场费，港股适用
is_subscribe_status                          bool           是否为认购状态，True-认购中，False-待上市
apply_end_time                               str            截止认购日期字符串，港股适用
apply_end_timestamp                          float          截止认购日期时间戳 因需处理认购手续，富途认购截止时间会早于交易所公布的日期，港股适用
==========================================   ===========   ==================================================================================================================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_ipo_list(Market.HK))
    quote_ctx.close()

.. note::

    * 接口限制请参见 :ref:`获取IPO列表的限制 <get-ipo-list-limit>`

get_future_info
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_future_info(self, code_list)

 获取期货合约资料

 :param code_list: 期货

 :return: (ret, data)

        ret != RET_OK 返回错误字符串
  
        ret == RET_OK data为DataFrame类型，字段如下:

		=========================   ===========   =====================================================
		参数                         类型           说明
		=========================   ===========   =====================================================
		code                        string         股票代码
		name                        string         股票名称
		owner                       string         标的
		exchange                    string         交易所
		type                        string         合约类型
		size                        float          合约规模
		size_unit                   string         合约规模单位
		price_currency              string         报价货币
		price_unit                  string         报价单位
		min_change                  float          最小变动
		min_change_unit             string         最小变动的单位
		trade_time                  string         交易时间
		time_zone                   string         时区
		last_trade_time             string         最后交易时间，主连，当月，下月等期货没有该字段
		exchange_format_url         string         交易所规格url
		=========================   ===========   =====================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_future_info(["HK.MPImain", "HK.HOImain"]))
    quote_ctx.close()

.. note::

    * 接口限制请参见 `获取期货合约资料限制 <../protocol/intro.html#id57>`_


request_trading_days
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: request_trading_days(self, market, start=None, end=None)

 在线请求交易日,注意该交易日是通过自然日去除周末以及节假日得到，不包括临时休市数据

 :param market: 市场类型，TradeDateMarket_
 :param start: 起始日期。例如'2018-01-01'。
 :param end: 结束日期。例如'2018-01-01'。
         start和end的组合如下：
            
            ==========    ==========    ========================================
            start类型      end类型       说明
            ==========    ==========    ========================================
            str            str           start和end分别为指定的日期
            None           str           start为end往前365天
            str            None          end为start往后365天
            None           None          end为当前日期，start为end往前365天
            ==========    ==========    ========================================
 :return: (ret_code, content)

        成功时返回(RET_OK, content)，content为字典列表，失败时返回(RET_ERROR, content)，其中content是错误描述字符串


        =================   ===========   ==============================================================================
        参数                  类型                        说明
        =================   ===========   ==============================================================================
        time                str            时间
        trade_date_type     str            标志是一天、上午半天、下午半天，参见 TradeDateType_
        =================   ===========   ==============================================================================

 .. code:: python

        [{'time': '2018-12-22', 'trade_date_type': 'WHOLE'},
         {'time': '2018-12-23', 'trade_date_type': 'WHOLE'},
         {'time': '2018-12-24', 'trade_date_type': 'MORNING'}]

..



        
 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.request_trading_days(TradeDateMarket.HK, start='2018-01-01', end='2018-01-10'))
    quote_ctx.close()

.. note::

    * 接口限制请参见 `在线获取交易日 <../protocol/intro.html#id57>`_
	
-----------------------------------------------------------------------------------------------------    

SysNotifyHandlerBase - OpenD通知回调
-----------------------------------------------------------------------------------------------------    

通知OpenD一些重要消息，类似连接断开等。

.. code:: python
    
    from futu import *
	
    class SysNotifyTest(SysNotifyHandlerBase):
        def on_recv_rsp(self, rsp_str):
            ret_code, data = super(SysNotifyTest, self).on_recv_rsp(rsp_pb)
            notify_type, sub_type, msg = data
            if ret_code != RET_OK:
                logger.debug("SysNotifyTest: error, msg: {}".format(msg))
                return RET_ERROR, data
            print(msg)
            return RET_OK, data
			
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    handler = SysNotifyTest()
    quote_ctx.set_handler(handler)
                
-----------------------------------------------------------------------------------------------------

on_recv_rsp
~~~~~~~~~~~~~~~~~~~

..  py:function:: on_recv_rsp(self, rsp_pb)

 在收到OpenD通知推送后会回调到该函数，使用者需要在派生类中覆盖此方法

 注意该回调是在独立子线程中

 :param rsp_pb: 派生类中不需要直接处理该参数
 :return: ret_code, notify_type, sub_type, msg
 
==================   ===========   ===============================================
参数                  类型           说明
==================   ===========   ===============================================
notify_type          str           通知类型，见 SysNotifyType_
sub_type             str           消息类型，不同的notify_type，取值也不同，见下表
msg                  str, dict     消息描述，不同的notify_type，取值也不同，见下表
==================   ===========   ===============================================
  

==============================   ================================   ==============================================
notify_type                       sub_type                             msg
==============================   ================================   ==============================================
SysNotifyType.NONE                None                                 None
SysNotifyType.GTW_EVENT           str, 取值见 GtwEventType_             str，通知描述信息
SysNotifyType.PROGRAM_STATUS      str, 取值见 ProgramStatusType_        str，通知描述信息
SysNotifyType.CONN_STATUS         None                                 | {'qot_logined': bool, 是否已登录行情连接 
                                                                       | 'trd_logined': bool} 是否已登录交易连接 
SysNotifyType.QOT_RIGHT           None                                 | {'hk_qot_right': str, 港股行情权限
                                                                       | 'cn_qot_right': str, A股行情权限
                                                                       | 'us_qot_right': str, 美股行情权限
                                                                       | 'hk_option_qot_right': str, 港股期权行情权限
                                                                       | 'has_us_option_qot_right': bool, 是否有美股期权行情权限
SysNotifyType.API_LEVEL           None                                 str, API用户等级
==============================   ================================   ==============================================

----------------------------

StockQuoteHandlerBase - 实时报价回调
-------------------------------------------

异步处理推送的订阅股票的报价。

.. code:: python
    
    import time
    from futu import *
	
    class StockQuoteTest(StockQuoteHandlerBase):
        def on_recv_rsp(self, rsp_str):
            ret_code, data = super(StockQuoteTest,self).on_recv_rsp(rsp_str)
            if ret_code != RET_OK:
                print("StockQuoteTest: error, msg: %s" % data)
                return RET_ERROR, data

            print("StockQuoteTest ", data) # StockQuoteTest自己的处理逻辑

            return RET_OK, data
			
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    handler = StockQuoteTest()
    quote_ctx.set_handler(handler)
    quote_ctx.subscribe(['HK.00700'], [SubType.QUOTE])
    time.sleep(15)  
    quote_ctx.close()	
                
-------------------------------------------

on_recv_rsp
~~~~~~~~~~~

..  py:function:: on_recv_rsp(self, rsp_pb)

 在收到实时报价推送后会回调到该函数，使用者需要在派生类中覆盖此方法

 注意该回调是在独立子线程中

 :param rsp_pb: 派生类中不需要直接处理该参数
 :return: 参见 get_stock_quote_ 的返回值
    
----------------------------

OrderBookHandlerBase - 实时摆盘回调
-------------------------------------------

异步处理推送的实时摆盘。

.. code:: python
    
    import time
    from futu import *
	
    class OrderBookTest(OrderBookHandlerBase):
        def on_recv_rsp(self, rsp_str):
            ret_code, data = super(OrderBookTest,self).on_recv_rsp(rsp_str)
            if ret_code != RET_OK:
                print("OrderBookTest: error, msg: %s" % data)
                return RET_ERROR, data

            print("OrderBookTest ", data) # OrderBookTest自己的处理逻辑

            return RET_OK, data
			
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    handler = OrderBookTest()
    quote_ctx.set_handler(handler)
    quote_ctx.subscribe(['HK.00700'], [SubType.ORDER_BOOK])
    time.sleep(15)  
    quote_ctx.close()
            
-------------------------------------------

on_recv_rsp
~~~~~~~~~~~

..  py:function:: on_recv_rsp(self, rsp_pb)


 在收到实摆盘数据推送后会回调到该函数，使用者需要在派生类中覆盖此方法

 注意该回调是在独立子线程中

 :param rsp_pb: 派生类中不需要直接处理该参数
 :return: 参见 get_order_book_ 的返回值
    
----------------------------

CurKlineHandlerBase - 实时k线推送回调
-------------------------------------------

异步处理推送的k线数据。

.. code:: python

    import time
    from futu import *

    class CurKlineTest(CurKlineHandlerBase):
        def on_recv_rsp(self, rsp_str):
            ret_code, data = super(CurKlineTest,self).on_recv_rsp(rsp_str)
            if ret_code != RET_OK:
                print("CurKlineTest: error, msg: %s" % data)
                return RET_ERROR, data

            print("CurKlineTest ", data) # CurKlineTest自己的处理逻辑

            return RET_OK, data

    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    handler = CurKlineTest()
    quote_ctx.set_handler(handler)
    quote_ctx.subscribe(['HK.00700'], [SubType.K_1M])
    time.sleep(15)  
    quote_ctx.close()			

-------------------------------------------

on_recv_rsp
~~~~~~~~~~~

..  py:function:: on_recv_rsp(self, rsp_pb)


 在收到实时k线数据推送后会回调到该函数，使用者需要在派生类中覆盖此方法

 注意该回调是在独立子线程中

 :param rsp_pb: 派生类中不需要直接处理该参数
 :return: 参见 get_cur_kline_ 的返回值，推送回调比 get_cur_kline_ 少了市盈率和换手率字段
    
----------------------------

TickerHandlerBase - 实时逐笔推送回调
-------------------------------------------

异步处理推送的逐笔数据。

.. code:: python
    
	import time
	from futu import *
	
	class TickerTest(TickerHandlerBase):
		def on_recv_rsp(self, rsp_str):
			ret_code, data = super(TickerTest,self).on_recv_rsp(rsp_str)
			if ret_code != RET_OK:
				print("CurKlineTest: error, msg: %s" % data)
				return RET_ERROR, data

			print("TickerTest ", data) # TickerTest自己的处理逻辑

			return RET_OK, data
                
	quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
	handler = TickerTest()
	quote_ctx.set_handler(handler)
	quote_ctx.subscribe(['HK.00700'], [SubType.TICKER])
	time.sleep(15)  
	quote_ctx.close()
	
.. note::

    * 行情连接断开重连后，OpenD拉取断开期间的逐笔数据（最多50根）并推送，可通过push_data_type字段区分

-------------------------------------------

on_recv_rsp
~~~~~~~~~~~

..  py:function:: on_recv_rsp(self, rsp_pb)


 在收到实时逐笔数据推送后会回调到该函数，使用者需要在派生类中覆盖此方法

 注意该回调是在独立子线程中

 :param rsp_pb: 派生类中不需要直接处理该参数
 :return: 参见 get_rt_ticker_ 的返回值，回调比get_rt_ticker多返回一个字段：push_data_type，该字段指明数据来源，参见 PushDataType_

----------------------------

RTDataHandlerBase - 实时分时推送回调
-------------------------------------------

异步处理推送的分时数据。

.. code:: python
    
	import time
	from futu import *
	
	class RTDataTest(RTDataHandlerBase):
		def on_recv_rsp(self, rsp_str):
			ret_code, data = super(RTDataTest,self).on_recv_rsp(rsp_str)
			if ret_code != RET_OK:
				print("RTDataTest: error, msg: %s" % data)
				return RET_ERROR, data

			print("RTDataTest ", data) # RTDataTest自己的处理逻辑

			return RET_OK, data
                
	quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
	handler = RTDataTest()
	quote_ctx.set_handler(handler)
	quote_ctx.subscribe(['HK.00700'], [SubType.RT_DATA])
	time.sleep(15)  
	quote_ctx.close()
	
-------------------------------------------

on_recv_rsp
~~~~~~~~~~~

..  py:function:: on_recv_rsp(self, rsp_pb)


 在收到实时逐笔数据推送后会回调到该函数，使用者需要在派生类中覆盖此方法

 注意该回调是在独立子线程中

 :param rsp_pb: 派生类中不需要直接处理该参数
 :return: 参见 get_rt_data_ 的返回值

----------------------------

BrokerHandlerBase - 实时经纪推送回调
-------------------------------------------

异步处理推送的经纪数据。

.. code:: python
    
    class BrokerTest(BrokerHandlerBase):
        def on_recv_rsp(self, rsp_str):
            ret_code, err_or_stock_code, data = super(BrokerTest, self).on_recv_rsp(rsp_str)
            if ret_code != RET_OK:
                print("BrokerTest: error, msg: {}".format(err_or_stock_code))
                return RET_ERROR, data

            print("BrokerTest: stock: {} data: {} ".format(err_or_stock_code, data))  # BrokerTest自己的处理逻辑

            return RET_OK, data


    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    handler = BrokerTest()
    quote_ctx.set_handler(handler)
    quote_ctx.subscribe(['HK.00700'], [SubType.BROKER])
    time.sleep(15)
    quote_ctx.close()
	
-------------------------------------------

on_recv_rsp
~~~~~~~~~~~

..  py:function:: on_recv_rsp(self, rsp_pb)


 在收到实时经纪数据推送后会回调到该函数，使用者需要在派生类中覆盖此方法

 注意该回调是在独立子线程中

 :param rsp_pb: 派生类中不需要直接处理该参数
 :return: 成功时返回(RET_OK, stock_code, [bid_frame_table, ask_frame_table]), 相关frame table含义见 get_broker_queue_ 的返回值说明

          失败时返回(RET_ERROR, ERR_MSG, None)

