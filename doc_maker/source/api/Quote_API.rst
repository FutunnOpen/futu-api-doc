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
 
 .. _PriceReminderType: Base_API.html#priceremindertype
 
 .. _PriceReminderFreq: Base_API.html#pricereminderfreq
 
 .. _SetPriceReminderOp: Base_API.html#setpricereminderop
 
 .. _PriceReminderMarketStatus: Base_API.html#priceremindermarketstatus
 
 .. _UserSecurityGroupType: Base_API.html#usersecuritygrouptype
 
 .. _RelativePosition: Base_API.html#relativeposition-ma-ema-rsi
 
一分钟上手
============

如下范例，创建api行情对象，调用get_market_snapshot获取港股腾讯00700的报价快照数据,最后关闭对象
   
 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    print(quote_ctx.get_market_snapshot('HK.00700'))
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
    
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
        
 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111, is_encrypt=False)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽

close
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: close

关闭上下文对象。默认情况下，futu-api内部创建的线程会阻止进程退出，只有当所有context都close后，进程才能正常退出。但通过SysConfig.set_all_thread_daemon可以设置所有内部线程为daemon线程，这时即使没有调用context的close，进程也可以正常退出。
        
 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
    
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
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽

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
    ret, data = quote_ctx.get_stock_basicinfo(Market.HK, SecurityType.STOCK)
    if ret == RET_OK:
        print(data)
    else:
        print('error:', data)
    print('******************************************')
    ret, data = quote_ctx.get_stock_basicinfo(Market.HK, SecurityType.STOCK, ['HK.00647', 'HK.00700'])
    if ret == RET_OK:
        print(data)
        print(data['name'][0])  # 取第一条的股票名称
        print(data['name'].values.tolist())  # 转为list
    else:
        print('error:', data)
    quote_ctx.close()  # 结束后记得关闭当条连接，防止连接条数用尽

 :Output:

 .. code-block:: python

           code             name  lot_size stock_type stock_child_type stock_owner option_type strike_time strike_price suspension listing_date        stock_id  delisting index_option_type  main_contract last_trade_time
    0      HK.00001               长和       500      STOCK              N/A                                              N/A        N/A   2015-03-18   4440996184065      False               N/A          False  
    ...         ...              ...       ...        ...              ...         ...         ...         ...          ...        ...          ...             ...        ...               ...            ...             ...
    2592   HK.84602  ICBC CNHPREF1-R     10000      STOCK              N/A                                              N/A        N/A   2014-12-11  70497593281146      False               N/A          False                
    
    [2593 rows x 16 columns]
    ******************************************
           code            name  lot_size stock_type stock_child_type stock_owner option_type strike_time strike_price suspension listing_date        stock_id  delisting index_option_type  main_contract last_trade_time
    0  HK.00647  JOYCE BOUTIQUE      2000      STOCK              N/A                                              N/A        N/A   2019-08-27  32607391711879      False               N/A          False                
    1  HK.00700            腾讯控股       100      STOCK              N/A                                              N/A        N/A   2004-06-16  54047868453564      False               N/A          False                
    JOYCE BOUTIQUE
    ['JOYCE BOUTIQUE', '腾讯控股']

.. note::

    * 当传入程序无法识别的股票时（包括很久之前退市的股票和不存在的股票），仍然返回股票信息，用静态信息标志来该股票不存在。统一处理为：code正常显示，name显示为“未知股票”，delisting显示为“true”，其他字段均为默认值（整型默认是0，字符串默认是空字符串）。
    * 跟其他的行情接口不同，其他接口遇到程序无法识别的股票时，会拒绝请求并返回错误描述“未知股票”。



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
 :param max_count: 本次请求最大返回的数据点个数，传None表示返回start和end之间所有的数据。注意：OpenD 是把所有数据请求到再下发给脚本的，如果K线个数过多，建议选择分页，因为超过10秒仍未获取完会提示超时。
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
    ret, data, page_req_key = quote_ctx.request_history_kline('HK.00700', start='2019-09-11', end='2019-09-18', max_count=5)  # 每页5个，请求第一页
    if ret == RET_OK:
        print(data)
        print(data['code'][0])    # 取第一条的股票代码
        print(data['close'].values.tolist())   # 第一页收盘价转为list
    else:
        print('error:', data)
    while page_req_key != None:  # 请求后面的所有结果
        print('*************************************')
        ret, data, page_req_key = quote_ctx.request_history_kline('HK.00700', start='2019-09-11', end='2019-09-18', max_count=5, page_req_key=page_req_key) # 请求翻页后的数据
        if ret == RET_OK:
            print(data)
        else:
            print('error:', data)
    print('All pages are finished!')
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

        code             time_key   open  close   high    low  pe_ratio  turnover_rate    volume      turnover  change_rate  last_close
    0   HK.00700  2019-09-11 00:00:00  341.0  346.0  347.8  339.4    36.405        0.00165  15794903  5.437023e+09     1.466276       340.2
    ..       ...                  ...    ...    ...    ...    ...       ...            ...       ...           ...          ...         ...
    4   HK.00700  2019-09-17 00:00:00  343.2  343.6  345.4  340.6    36.153        0.00097   9225687  3.165508e+09    -0.865551       346.6
    
    [5 rows x 12 columns]
    HK.00700
    [346.0, 349.4, 349.6, 346.6, 343.6]
    *************************************
       code             time_key   open  close   high    low  pe_ratio  turnover_rate   volume      turnover  change_rate  last_close
    0  HK.00700  2019-09-18 00:00:00  344.0  343.0  345.6  341.6     36.09        0.00077  7335407  2.516701e+09    -0.290698       343.6
    All pages are finished!

.. note::

    * 接口限制请参见 :ref:`在线获取单只股票一段历史K线限制 <request-history-kline-limit>`
	

	
get_market_snapshot
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_market_snapshot(self, code_list)

获取市场快照

 :param code_list: 股票列表，股票个数最多400
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
 wrt_issuer_code                    str            发行人代码
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
    
    ret, data = quote_ctx.get_market_snapshot(['SH.600000', 'HK.00700'])
    if ret == RET_OK:
        print(data)
        print(data['code'][0])    # 取第一条的股票代码
        print(data['code'].values.tolist())   # 转为list
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

       code          update_time  last_price  open_price  high_price  low_price  prev_close_price    volume      turnover  turnover_rate  suspension listing_date  lot_size  price_spread  stock_owner  ask_price  bid_price  ask_vol  bid_vol  enable_margin  mortgage_ratio  long_margin_initial_ratio  enable_short_sell  short_sell_rate  short_available_volume  short_margin_initial_ratio  amplitude  avg_price  bid_ask_ratio  volume_ratio  highest52weeks_price  lowest52weeks_price  highest_history_price  lowest_history_price  after_volume  after_turnover sec_status  equity_valid  issued_shares  total_market_val     net_asset    net_profit  earning_per_share  outstanding_shares  circular_market_val  net_asset_per_share  ey_ratio  pe_ratio  pb_ratio  pe_ttm_ratio  ...  wrt_upper_strike_price  wrt_lowe_strike_price  wrt_inline_price_status  option_valid  option_type  strike_time  option_strike_price  option_contract_size  option_open_interest  option_implied_volatility  option_premium  option_delta  option_gamma  option_vega  option_theta  option_rho  option_net_open_interest  option_expiry_date_distance  option_contract_nominal_value  option_owner_lot_multiplier  option_area_type  \
    0  SH.600000  2020-03-26 15:00:00       10.23        10.1       10.37      10.08             10.15  30921803  3.159604e+08          0.110       False   1999-11-10       100          0.01          NaN      10.23      10.22    39048    12300           True             0.0                       60.0              False              NaN                     NaN                         NaN      2.857     10.218         -1.327         0.823                 13.33                 9.82                  13.57             -1.408106             0             0.0     NORMAL          True    29352080397      3.002718e+11  4.801707e+11  5.591571e+10              1.905         28103763899         2.875015e+11               16.359     0.127      5.37     0.625         4.918  ...                     NaN                    NaN                      NaN         False          NaN          NaN                  NaN                   NaN                   NaN                        NaN             NaN           NaN           NaN          NaN           NaN         NaN                       NaN                          NaN                            NaN                          NaN               NaN   
    1   HK.00700  2020-03-26 16:08:06      381.80       386.0      386.00     376.00            380.00  29318892  1.119300e+10          0.307       False   2004-06-16       100          0.20          NaN     382.00     381.80   383400     5900           True            70.0                       44.0              False              NaN                     NaN                         NaN      2.632    381.767         30.192         0.655                420.00               312.20                 474.72             -3.581000             0             0.0     NORMAL          True     9552936193      3.647311e+12  4.776564e+11  1.029998e+11             10.782          9552936193         3.647311e+12               50.001     0.227     35.41     7.635        33.606  ...                     NaN                    NaN                      NaN         False          NaN          NaN                  NaN                   NaN                   NaN                        NaN             NaN           NaN           NaN          NaN           NaN         NaN                       NaN                          NaN                            NaN                          NaN               NaN   
    
       option_contract_multiplier  index_valid  index_raise_count  index_fall_count  index_equal_count  plate_valid  plate_raise_count  plate_fall_count  plate_equal_count  future_valid  future_last_settle_price  future_position  future_position_change  future_main_contract  future_last_trade_time  pre_price  pre_high_price  pre_low_price  pre_volume  pre_turnover  pre_change_val  pre_change_rate  pre_amplitude  after_price  after_high_price  after_low_price  after_change_val  after_change_rate  after_amplitude  
    0                         NaN        False                NaN               NaN                NaN        False                NaN               NaN                NaN         False                       NaN              NaN                     NaN                   NaN                     NaN        N/A             N/A            N/A         N/A           N/A             N/A              N/A            N/A          N/A               N/A              N/A               N/A                N/A              N/A  
    1                         NaN        False                NaN               NaN                NaN        False                NaN               NaN                NaN         False                       NaN              NaN                     NaN                   NaN                     NaN        N/A             N/A            N/A         N/A           N/A             N/A              N/A            N/A          N/A               N/A              N/A               N/A                N/A              N/A  
    
    [2 rows x 123 columns]
    SH.600000
    ['SH.600000', 'HK.00700']

.. note::

    * 接口限制请参见 :ref:`获取股票快照限制 <get-market-snapshot-limit>`


get_rt_data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_rt_data(self, code)

 获取指定股票的分时数据

 :param code: 股票代码，例如，HK.00700
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
    ret_sub, err_message = quote_ctx.subscribe(['HK.00700'], [SubType.RT_DATA], subscribe_push=False)
    # 先订阅分时数据类型。订阅成功后OpenD将持续收到服务器的推送，False代表暂时不需要推送给脚本
    if ret_sub == RET_OK:   # 订阅成功
        ret, data = quote_ctx.get_rt_data('HK.00700')   # 获取一次分时数据
        if ret == RET_OK:
            print(data)
        else:
            print('error:', data)
    else:
        print('subscription failed', err_message)
    quote_ctx.close()   # 关闭当条连接，OpenD会在1分钟后自动取消相应股票相应类型的订阅
	
 :Output:

 .. code:: python

        code                 time  is_blank  opened_mins  cur_price  last_close   avg_price  volume     turnover
    0   HK.00700  2020-03-30 09:30:00     False          570      371.8       382.4  371.800000  923800  343468840.0
    ..       ...                  ...       ...          ...        ...         ...         ...     ...          ...
    32  HK.00700  2020-03-30 10:02:00     False          602      372.8       382.4  372.979075    4100    1527820.0
    
    [33 rows x 9 columns]

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
    
    ret, data = quote_ctx.get_plate_stock('HK.BK1001')
    if ret == RET_OK:
        print(data)
        print(data['stock_name'][0])    # 取第一条的股票名称
        print(data['stock_name'].values.tolist())   # 转为list
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python
 
        code  lot_size stock_name  stock_owner  stock_child_type stock_type   list_time        stock_id  main_contract last_trade_time
    0   HK.00462      4000       天然乳品          NaN               NaN      STOCK  2005-06-10  55589761712590          False                
    ..       ...       ...        ...          ...               ...        ...         ...             ...            ...             ...
    10  HK.06863      1000       辉山乳业          NaN               NaN      STOCK  2013-09-27  68607807593167          False                
    
    [11 rows x 10 columns]
    天然乳品
    ['天然乳品', '现代牧业', '雅士利国际', '原生态牧业', '中国圣牧', '中地乳业', '庄园牧场', '澳优', '蒙牛乳业', '中国飞鹤', '辉山乳业']

.. note::

    *   该接口也可用于获取指数成份股, 如获取上证指数成份股:
	
	*   接口限制请参见 :ref:`获取板块下的股票限制 <get-plate-stock-limit>`

			    
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
		HK.BK1912                  已上市新股-港股
        SH.3000000                 上海主板
        SH.BK0901                  上证B股
        SH.BK0902                  深证B股 
        SH.3000002                 沪深指数
        SH.3000005                 沪深全部A股
        SH.BK0600                  富途热门(沪深)
        SH.BK0992                  科创板
		SH.BK0912                  已上市新股-A股	
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
		US.BK2912                  已上市新股-美股
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
    
    ret, data = quote_ctx.get_plate_list(Market.HK, Plate.CONCEPT)
    if ret == RET_OK:
        print(data)
        print(data['plate_name'][0])    # 取第一条的板块名称
        print(data['plate_name'].values.tolist())   # 转为list
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

        code plate_name plate_id
    0   HK.BK1000      做空集合股   BK1000
    ..        ...        ...      ...
    77  HK.BK1999       殡葬概念   BK1999
    
    [78 rows x 3 columns]
    做空集合股
    ['做空集合股', '阿里概念股', '雄安概念股', '苹果概念', '一带一路', '5G概念', '夜店股', '粤港澳大湾区', '特斯拉概念股', '啤酒', '疑似财技股', '体育用品', '稀土概念', '人民币升值概念', '抗疫概念', '新股与次新股', '腾讯概念', '云办公', 'SaaS概念', '在线教育', '汽车经销商', '挪威政府全球养老基金持仓', '武汉本地概念股', '核电', '内地医药股', '化妆美容股', '科网股', '公用股', '石油股', '电讯设备', '电力股', '手游股', '婴儿及小童用品股', '百货业股', '收租股', '港口运输股', '电信股', '环保', '煤炭股', '汽车股', '电池', '物流', '内地物业管理股', '农业股', '黄金股', '奢侈品股', '电力设备股', '连锁快餐店', '重型机械股', '食品股', '内险股', '纸业股', '水务股', '奶制品股', '光伏太阳能股', '内房股', '内地教育股', '家电股', '风电股', '蓝筹地产股', '内银股', '航空股', '石化股', '建材水泥股', '中资券商股', '高铁基建股', '燃气股', '公路及铁路股', '钢铁金属股', '华为概念', 'OLED概念', '工业大麻', '香港本地股', '香港零售股', '区块链', '猪肉概念', '节假日概念', '殡葬概念']
	
.. note::

    * 接口限制请参见 :ref:`获取板块集合下的板块限制 <get-plate-list-limit>`

	
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
        order_id                int64           交易所订单id，与交易接口返回的订单id并不一样
        order_volume            int64           订单股数
        =====================   ===========   ==============================================================

        ask_frame_table 经纪卖盘数据
        
        =====================   ===========   ==============================================================
        参数                      类型                        说明
        =====================   ===========   ==============================================================
        code                    str             股票代码
        ask_broker_id           int             经纪卖盘id
        ask_broker_name         str             经纪卖盘名称
        ask_broker_pos          int             经纪档位
        order_id                int64           交易所订单id，与交易接口返回的订单id并不一样
        order_volume            int64           订单股数
        =====================   ===========   ==============================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    ret_sub, err_message = quote_ctx.subscribe(['HK.00700'], [SubType.BROKER], subscribe_push=False)
    # 先订阅经纪队列类型。订阅成功后OpenD将持续收到服务器的推送，False代表暂时不需要推送给脚本
    if ret_sub == RET_OK:   # 订阅成功
        ret, bid_frame_table, ask_frame_table = quote_ctx.get_broker_queue('HK.00700')   # 获取一次经纪队列数据
        if ret == RET_OK:
            print(bid_frame_table)
        else:
            print('error:', bid_frame_table)
    else:
        print('subscription failed')
    quote_ctx.close()   # 关闭当条连接，OpenD会在1分钟后自动取消相应股票相应类型的订阅
	
 :Output:

 .. code:: python

        code  bid_broker_id bid_broker_name  bid_broker_pos order_id order_volume
    0   HK.00700            140      海通国际证券有限公司               1      N/A          N/A
    ..       ...            ...             ...             ...      ...          ...
    35  HK.00700           4978  法国兴业证券(香港)有限公司               5      N/A          N/A
    
    [36 rows x 6 columns]

subscribe
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: subscribe(self, code_list, subtype_list, is_first_push=True, subscribe_push=True, is_detailed_orderbook=False)

 订阅注册需要的实时信息，指定股票和订阅的数据类型即可，港股订阅需要Lv2行情。 

 :param code_list: 需要订阅的股票代码列表
 :param subtype_list: 需要订阅的数据类型列表，参见 SubType_
 :param is_first_push: 订阅成功之后是否马上推送一次数据
 :param subscribe_push: 订阅后推送
 :param is_detailed_orderbook: 是否订阅详细的摆盘订单明细，仅用于 SF 行情权限下订阅 ORDER_BOOK 类型
 :return: (ret, err_message)

        ret == RET_OK err_message为None
        
        ret != RET_OK err_message为错误描述字符串
        
 :Example:

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
    quote_ctx.set_handler(handler)  # 设置实时摆盘回调
    quote_ctx.subscribe(['HK.00700'], [SubType.ORDER_BOOK])  # 订阅买卖摆盘类型，OpenD开始持续收到服务器的推送
    time.sleep(15)  #  设置脚本接收OpenD的推送持续时间为15秒
    quote_ctx.close()  # 关闭当条连接，OpenD会在1分钟后自动取消相应股票相应类型的订阅
	
 :Output:

 .. code:: python

    OrderBookTest  {'code': 'HK.00700', 'svr_recv_time_bid': '2020-04-29 10:40:03.147', 'svr_recv_time_ask': '2020-04-29 10:40:03.147', 'Bid': [(416.8, 2600, 11, {}), (416.6, 13100, 17, {}), (416.4, 24600, 17, {}), (416.2, 28000, 13, {}), (416.0, 46900, 30, {}), (415.8, 10900, 7, {}), (415.6, 7100, 9, {}), (415.4, 13300, 3, {}), (415.2, 300, 3, {}), (415.0, 11200, 36, {})], 'Ask': [(417.0, 17600, 31, {}), (417.2, 17800, 24, {}), (417.4, 15300, 10, {}), (417.6, 28800, 17, {}), (417.8, 20700, 11, {}), (418.0, 114200, 155, {}), (418.2, 20600, 19, {}), (418.4, 24100, 28, {}), (418.6, 42700, 45, {}), (418.8, 181900, 76, {})]}

.. note::
    
	* 接口限制请参见 :ref:`订阅反订阅限制 <subscribe-limit>`


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
    import time
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    
    print('current subscription status :', quote_ctx.query_subscription())  # 查询初始订阅状态
    ret_sub, err_message = quote_ctx.subscribe(['HK.00700'], [SubType.QUOTE, SubType.TICKER], subscribe_push=False)
    # 先订阅了QUOTE和TICKER两个类型。订阅成功后OpenD将持续收到服务器的推送，False代表暂时不需要推送给脚本
    if ret_sub == RET_OK:   # 订阅成功
        print('subscribe successfully！current subscription status :', quote_ctx.query_subscription())  # 订阅成功后查询订阅状态
        time.sleep(60)  # 订阅之后至少1分钟才能取消订阅
        ret_unsub, err_message_unsub = quote_ctx.unsubscribe(['HK.00700'], [SubType.QUOTE])
        if ret_unsub == RET_OK:
            print('unsubscribe all successfully！current subscription status:', quote_ctx.query_subscription())  # 取消订阅后查询订阅状态
        else:
            print('Failed to cancel all subscriptions！', err_message_unsub)
    else:
        print('subscription failed', err_message)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

    current subscription status : (0, {'total_used': 0, 'remain': 1000, 'own_used': 0, 'sub_list': {}})
    subscribe successfully！current subscription status : (0, {'total_used': 2, 'remain': 998, 'own_used': 2, 'sub_list': {'QUOTE': ['HK.00700'], 'TICKER': ['HK.00700']}})
    unsubscribe all successfully！current subscription status: (0, {'total_used': 1, 'remain': 999, 'own_used': 1, 'sub_list': {'TICKER': ['HK.00700']}})
  
.. note::

	* 接口限制请参见 :ref:`订阅反订阅限制 <subscribe-limit>`

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
    import time
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    
    print('current subscription status :', quote_ctx.query_subscription())  # 查询初始订阅状态
    ret_sub, err_message = quote_ctx.subscribe(['HK.00700'], [SubType.QUOTE, SubType.TICKER], subscribe_push=False)
    # 先订阅了QUOTE和TICKER两个类型。订阅成功后OpenD将持续收到服务器的推送，False代表暂时不需要推送给脚本
    if ret_sub == RET_OK:  # 订阅成功
        print('subscribe successfully！current subscription status :', quote_ctx.query_subscription())  # 订阅成功后查询订阅状态
        time.sleep(60)  # 订阅之后至少1分钟才能取消订阅
        ret_unsub, err_message_unsub = quote_ctx.unsubscribe_all()  # 取消所有订阅
        if ret_unsub == RET_OK:
            print('unsubscribe successfully！current subscription status:', quote_ctx.query_subscription())  # 取消订阅后查询订阅状态
        else:
            print('unsubscription failed', err_message_unsub)
    else:
        print('subscription failed', err_message)
    quote_ctx.close()  # 结束后记得关闭当条连接，防止连接条数用尽

 :Output:

 .. code:: python

	current subscription status : (0, {'total_used': 0, 'remain': 1000, 'own_used': 0, 'sub_list': {}})
	subscribe successfully！current subscription status : (0, {'total_used': 2, 'remain': 998, 'own_used': 2, 'sub_list': {'QUOTE': ['HK.00700'], 'TICKER': ['HK.00700']}})
	unsubscribe successfully！current subscription status: (0, {'total_used': 0, 'remain': 1000, 'own_used': 0, 'sub_list': {}})

.. note::

	* 接口限制请参见 :ref:`订阅反订阅限制 <subscribe-limit>`
  
query_subscription
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: query_subscription(self, is_all_conn=True)

 查询已订阅的实时信息

 :param is_all_conn: 是否返回所有连接的订阅状态,不传或者传False只返回当前连接数据
 :return: (ret, data)  
        
        ret != RET_OK 返回错误字符串
        
        ret == RET_OK 返回订阅信息的字典数据 ，格式如下:
        
 .. code:: python

        {
            'total_used': 4,    # 所有连接已使用的订阅额度
            'own_used': 0,       # 当前连接已使用的订阅额度
            'remain': 496,       #  剩余的订阅额度
            'sub_list':          #  每种订阅类型对应的股票列表
            {
                'BROKER': ['HK.00700', 'HK.02318'],
                'RT_DATA': ['HK.00700', 'HK.02318']
            }
        }

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    
    quote_ctx.subscribe(['HK.00700'], [SubType.QUOTE])
    ret, data = quote_ctx.query_subscription()
    if ret == RET_OK:
        print(data)
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

    {'total_used': 1, 'remain': 999, 'own_used': 1, 'sub_list': {'QUOTE': ['HK.00700']}}
	
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
		market_usfuture         str            香港期货市场状态，参见 MarketState_
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
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽

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
    
    ret_sub, err_message = quote_ctx.subscribe(['HK.00700'], [SubType.QUOTE], subscribe_push=False)
    # 先订阅k线类型。订阅成功后OpenD将持续收到服务器的推送，False代表暂时不需要推送给脚本
    if ret_sub == RET_OK:  # 订阅成功
        ret, data = quote_ctx.get_stock_quote(['HK.00700'])  # 获取订阅股票报价的实时数据
        if ret == RET_OK:
            print(data)
            print(data['code'][0])   # 取第一条的股票代码
            print(data['code'].values.tolist())   # 转为list
        else:
            print('error:', data)
    else:
        print('subscription failed', err_message)
    quote_ctx.close()  # 关闭当条连接，OpenD会在1分钟后自动取消相应股票相应类型的订阅
	
 :Output:

 .. code:: python

       code   data_date data_time  last_price  open_price  high_price  low_price  prev_close_price    volume      turnover  turnover_rate  amplitude  suspension listing_date  price_spread dark_status sec_status strike_price contract_size open_interest implied_volatility premium delta gamma vega theta  rho net_open_interest expiry_date_distance contract_nominal_value owner_lot_multiplier option_area_type contract_multiplier last_settle_price position position_change pre_price pre_high_price pre_low_price pre_volume pre_turnover pre_change_val pre_change_rate pre_amplitude after_price after_high_price after_low_price after_volume after_turnover after_change_val after_change_rate after_amplitude
    0  HK.00700  2020-03-30  15:49:19       376.8       371.8       380.0      371.6             382.4  18829170  7.055425e+09          0.197      2.197       False   2004-06-16           0.2         N/A     NORMAL          N/A           N/A           N/A                N/A     N/A   N/A   N/A  N/A   N/A  N/A               N/A                  N/A                    N/A                  N/A              N/A                 N/A               N/A      N/A             N/A       N/A            N/A           N/A        N/A          N/A            N/A             N/A           N/A         N/A              N/A             N/A          N/A            N/A              N/A               N/A             N/A
    HK.00700
    ['HK.00700']
    
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
    
    ret_sub, err_message = quote_ctx.subscribe(['HK.00700'], [SubType.TICKER], subscribe_push=False)
    # 先订阅逐笔类型。订阅成功后OpenD将持续收到服务器的推送，False代表暂时不需要推送给脚本
    if ret_sub == RET_OK:  # 订阅成功
        ret, data = quote_ctx.get_rt_ticker('HK.00700', 2)  # 获取港股00700最近2个逐笔
        if ret == RET_OK:
            print(data)
            print(data['turnover'][0])   # 取第一条的成交金额
            print(data['turnover'].values.tolist())   # 转为list
        else:
            print('error:', data)
    else:
        print('subscription failed', err_message)
    quote_ctx.close()  # 关闭当条连接，OpenD会在1分钟后自动取消相应股票相应类型的订阅
	
 :Output:

 .. code:: python

       code                 time  price   volume     turnover ticker_direction             sequence     type
    0  HK.00700  2020-03-30 16:04:41  372.4       60      22344.0          NEUTRAL  6809908936888567318  ODD_LOT
    1  HK.00700  2020-03-30 16:08:17  376.6  2198400  827917440.0          NEUTRAL  6809909864601503255  AUCTION
    22344.0
    [22344.0, 827917440.0]
	
.. note::

	* 接口限制请参见 :ref:`获取逐笔限制 <get-rt-ticker-limit>`

	
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
    
    ret_sub, err_message = quote_ctx.subscribe(['HK.00700'], [SubType.K_DAY], subscribe_push=False)
    # 先订阅k线类型。订阅成功后OpenD将持续收到服务器的推送，False代表暂时不需要推送给脚本
    if ret_sub == RET_OK:  # 订阅成功
        ret, data = quote_ctx.get_cur_kline('HK.00700', 2, SubType.K_DAY, AuType.QFQ)  # 获取港股00700最近2个K线数据
        if ret == RET_OK:
            print(data)
            print(data['turnover_rate'][0])   # 取第一条的换手率
            print(data['turnover_rate'].values.tolist())   # 转为list
        else:
            print('error:', data)
    else:
        print('subscription failed', err_message)
    quote_ctx.close()  # 关闭当条连接，OpenD会在1分钟后自动取消相应股票相应类型的订阅

	
 :Output:

 .. code:: python

       code             time_key   open  close   high    low    volume      turnover  pe_ratio  turnover_rate  last_close
    0  HK.00700  2020-03-27 00:00:00  390.0  382.4  390.0  381.8  28738698  1.103966e+10    35.466        0.00301       381.8
    1  HK.00700  2020-03-30 00:00:00  371.8  376.6  380.0  371.6  21838731  8.188543e+09    34.928        0.00229       382.4
    0.00301
    [0.00301, 0.00229]

.. note::

	* 接口限制请参见 :ref:`获取K线限制 <get-cur-kline-limit>`
	
get_order_book
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_order_book(self, code, num = 10)

 获取实时摆盘数据

 :param code: 股票代码
 :param num: 请求摆盘档数，LV2行情用户最多可以获取10档，SF 行情用户可以获取全盘
 :return: (ret, data)

 ret != RET_OK 返回错误字符串
 
 ret == RET_OK 返回字典，数据格式如下::
 
  {
  'code': 股票代码
  'svr_recv_time_bid': 富途服务器从交易所收到数据的时间(for bid) 部分数据的接收时间为零，例如服务器重启或第一次推送的缓存数据。
  'svr_recv_time_ask': 富途服务器从交易所收到数据的时间(for ask)
  'Ask': [ (ask_price1, ask_volume1，order_num, {'orderid1': order_volume1, 'orderid2': order_volume2, …… }), (ask_price2, ask_volume2, order_num, {'orderid1': order_volume1, 'orderid2': order_volume2, …… }),…]
  'Bid': [ (bid_price1, bid_volume1, order_num, {'orderid1': order_volume1, 'orderid2': order_volume2, …… }), (bid_price2, bid_volume2, order_num,  {'orderid1': order_volume1, 'orderid2': order_volume2, …… }),…]
  }

 | 'Ask'：卖盘
 | 'Bid': 买盘
 | 每个元组的含义是:
 | 委托价格
 | 委托数量
 | 委托订单数
 | 委托订单明细（仅港股SF行情支持此字段，最多支持1000笔委托订单明细）

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    ret_sub = quote_ctx.subscribe(['HK.00700'], [SubType.ORDER_BOOK], subscribe_push=False)[0]
    # 先订阅买卖摆盘类型。订阅成功后 OpenD 将持续收到服务器的推送，False 代表暂时不需要推送给脚本
    if ret_sub == RET_OK:  # 订阅成功
        ret, data = quote_ctx.get_order_book('HK.00700', num=3)  # 获取一次 3 档实时摆盘数据
        if ret == RET_OK:
            print(data)
        else:
            print('error:', data)
    else:
        print('subscription failed')
    quote_ctx.close()  # 关闭当条连接，OpenD 会在 1 分钟后自动取消相应股票相应类型的订阅
	
 :Output:

 .. code:: python

    {'code': 'HK.00700', 'svr_recv_time_bid': '', 'svr_recv_time_ask': '', 'Bid': [(384.2, 15400, 6, {}), (384.0, 3700, 7, {}), (383.8, 6600, 10, {})], 'Ask': [(384.4, 3000, 9, {}), (384.6, 25800, 23, {}), (384.8, 19100, 27, {})]}
	
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
    future_last_trade_time      string         最后交易时间（期货特有字段，主连，当月，下月等无该字段）
    ========================   ===========   ==============================================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    
    # 获取正股相关的窝轮
    ret, data = quote_ctx.get_referencestock_list('HK.00700', SecurityReferenceType.WARRANT)
    if ret == RET_OK:
        print(data)
        print(data['code'][0])    # 取第一条的股票代码
        print(data['code'].values.tolist())   # 转为list
    else:
        print('error:', data)
    print('******************************************')
    # 港期相关合约
    ret, data = quote_ctx.get_referencestock_list('HK.A50main', SecurityReferenceType.FUTURE)
    if ret == RET_OK:
        print(data)
        print(data['code'][0])    # 取第一条的股票代码
        print(data['code'].values.tolist())   # 转为list
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

          code  lot_size stock_type stock_name   list_time  wrt_valid wrt_type  wrt_code  future_valid  future_main_contract  future_last_trade_time
    0     HK.47096      1000    WARRANT   腾讯摩通零三界A  2019-07-25       True   INLINE  HK.00700         False                   NaN                     NaN
    ...        ...       ...        ...        ...         ...        ...      ...       ...           ...                   ...                     ...
    1396  HK.58533     10000    WARRANT   腾讯高盛零九熊A  2020-04-01       True     BEAR  HK.00700         False                   NaN                     NaN
    
    [1397 rows x 11 columns]
    HK.47096
    ['HK.47096', 'HK.24512', 'HK.24719', 'HK.27886', 'HK.28043', 'HK.28152', 'HK.28621', 'HK.14339', 'HK.17017',
    ...        ...       ...        ...        ...         ...        ...      ...       ... 
     'HK.58474', 'HK.58533']
    ******************************************
          code  lot_size stock_type         stock_name list_time  wrt_valid  wrt_type  wrt_code  future_valid  future_main_contract future_last_trade_time
    0   HK.A50main      5000     FUTURE  安硕富时 A50 ETF 期货主连                False       NaN       NaN          True                  True                       
    ..         ...       ...        ...                ...       ...        ...       ...       ...           ...                   ...                    ...
    5   HK.A502009      5000     FUTURE  安硕富时 A50 ETF 2009                False       NaN       NaN          True                 False             2020-09-29
    
    [6 rows x 11 columns]
    HK.A50main
    ['HK.A50main', 'HK.A502003', 'HK.A502004', 'HK.A502005', 'HK.A502006', 'HK.A502009']

.. note::

	* 接口限制请参见 :ref:`获取股票关联数据限制 <get-referencestock-list-limit>`

	
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
    
    code_list = ['HK.00001']
    ret, data = quote_ctx.get_owner_plate(code_list)
    if ret == RET_OK:
        print(data)
        print(data['code'][0])    # 取第一条的股票代码
        print(data['plate_code'].values.tolist())   # 板块代码转为list
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

        code          plate_code plate_name plate_type
    0   HK.00001  HK.HSI Constituent      恒指成份股      OTHER
    ..       ...                 ...        ...        ...
    10  HK.00001           HK.BK1993      香港本地股    CONCEPT
    
    [11 rows x 4 columns]
    HK.00001
    ['HK.HSI Constituent', 'HK.GangGuTong', 'HK.BK1000', 'HK.BK1061', 'HK.BK1107', 'HK.BK1197', 'HK.BK1249', 'HK.BK1600', 'HK.BK1609', 'HK.BK1922', 'HK.BK1983', 'HK.BK1993']

.. note::

	* 接口限制请参见 :ref:`获取股票所属板块限制 <get-owner-plate-limit>`

	
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
    
    ret, data = quote_ctx.get_holding_change_list('US.AAPL', StockHolder.INSTITUTE, '2018-10-01')
    if ret == RET_OK:
        print(data)
        print(data['holding_ratio'][0])   # 取第一条的持股比例
        print(data['holding_ratio'].values.tolist())   # 转为list
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

        holder_name  holding_qty  holding_ratio  change_qty  change_ratio                 time
    0                            The Vanguard Group, Inc.  331132509.0         7.4525  -3981157.0         -1.19  2019-09-30 00:00:00
    ..                                                ...          ...            ...         ...           ...                  ...
    92  Los Angeles Capital Management And Equity Rese...    4007663.0         0.0902    -57784.0         -1.42  2019-09-30 00:00:00
    
    [93 rows x 6 columns]
    7.4525
    [7.4525, 5.6004, 4.3059, 4.1463, 2.1087, 1.3884, 1.0522, 0.905, 0.7931, 0.7871, 0.7782, 0.6872, 0.6687, 0.666, 0.6186, 0.0956, 0.0954, 0.0903, 
    ..                                                ...          ...            ...         ...           ...                  ...
    0.0903, 0.0902]

.. note::

	* 接口限制请参见 :ref:`获取持股变化列表限制 <get-holding-change-list-limit>`

	
get_option_chain
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_option_chain(self, code, index_option_type=IndexOptionType.NORMAL, start=None, end=None, option_type=OptionType.ALL, option_cond_type=OptionCondType.ALL, data_filter=None)

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
 :param data_filter: 数据筛选条件，默认不筛选，参考OptionDataFilter。
                OptionDataFilter字段如下：
				
                ============================    ==========    ========================================
                 字段                            类型           说明
                ============================    ==========    ========================================
                 implied_volatility_min         float          隐含波动率过滤起点 %
                 implied_volatility_max         float          隐含波动率过滤终点 %
                 delta_min                      float          希腊值 Delta过滤起点
                 delta_max                      float          希腊值 Delta过滤终点
                 gamma_min                      float          希腊值 Gamma过滤起点
                 gamma_max                      float          希腊值 Gamma过滤终点
                 vega_min                       float          希腊值 Vega过滤起点
                 vega_max                       float          希腊值 Vega过滤终点
                 theta_min                      float          希腊值 Theta过滤起点
                 theta_max                      float          希腊值 Theta过滤终点
                 rho_min                        float          希腊值 Rho过滤起点
                 rho_max                        float          希腊值 Rho过滤终点
                 net_open_interest_min          float          净未平仓合约数过滤起点
                 net_open_interest_max          float          净未平仓合约数过滤终点
                 open_interest_min              float          未平仓合约数过滤起点
                 open_interest_max              float          未平仓合约数过滤终点
                 vol_min                        float          成交量过滤起点
                 vol_max                        float          成交量过滤终点
                ============================    ==========    ========================================

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

 :Example:

 .. code-block:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    
    ret, data = quote_ctx.get_option_chain('HK.00700', IndexOptionType.SMALL, '2020-03-28', '2020-04-02', OptionType.CALL, OptionCondType.WITHIN)
    if ret == RET_OK:
        print(data)
        print(data['code'][0])    # 取第一条的股票代码
        print(data['code'].values.tolist())   # 转为list
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

        code                name  lot_size stock_type option_type stock_owner strike_time  strike_price  suspension  stock_id index_option_type
    0   HK.TCH200330C250000  腾讯 200330 250.00 购       100       DRVT        CALL    HK.00700  2020-03-30         250.0       False  80049079               N/A
    ..                  ...                 ...       ...        ...         ...         ...         ...           ...         ...       ...               ...
    18  HK.TCH200330C380000  腾讯 200330 380.00 购       100       DRVT        CALL    HK.00700  2020-03-30         380.0       False  80031978               N/A
    
    [19 rows x 11 columns]
    HK.TCH200330C250000
    ['HK.TCH200330C250000', 'HK.TCH200330C255000', 'HK.TCH200330C260000', 'HK.TCH200330C265000', 'HK.TCH200330C270000', 'HK.TCH200330C275000', 
    ..                  ...                 ...       ...        ...         ...         ...         ...           ...
     'HK.TCH200330C380000']
   
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
    
    ret, data = quote_ctx.get_history_kl_quota(get_detail=True)  # 设置True代表需要返回详细的拉取历史K线的记录
    if ret == RET_OK:
        print(data)
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

    (1, 99, [{'code': 'HK.00700', 'request_time': '2020-03-27 19:15:57'}])

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
    
    ret, data = quote_ctx.get_rehab("HK.00700")
    if ret == RET_OK:
        print(data)
        print(data['ex_div_date'][0])    # 取第一条的除权除息日
        print(data['ex_div_date'].values.tolist())   # 转为list
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

        ex_div_date  split_ratio  per_cash_div  per_share_div_ratio  per_share_trans_ratio  allotment_ratio  allotment_price  stk_spo_ratio  stk_spo_price  forward_adj_factorA  forward_adj_factorB  backward_adj_factorA  backward_adj_factorB
    0   2005-04-19          NaN          0.07                  NaN                    NaN              NaN              NaN            NaN            NaN                  1.0                -0.07                   1.0                  0.07
    ..         ...          ...           ...                  ...                    ...              ...              ...            ...            ...                  ...                  ...                   ...                   ...
    15  2019-05-17          NaN          1.00                  NaN                    NaN              NaN              NaN            NaN            NaN                  1.0                -1.00                   1.0                  1.00
    
    [16 rows x 13 columns]
    2005-04-19
    ['2005-04-19', '2006-05-15', '2007-05-09', '2008-05-06', '2009-05-06', '2010-05-05', '2011-05-03', '2012-05-18', '2013-05-20', '2014-05-15', '2014-05-16', '2015-05-15', '2016-05-20', '2017-05-19', '2018-05-18', '2019-05-17']

.. note::

	* 接口限制请参见 :ref:`在线获取单只股票复权信息限制 <get-rehab-limit>`
	
	* 复权后价格 = 复权前价格 * 复权因子a + 复权因子b

get_warrant
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_warrant(self, stock_owner='', req=None)

 通过标的股查询窝轮

 :param stock_owner: 所属正股的股票代码,例如：'HK.00700'，会去找腾讯的窝轮，注意有些股票没有对应窝轮牛熊。
 :param req: 请求参数组合


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
cur_price_min               float             最新价的过滤下限（闭区间），不传代表下限为-∞
cur_price_max               float             最新价的过滤上限（闭区间），不传代表上限为+∞
strike_price_min            float             行使价的过滤下限（闭区间），不传代表下限为-∞
strike_price_max            float             行使价的过滤上限（闭区间），不传代表上限为+∞
street_min                  float             街货占比的过滤下限（闭区间），该字段为百分比字段，默认不展示%，如20实际对应20%。不传代表下限为-∞
street_max                  float             街货占比的过滤上限（闭区间），该字段为百分比字段，默认不展示%，如20实际对应20%。不传代表上限为+∞
conversion_min              float             换股比率的过滤下限（闭区间），不传代表下限为-∞
conversion_max              float             换股比率的过滤上限（闭区间），不传代表上限为+∞
vol_min                     unsigned int      成交量的过滤下限（闭区间），不传代表下限为-∞
vol_max                     unsigned int      成交量的过滤上限（闭区间），不传代表上限为+∞
premium_min                 float             溢价的过滤下限（闭区间），该字段为百分比字段，默认不展示%，如20实际对应20%。不传代表下限为-∞
premium_max                 float             溢价的过滤上限（闭区间），该字段为百分比字段，默认不展示%，如20实际对应20%。不传代表上限为+∞
leverage_ratio_min          float             杠杆比率的过滤下限（闭区间），不传代表下限为-∞
leverage_ratio_max          float             杠杆比率的过滤上限（闭区间），不传代表上限为+∞
delta_min                   float             对冲值的过滤下限（闭区间）, 仅认购认沽支持该字段过滤，不传代表下限为-∞
delta_max                   float             对冲值的过滤上限（闭区间）, 仅认购认沽支持该字段过滤，不传代表上限为+∞
implied_min                 float             引伸波幅的过滤下限（闭区间）, 仅认购认沽支持该字段过滤，不传代表下限为-∞
implied_max                 float             引伸波幅的过滤上限（闭区间）, 仅认购认沽支持该字段过滤，不传代表上限为+∞
recovery_price_min          float             收回价的过滤下限（闭区间）, 仅牛熊证支持该字段过滤，不传代表下限为-∞
recovery_price_max          float             收回价的过滤上限（闭区间）, 仅牛熊证支持该字段过滤，不传代表上限为+∞
price_recovery_ratio_min    float             正股距收回价, 的过滤下限（闭区间）, 仅牛熊证支持该字段过滤。该字段为百分比字段，默认不展示%，如20实际对应20%。不传代表下限为-∞
price_recovery_ratio_max    float             正股距收回价, 的过滤上限（闭区间）, 仅牛熊证支持该字段过滤。该字段为百分比字段，默认不展示%，如20实际对应20%。不传代表上限为+∞
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
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    
    req = WarrantRequest()
    req.sort_field = SortField.TURNOVER
    req.type_list = WrtType.CALL
    req.cur_price_min = 0.2
    req.cur_price_max = 0.201
    ret, ls = quote_ctx.get_warrant("HK.00700", req)
    if ret == RET_OK:  # 所有接口先判断是否正常，再取数据
        warrant_data_list, last_page, all_count = ls
        print(len(warrant_data_list), all_count, warrant_data_list)
        print(warrant_data_list['stock'][0])    # 取第一条的窝轮代码
        print(warrant_data_list['stock'].values.tolist())   # 转为list
    else:
        print('error: ', ls)
    quote_ctx.close()  # 所有接口结尾加上这条close，防止连接条数用尽
	
 :Output:

 .. code:: python

    2 2 
       stock        name stock_owner  type issuer maturity_time   list_time last_trade_time  recovery_price  conversion_ratio  lot_size  strike_price  last_close_price  cur_price  price_change_val  change_rate  status  bid_price  ask_price   bid_vol  ask_vol    volume   turnover   score  premium  break_even_point  leverage    ipop  price_recovery_ratio  conversion_price  street_rate  street_vol  amplitude  issue_size  high_price  low_price  implied_volatility  delta  effective_leverage  list_timestamp  last_trade_timestamp  maturity_timestamp  upper_strike_price  lower_strike_price  inline_price_status
    0  HK.19692  腾讯法巴零八购B.C    HK.00700  CALL     BP    2020-08-04  2020-01-15      2020-07-29             NaN             100.0     10000        430.50             0.234      0.201            -0.033   -14.102564  NORMAL      0.198      0.201   5000000  5000000   2360000   496360.0  77.402   18.019            450.60    18.995 -11.312                   NaN              20.1         0.33      264000      2.137    80000000       0.214      0.209              41.154  0.358               6.800    1.579018e+09          1.595952e+09        1.596470e+09                 NaN                 NaN                  NaN
    1  HK.18854  腾讯瑞信零七购A.C    HK.00700  CALL     CS    2020-07-09  2020-01-08      2020-07-03             NaN             100.0     10000        401.28             0.212      0.200            -0.012    -5.660377  NORMAL      0.199      0.200  10000000   100000  12200000  2581320.0  72.970   10.340            421.28    19.090  -4.854                   NaN              20.0         4.98    12450000     17.453   250000000       0.220      0.183              34.305  0.429               8.189    1.578413e+09          1.593706e+09        1.594224e+09                 NaN                 NaN                  NaN
    HK.19692
    ['HK.19692', 'HK.18854']

.. note::

	* 接口限制请参见 :ref:`获取窝轮限制 <get-warrant-limit>`

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
    
    ret, data = quote_ctx.get_capital_flow("HK.00700")
    if ret == RET_OK:
        print(data)
        print(data['in_flow'][0])    # 取第一条的净流入的资金额度
        print(data['in_flow'].values.tolist())   # 转为list
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

         last_valid_time       in_flow capital_flow_item_time
    0    2020-03-26 16:00:00 -3.162472e+07    2020-03-26 09:30:00
    ..                   ...           ...                    ...
    330  2020-03-26 16:00:00 -1.433742e+09    2020-03-26 16:00:00
    
    [331 rows x 3 columns]
    -31624720.0
    [-31624720.0, -64132100.0, -70575660.0, -65573840.0, -43585860.0, -80639800.0, -80546540.0, -107078020.0, -117504440.0, -111991620.0, -118827340.0, 
    ..                   ...           ...                    ...
    -1419125560.0, -1415245360.0, -1437747220.0, -1433741860.0, -1433741860.0]

.. note::

	* 接口限制请参见 :ref:`获取资金流向限制 <get-capital-flow-limit>`


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
    
    ret, data = quote_ctx.get_capital_distribution("HK.00700")
    if ret == RET_OK:
        print(data)
        print(data['capital_in_big'][0])    # 取第一条的流入资金额度，大单
        print(data['capital_in_big'].values.tolist())   # 转为list
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

          capital_in_big  capital_in_mid  capital_in_small  capital_out_big  capital_out_mid  capital_out_small          update_time
    0     227797260.0     438703020.0       114061560.0      209093300.0      578002980.0        141638900.0  2020-03-27 09:47:36
    227797260.0
    [227797260.0]

.. note::

	* 接口限制请参见 :ref:`获取资金分布限制 <get-capital-distribution-limit>`


get_user_security
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_user_security(self, group_name)

 获取指定分组的自选股列表

 :param group_name: 需要查询的自选股分组名称。
 
        系统分组的中英文对应名称如下：
 
        =================   ===========   
        中文                英文     
        =================   ===========   
        全部                All
		沪深				CN
		港股				HK
		美股				US
		期权				Options
		港股期权			HK options
		美股期权			US options
		特别关注			Starred
		期货				Futures
        =================   ===========  
		
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
    
    ret, data = quote_ctx.get_user_security("A")
    if ret == RET_OK:
        print(data)
        print(data['code'][0])    # 取第一条的股票代码
        print(data['code'].values.tolist())   # 转为list
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

       code    name  lot_size stock_type stock_child_type stock_owner option_type strike_time strike_price suspension listing_date        stock_id  delisting  main_contract last_trade_time
    0  HK.HSImain  恒指期货主连        50     FUTURE              N/A                                              N/A        N/A                     71000662      False           True                
    1  HK.00700    腾讯控股       100      STOCK              N/A                                              N/A        N/A   2004-06-16  54047868453564      False          False                
    HK.HSImain
    ['HK.HSImain', 'HK.00700']

.. note::

	* 接口限制请参见 :ref:`获取指定分组的自选股列表 <get-user-security-limit>`

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
    
    ret, data = quote_ctx.modify_user_security("A", ModifyUserSecurityOp.ADD, ['HK.00700'])
    if ret == RET_OK:
        print(data) # 返回success
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

    success

.. note::

	* 接口限制请参见 :ref:`修改指定分组的自选股列表 <modify-user-security-limit>`

get_stock_filter
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_stock_filter(self, market, filter_list, plate_code=None, begin=0, num=200)

 获取条件选股

 :param market: 市场标识，注意这里不区分沪和深，输入沪或者深都会返回沪深市场的股票（这个是和客户端保持一致的）参见 Market_
 :param filter_list: 
        | 筛选条件的枚举值，筛选条件是SimpleFilter，AccumulateFilter，FinancialFilter类型数据的list对象field。
        
        | SimpleFilter对象field的相关参数如下：
        
        ============================================   ===========   ================================================
        参数                                            类型           说明
        ============================================   ===========   ================================================
        stock_field                                    str            StockField 简单属性，取值见 `StockField <Base_API.html#stockfield-simplefilter>`_ 
        filter_min                                     float          区间下限（闭区间），不传代表下限为-∞
        filter_max                                     float          区间上限（闭区间），不传代表上限为+∞
        is_no_filter                                   bool           该字段是否不需要筛选，True代表不筛选，False代表筛选。不传默认为不筛选
        sort                                           str            SortDir 排序方向，取值见 SortDir_，不传默认为不排序 
        ============================================   ===========   ================================================
        
        | AccumulateFilter对象field的相关参数如下：
        
        ============================================   ===========   ================================================
        参数                                            类型           说明
        ============================================   ===========   ================================================
        stock_field                                    str            StockField 累积字段，取值见 `StockField <Base_API.html#stockfield-accumulatefilter>`_ 
        filter_min                                     float          区间下限（闭区间），不传代表下限为-∞
        filter_max                                     float          区间上限（闭区间），不传代表上限为+∞
        is_no_filter                                   bool           该字段是否不需要筛选，True代表不筛选，False代表筛选。不传默认为不筛选
        sort                                           str            SortDir 排序方向，取值见 SortDir_，不传默认为不排序
        days                                           int            所筛选的数据的累计天数
        ============================================   ===========   ================================================
        
        | FinancialFilter对象field的相关参数如下：
        
        ============================================   ===========   ================================================
        参数                                            类型           说明
        ============================================   ===========   ================================================
        stock_field                                    str            StockField 财务字段，取值见 `StockField <Base_API.html#stockfield-financialfilter>`_ 
        filter_min                                     float          区间下限（闭区间），不传代表下限为-∞
        filter_max                                     float          区间上限（闭区间），不传代表上限为+∞
        is_no_filter                                   bool           该字段是否不需要筛选，True代表不筛选，False代表筛选。不传默认为不筛选
        sort                                           str            SortDir 排序方向，取值见 SortDir_，不传默认为不排序
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
        | **stock_list** - 返回的是SimpleFilter类型数据的list对象ret_list，对象ret_list中stock_code和stock_name默认都会返回，同时filter_list中设置了筛选的字段也会返回。返回的数据列字段如下:

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
ps_ttm                                         float          市销率(TTM) 例如填写 [100, 500] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
pcf_ttm                                        float          市现率(TTM) 例如填写 [100, 1000] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
total_share                                    float          总股数 例如填写 [1000000000,1000000000] 值区间 (单位：股)
float_share                                    float          流通股数 例如填写 [1000000000,1000000000] 值区间 (单位：股)
float_market_val                               float          流通市值 例如填写 [1000000000,1000000000] 值区间 (单位：元)
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
roic                                           float          盈利能力属性投入资本回报率 例如填写 [1.0,10.0] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
roa_ttm                                        float          资产回报率(TTM) 例如填写 [1.0,10.0] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%。仅适用于年报。）
ebit_ttm                                       float          息税前利润(TTM) 例如填写 [1000000000,1000000000] 值区间（单位：元。仅适用于年报。）
ebitda                                         float          税息折旧及摊销前利润 例如填写 [1000000000,1000000000] 值区间（单位：元）
operating_margin_ttm                           float          营业利润率(TTM) 例如填写 [1.0,10.0] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%。仅适用于年报。）
ebit_margin                                    float          EBIT利润率 例如填写 [1.0,10.0] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
ebitda_margin                                  float          EBITDA利润率 例如填写 [1.0,10.0] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
financial_cost_rate                            float          财务成本率 例如填写 [1.0,10.0] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
operating_profit_ttm                           float          营业利润(TTM) 例如填写 [1000000000,1000000000] 值区间 （单位：元。仅适用于年报。）
shareholder_net_profit_ttm                     float          归属于母公司的净利润 例如填写 [1000000000,1000000000] 值区间 （单位：元。仅适用于年报。）
net_profit_cash_cover_ttm                      float          盈利中的现金收入比例 例如填写 [1.0,60.0] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%。仅适用于年报。）
current_ratio                                  float          偿债能力属性流动比率 例如填写 [100,250] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
quick_ratio                                    float          速动比率 例如填写 [100,250] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
current_asset_ratio                            float          清债能力属性流动资产率 例如填写 [10,100] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
current_debt_ratio                             float          流动负债率 例如填写 [10,100] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
equity_multiplier                              float          权益乘数 例如填写 [100,180] 值区间
property_ratio                                 float          产权比率 例如填写 [50,100] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
cash_and_cash_equivalents                      float          现金和现金等价 例如填写 [1000000000,1000000000] 值区间（单位：元）
total_asset_turnover                           float          运营能力属性总资产周转率 例如填写 [50,100] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
fixed_asset_turnover                           float          固定资产周转率 例如填写 [50,100] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
inventory_turnover                             float          存货周转率 例如填写 [50,100] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
operating_cash_flow_ttm                        float          经营活动现金流(TTM) 例如填写 [1000000000,1000000000] 值区间（单位：元。仅适用于年报。）
accounts_receivable                            float          应收账款净额 例如填写 [1000000000,1000000000] 值区间 例如填写 [1000000000,1000000000] 值区间 （单位：元）
ebit_growth_rate                               float          成长能力属性EBIT同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
operating_profit_growth_rate                   float          营业利润同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
total_assets_growth_rate                       float          总资产同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
profit_to_shareholders_growth_rate             float          归母净利润同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
profit_before_tax_growth_rate                  float          总利润同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
eps_growth_rate                                float          EPS同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
roe_growth_rate                                float          ROE同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
roic_growth_rate                               float          ROIC同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
nocf_growth_rate                               float          经营现金流同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
nocf_per_share_growth_rate                     float          每股经营现金流同比增长率 例如填写 [1.0,10.0] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
operating_revenue_cash_cover                   float          现金流属性经营现金收入比 例如填写 [10,100] 值区间（该字段为百分比字段，默认省略%，如20实际对应20%）
operating_profit_to_total_profit               float          营业利润占比 例如填写 [10,100] 值区间 （该字段为百分比字段，默认省略%，如20实际对应20%）
basic_eps                                      float          市场表现属性基本每股收益 例如填写 [0.1,10] 值区间 (单位：元)
diluted_eps                                    float          稀释每股收益 例如填写 [0.1,10] 值区间 (单位：元)
nocf_per_share                                 float          每股经营现金净流量 例如填写 [0.1,10] 值区间 (单位：元)
\
============================================   ===========   ==============================================================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    simple_filter = SimpleFilter()
    simple_filter.filter_min = 200
    simple_filter.filter_max = 1000
    simple_filter.stock_field = StockField.CUR_PRICE
    simple_filter.is_no_filter = False
    simple_filter.sort = SortDir.ASCEND
    ret, ls = quote_ctx.get_stock_filter(Market.HK, [simple_filter])  # 对香港市场的股票做简单筛选
    if ret == RET_OK:
        last_page, all_count, ret_list = ls
        print(len(ret_list), all_count, ret_list)
        for item in ret_list:
            print(item.stock_code)  # 取其中的股票代码
    else:
        print('error: ', ls)
    quote_ctx.close()  # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

    2 2 [ stock_code:HK.00388  stock_name:香港交易所  cur_price:231.8 ,  stock_code:HK.00700  stock_name:腾讯控股  cur_price:381.8 ]
    HK.00388
    HK.00700

.. note::

	*   接口限制请参见 :ref:`获取条件选股 <get-stock-filter-limit>`
	
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

 :param market: 市场标识，注意这里不区分沪和深，输入沪或者深都会返回沪深市场的股票（这个是和客户端保持一致的）。输入Market.HK_FUTURE效果同Market.HK，返回港股市场所有数据。参见 Market_

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
    
    ret, data = quote_ctx.get_ipo_list(Market.HK)
    if ret == RET_OK:
        print(data)
        print(data['code'][0])    # 取第一条的股票代码
        print(data['code'].values.tolist())   # 转为list
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

       code      name   list_time  list_timestamp apply_code issue_size online_issue_size apply_upper_limit apply_limit_market_value is_estimate_ipo_price ipo_price industry_pe_rate is_estimate_winning_ratio winning_ratio issue_pe_rate apply_time apply_timestamp winning_time winning_timestamp is_has_won winning_num_data  ipo_price_min  ipo_price_max  list_price  lot_size  entrance_price  is_subscribe_status apply_end_time  apply_end_timestamp
    0  HK.01957  MBV INTL  2020-03-30    1.585498e+09        N/A        N/A               N/A               N/A                      N/A                   N/A       N/A              N/A                       N/A           N/A           N/A        N/A             N/A          N/A               N/A        N/A              N/A            0.8           0.88         0.0      2500         2222.17                False     2020-03-19         1.584580e+09
    HK.01957
    ['HK.01957']

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
    
    ret, data = quote_ctx.get_future_info(["HK.MPImain", "HK.HAImain"])
    if ret == RET_OK:
        print(data)
        print(data['code'][0])    # 取第一条的股票代码
        print(data['code'].values.tolist())   # 转为list
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

       code      name       owner exchange  type     size size_unit price_currency price_unit  min_change min_change_unit                        trade_time time_zone last_trade_time                                exchange_format_url
    0  HK.MPImain    內房期货主连  恒生中国内地地产指数      港交所  股指期货     50.0    指数点×港元             港元        指数点        0.50             指数点  (09:15 - 12:00), (13:00 - 16:30)       CCT                  https://sc.hkex.com.hk/TuniS/www.hkex.com.hk/P...
    1  HK.HAImain  海通证券期货主连    HK.06837      港交所  股票期货  10000.0         股             港元      每股/港元        0.01              港元  (09:30 - 12:00), (13:00 - 16:00)       CCT                  https://sc.hkex.com.hk/TuniS/www.hkex.com.hk/P...
    HK.MPImain
    ['HK.MPImain', 'HK.HAImain']

.. note::

	* 接口限制请参见 :ref:`获取期货合约资料限制 <get-future-info-limit>`


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
    
    ret, data = quote_ctx.request_trading_days(TradeDateMarket.HK, start='2020-04-01', end='2020-04-10')
    if ret == RET_OK:
        print(data)
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

    [{'time': '2020-04-01', 'trade_date_type': 'WHOLE'}, {'time': '2020-04-02', 'trade_date_type': 'WHOLE'}, {'time': '2020-04-03', 'trade_date_type': 'WHOLE'}, {'time': '2020-04-06', 'trade_date_type': 'WHOLE'}, {'time': '2020-04-07', 'trade_date_type': 'WHOLE'}, {'time': '2020-04-08', 'trade_date_type': 'WHOLE'}, {'time': '2020-04-09', 'trade_date_type': 'WHOLE'}]

.. note::

	* 接口限制请参见 :ref:`在线获取交易日 <request-trading-day-limit>`
	
-----------------------------------------------------------------------------------------------------    

set_price_reminder
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: set_price_reminder(self, code, op, key=None, reminder_type=None, reminder_freq=None, value=None, note=None)

 新增、删除、修改、启用、禁用 某只股票的到价提醒，每只股票每种类型最多可设置10个提醒
 
 :param code: 股票
 :param op: SetPriceReminderOp_，操作类型
 :param key: int64，标识，新增的情况不需要填
 :param reminder_type: PriceReminderType_，到价提醒的频率，删除、启用、禁用的情况下会忽略该入参
 :param reminder_freq: PriceReminderFreq_，到价提醒的频率，删除、启用、禁用的情况下会忽略该入参
 :param value: float，提醒值，删除、启用、禁用的情况下会忽略该入参
 :param note: str，用户设置的备注，最多10个字符，删除、启用、禁用的情况下会忽略该入参
 :return: (ret, data)
    ret != RET_OK 返回错误字符串
	
    ret == RET_OK data为key
..

 :Example:
 .. code:: python

    from futu import *
    import time
    class PriceReminderTest(PriceReminderHandlerBase):
        def on_recv_rsp(self, rsp_str):
            ret_code, content = super(PriceReminderTest,self).on_recv_rsp(rsp_str)
            if ret_code != RET_OK:
                print("PriceReminderTest: error, msg: %s" % content)
                return RET_ERROR, content
            print("PriceReminderTest ", content) # PriceReminderTest自己的处理逻辑
            return RET_OK, content
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    handler = PriceReminderTest()
    quote_ctx.set_handler(handler)
    ret, data = quote_ctx.get_market_snapshot(['HK.HSImain'])
    if ret == RET_OK:
        bid_price = data['bid_price'][0]  # 获取实时买一价
        ask_price = data['ask_price'][0]  # 获取实时卖一价
        # 设置当卖一价低于（ask_price-1）时提醒
        ret_ask, ask_data = quote_ctx.set_price_reminder(code='HK.HSImain', op=SetPriceReminderOp.ADD, key=None, reminder_type=PriceReminderType.ASK_PRICE_DOWN, reminder_freq=PriceReminderFreq.ALWAYS, value=(ask_price-1), note='123')
        if ret_ask == RET_OK:
            print('卖一价低于（ask_price-1）时提醒设置成功：', ask_data)
        else:
            print('error:', ask_data)
        # 设置当买一价高于（bid_price+1）时提醒
        ret_bid, bid_data = quote_ctx.set_price_reminder(code='HK.HSImain', op=SetPriceReminderOp.ADD, key=None, reminder_type=PriceReminderType.BID_PRICE_UP, reminder_freq=PriceReminderFreq.ALWAYS, value=(bid_price+1), note='456')
        if ret_bid == RET_OK:
            print('买一价高于（bid_price+1）时提醒设置成功：', bid_data)
        else:
            print('error:', bid_data)
    time.sleep(15)
    quote_ctx.close()
	
 :Output:

 .. code:: python

    卖一价低于（ask_price-1）时提醒设置成功： 158815356110052101
    买一价高于（bid_price+1）时提醒设置成功： 158815356129980801
    PriceReminderTest  {'code': 'HK.HSImain', 'price': 24532.0, 'change_rate': 0.122, 'market_status': 'OPEN', 'content': '买一价高于24533.000', 'note': '456', 'key': 158815356129980801, 'reminder_type': 'BID_PRICE_UP', 'set_value': 24533.0, 'cur_value': 24533.0}
    PriceReminderTest  {'code': 'HK.HSImain', 'price': 24532.0, 'change_rate': 0.122, 'market_status': 'OPEN', 'content': '卖一价低于24533.000', 'note': '123', 'key': 158815356110052101, 'reminder_type': 'ASK_PRICE_DOWN', 'set_value': 24533.0, 'cur_value': 24533.0}

.. note::

    * 接口限制请参见 :ref:`设置到价提醒 <set-price-reminder-limit>`
    
    * API 中成交量设置统一以股为单位。但是牛牛客户端中，A 股是以为手为单位展示
    
    * 到价提醒类型，存在最小精度，如下：

        TURNOVER_UP：成交额最小精度为 10 元（人民币元，港元，美元）。传入的数值会自动向下取整到最小精度的整数倍。如果设置【00700成交额102元提醒】，设置后会得到【00700成交额100元提醒】；如果设置【00700 成交额 8 元提醒】，设置后会得到【00700 成交额 0 元提醒】。

        VOLUME_UP：A 股成交量最小精度为 1000 股，其他市场股票成交量最小精度为 10 股。传入的数值会自动向下取整到最小精度的整数倍。

        BID_VOL_UP、ASK_VOL_UP：A 股的买一卖一量最小精度为 100 股。传入的数值会自动向下取整到最小精度的整数倍。

        其余到价提醒类型精度支持到小数点后 3 位
		
-----------------------------------------------------------------------------------------------------

get_price_reminder
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_price_reminder(self, code = None, market = None)

  获取对某只股票(某个市场)设置的到价提醒列表

 :param code: 获取该股票的到价提醒，code和market二选一，都存在的情况下code优先。
 :param market: 获取该市场的到价提醒，注意输入沪深都会认为是A股市场。输入Market.HK_FUTURE效果同Market.HK，返回港股市场所有数据。参见 Market_
 :return: (ret, data)
    ret != RET_OK 返回错误字符串

    ret == RET_OK data为DataFrame类型，字段如下:
    
    =========================   ==================   ========================================
    参数                         类型                 说明
    =========================   ==================   ========================================
    code                        str                  股票代码
    key                         int64                标识，用于修改到价提醒
    reminder_type               str                  到价提醒的类型，参见 PriceReminderType_
    reminder_freq               str                  到价提醒的频率，参见 PriceReminderFreq_
    value                       float                提醒值
    enable                      bool                 是否启用
    note                        string               备注，最多10个字符
    =========================   ==================   ========================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    
    ret, data = quote_ctx.get_price_reminder(code='HK.00700')
    if ret == RET_OK:
        print(data)
        print(data['key'].values.tolist())   # 转为list
    else:
        print('error:', data)
    print('******************************************')
    ret, data = quote_ctx.get_price_reminder(code=None, market=Market.HK)
    if ret == RET_OK:
        print(data)
        print(data['code'][0])    # 取第一条的股票代码
        print(data['code'].values.tolist())   # 转为list
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽
	
 :Output:

 .. code:: python

        code                key       reminder_type  reminder_freq  value  enable      note
    0   HK.00700  158441340870842901      PRICE_UP        ALWAYS    382.4   False   10:50:04
    1   HK.00700  158573111208632201    PRICE_DOWN        ALWAYS    370.0    True         
    
    [6 rows x 7 columns]
    [158441340870842901, 158573111208632201]
    ******************************************
        code              key          reminder_type  reminder_freq   value  enable      note
    0   HK.00700   158441340870842901      PRICE_UP        ALWAYS    382.4   False    10:50:04
    1   HK.00700   158573111208632201    PRICE_DOWN        ALWAYS    370.0    True
    2   HK.HSImain  158573100205367801    PRICE_DOWN        ALWAYS  22900.0    True         
    
    [12 rows x 7 columns]
    HK.00700
    ['HK.00700', 'HK.00700', 'HK.HSImain']

.. note::

    * 接口限制请参见 :ref:`获取到价提醒列表 <get-price-reminder-limit>`

-----------------------------------------------------------------------------------------------------

get_user_security_group
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  py:function:: get_user_security_group(self, group_type = UserSecurityGroupType.ALL)
     
  获取自选股分组列表
  
 :param group_type: UserSecurityGroupType_，分组类型
 :return: (ret, data) ret != RET_OK 返回错误字符串
  
    ret == RET_OK data为DataFrame类型，字段如下:
  
  =========================   ==================   ================================
  参数                         类型                 说明
  =========================   ==================   ================================
  group_name                   str                  分组名
  group_type                   str                  UserSecurityGroupType，分组类型
  =========================   ==================   ================================

 :Example:

 .. code:: python

    from futu import *
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
  
    ret, data = quote_ctx.get_user_security_group(group_type = UserSecurityGroupType.ALL)
    if ret == RET_OK:
        print(data)
    else:
        print('error:', data)
    quote_ctx.close() # 结束后记得关闭当条连接，防止连接条数用尽

 :Output:

 .. code:: python

               group_name group_type
    0          期权     SYSTEM
    ..         ...        ...
    12          C     CUSTOM
    
    [13 rows x 2 columns]

.. note::

    * 接口限制请参见 :ref:`获取自选股分组列表 <get-user-security-group-limit>`

-----------------------------------------------------------------------------------------------------

SysNotifyHandlerBase - OpenD通知回调
-----------------------------------------------------------------------------------------------------    

通知OpenD一些重要消息，类似连接断开等。

 :Example:

 .. code:: python

    import time
    from futu import *
    
    class SysNotifyTest(SysNotifyHandlerBase):
        def on_recv_rsp(self, rsp_str):
            ret_code, data = super(SysNotifyTest, self).on_recv_rsp(rsp_str)
            notify_type, sub_type, msg = data
            if ret_code != RET_OK:
                logger.debug("SysNotifyTest: error, msg: {}".format(msg))
                return RET_ERROR, data
            print(msg)
            return RET_OK, data
    
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    handler = SysNotifyTest()
    quote_ctx.set_handler(handler)  # 设置回调
    time.sleep(15)  # 设置脚本接收OpenD的推送持续时间为15秒
    quote_ctx.close()  # 关闭当条连接，OpenD会在1分钟后自动取消相应股票相应类型的订阅
                
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

--------------------------------------------------------------------------------------------------------------------

StockQuoteHandlerBase - 实时报价回调
-------------------------------------------

异步处理推送的订阅股票的报价。

 :Example:

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
    quote_ctx.set_handler(handler)  # 设置实时报价回调
    quote_ctx.subscribe(['HK.00700'], [SubType.QUOTE])  # 订阅实时报价类型，OpenD开始持续收到服务器的推送
    time.sleep(15)  #  设置脚本接收OpenD的推送持续时间为15秒
    quote_ctx.close()   # 关闭当条连接，OpenD会在1分钟后自动取消相应股票相应类型的订阅	
	
 :Output:

 .. code:: python

       StockQuoteTest         code   data_date data_time  last_price  open_price  high_price  low_price  prev_close_price    volume      turnover  turnover_rate  amplitude  suspension listing_date  price_spread dark_status sec_status strike_price contract_size open_interest implied_volatility premium delta gamma vega theta  rho net_open_interest expiry_date_distance contract_nominal_value owner_lot_multiplier option_area_type contract_multiplier last_settle_price position position_change pre_price pre_high_price pre_low_price pre_volume pre_turnover pre_change_val pre_change_rate pre_amplitude after_price after_high_price after_low_price after_volume after_turnover after_change_val after_change_rate after_amplitude
    0  HK.00700  2020-03-27  14:43:35       384.0       390.0       390.0      381.8             381.8  21573862  8.298364e+09          0.226      2.148       False   2004-06-16           0.2         N/A     NORMAL          N/A           N/A           N/A                N/A     N/A   N/A   N/A  N/A   N/A  N/A               N/A                  N/A                    N/A                  N/A              N/A                 N/A               N/A      N/A             N/A       N/A            N/A           N/A        N/A          N/A            N/A             N/A           N/A         N/A              N/A             N/A          N/A            N/A              N/A               N/A             N/A
                
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
	
 :Example:

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
    quote_ctx.set_handler(handler)  # 设置实时摆盘回调
    quote_ctx.subscribe(['HK.00700'], [SubType.ORDER_BOOK])  # 订阅买卖摆盘类型，OpenD开始持续收到服务器的推送
    time.sleep(15)  #  设置脚本接收OpenD的推送持续时间为15秒
    quote_ctx.close()  # 关闭当条连接，OpenD会在1分钟后自动取消相应股票相应类型的订阅
	
 :Output:

 .. code:: python

    OrderBookTest  {'code': 'HK.00700', 'svr_recv_time_bid': '2020-04-29 15:55:53.299', 'svr_recv_time_ask': '2020-04-29 15:55:53.299', 'Bid': [(414.2, 60600, 63, {}), (414.0, 96000, 34, {}), (413.8, 23400, 17, {}), (413.6, 31800, 24, {}), (413.4, 33900, 19, {}), (413.2, 47800, 17, {}), (413.0, 42500, 44, {}), (412.8, 17000, 6, {}), (412.6, 10600, 6, {}), (412.4, 5200, 5, {})], 'Ask': [(414.4, 74200, 61, {}), (414.6, 25700, 26, {}), (414.8, 18800, 24, {}), (415.0, 35700, 51, {}), (415.2, 17500, 15, {}), (415.4, 33500, 9, {}), (415.6, 62000, 18, {}), (415.8, 36700, 11, {}), (416.0, 42000, 73, {}), (416.2, 3800, 10, {})]}
   
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
	
 :Example:

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
    quote_ctx.set_handler(handler)  # 设置实时摆盘回调
    quote_ctx.subscribe(['HK.00700'], [SubType.K_1M])   # 订阅k线数据类型，OpenD开始持续收到服务器的推送
    time.sleep(15)  # 设置脚本接收OpenD的推送持续时间为15秒
    quote_ctx.close()   # 关闭当条连接，OpenD会在1分钟后自动取消相应股票相应类型的订阅		
	
 :Output:

 .. code:: python

       CurKlineTest         code             time_key   open  close   high    low  volume    turnover k_type  last_close
                      0  HK.00700  2020-04-01 15:55:00  375.2  375.4  375.4  375.2   39000  14639140.0   K_1M         0.0
       CurKlineTest         code             time_key   open  close   high    low  volume  turnover k_type  last_close
                      0  HK.00700  2020-04-01 15:56:00  375.4  375.4  375.4  375.4    1000  375400.0   K_1M       375.4
       CurKlineTest         code             time_key   open  close   high    low  volume  turnover k_type  last_close
                      0  HK.00700  2020-04-01 15:56:00  375.4  375.4  375.4  375.4    1100  412940.0   K_1M       375.4

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
	
 :Example:

 .. code:: python

	import time
	from futu import *
	
	class TickerTest(TickerHandlerBase):
	    def on_recv_rsp(self, rsp_str):
	        ret_code, data = super(TickerTest,self).on_recv_rsp(rsp_str)
	        if ret_code != RET_OK:
	            print("TickerTest: error, msg: %s" % data)
	            return RET_ERROR, data
	        print("TickerTest ", data) # TickerTest自己的处理逻辑
	        return RET_OK, data
	quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
	handler = TickerTest()
	quote_ctx.set_handler(handler)  # 设置实时逐笔推送回调
	quote_ctx.subscribe(['HK.00700'], [SubType.TICKER]) # 订阅逐笔类型，OpenD开始持续收到服务器的推送
	time.sleep(15)  # 设置脚本接收OpenD的推送持续时间为15秒
	quote_ctx.close()   # 关闭当条连接，OpenD会在1分钟后自动取消相应股票相应类型的订阅
	
 :Output:

 .. code:: python

	   TickerTest         code                 time  price  volume  turnover ticker_direction             sequence        type push_data_type
	0  HK.00700  2020-03-27 15:15:17  382.4     100   38240.0             SELL  6808782951082376714  AUTO_MATCH          CACHE
	   TickerTest          code                 time   price  volume  turnover ticker_direction             sequence     type push_data_type
	0  HK.00700  2020-03-27 15:15:19  382.6     900   344340.0             BUY  6808782951082378963  AUTO_MATCH       REALTIME
	
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
	
 :Example:

 .. code:: python

	import time
	from futu import *
	
	class RTDataTest(RTDataHandlerBase):
	    def on_recv_rsp(self, rsp_str):
	        ret_code, data = super(RTDataTest, self).on_recv_rsp(rsp_str)
	        if ret_code != RET_OK:
	            print("RTDataTest: error, msg: %s" % data)
	            return RET_ERROR, data
	        print("RTDataTest ", data) # RTDataTest自己的处理逻辑
	        return RET_OK, data
	quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
	handler = RTDataTest()
	quote_ctx.set_handler(handler)  # 设置实时分时推送回调
	quote_ctx.subscribe(['HK.00700'], [SubType.RT_DATA]) # 订阅分时类型，OpenD开始持续收到服务器的推送
	time.sleep(15)  # 设置脚本接收OpenD的推送持续时间为15秒
	quote_ctx.close()   # 关闭当条连接，OpenD会在1分钟后自动取消相应股票相应类型的订阅
	
 :Output:

 .. code:: python

	   RTDataTest         code                 time  is_blank  opened_mins  cur_price  last_close   avg_price     turnover   volume
	0  HK.00700  2020-03-30 16:00:00     False          960      376.6       382.4  374.818939  855764640.0  2272500
	
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
	
 :Example:

 .. code:: python

    import time
    from futu import *
    
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
    quote_ctx.set_handler(handler)  # 设置实时经纪推送回调
    quote_ctx.subscribe(['HK.00700'], [SubType.BROKER]) # 订阅经纪类型，OpenD开始持续收到服务器的推送
    time.sleep(15)  # 设置脚本接收OpenD的推送持续时间为15秒
    quote_ctx.close()   # 关闭当条连接，OpenD会在1分钟后自动取消相应股票相应类型的订阅
	
 :Output:

 .. code:: python

        BrokerTest: stock: HK.00700 data: [        code  bid_broker_id  bid_broker_name  bid_broker_pos order_id order_volume
    0   HK.00700            517  Eclipse Options               1      N/A          N/A
    ..       ...            ...              ...             ...      ...          ...
    23  HK.00700            535       海通国际证券有限公司              14      N/A          N/A
    
    [24 rows x 6 columns],         code  ask_broker_id ask_broker_name  ask_broker_pos order_id order_volume
    0   HK.00700           8577  汇丰证券经纪(亚洲)有限公司               1      N/A          N/A
    ..       ...            ...             ...             ...      ...          ...
    37  HK.00700           5357     星展唯高达香港有限公司               3      N/A          N/A
    
    [38 rows x 6 columns]] 
	
-------------------------------------------

on_recv_rsp
~~~~~~~~~~~

..  py:function:: on_recv_rsp(self, rsp_pb)


 在收到实时经纪数据推送后会回调到该函数，使用者需要在派生类中覆盖此方法

 注意该回调是在独立子线程中

 :param rsp_pb: 派生类中不需要直接处理该参数
 :return: 成功时返回(RET_OK, stock_code, [bid_frame_table, ask_frame_table]), 相关frame table含义见 get_broker_queue_ 的返回值说明

          失败时返回(RET_ERROR, ERR_MSG, None)

----------------------------

PriceReminderHandlerBase - 到价提醒通知回调
-------------------------------------------

异步处理推送的到价提醒通知
	
 :Example:

 .. code:: python

    import time
    from futu import *
    
    class PriceReminderTest(PriceReminderHandlerBase):
        def on_recv_rsp(self, rsp_str):
            ret_code, content = super(PriceReminderTest,self).on_recv_rsp(rsp_str)
            if ret_code != RET_OK:
                print("PriceReminderTest: error, msg: %s" % content)
                return RET_ERROR, content
            print("PriceReminderTest ", content) # PriceReminderTest自己的处理逻辑
            return RET_OK, content
    quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)
    handler = PriceReminderTest()
    quote_ctx.set_handler(handler)  # 设置到价提醒通知回调
    time.sleep(15)  # 设置脚本接收OpenD的推送持续时间为15秒
    quote_ctx.close()   # 关闭当条连接，OpenD会在1分钟后自动取消相应股票相应类型的订阅
	
 :Output:

 .. code:: python

    PriceReminderTest  {'code': 'HK.HSImain', 'price': 24529.0, 'change_rate': 0.11, 'market_status': 'OPEN', 'content': '价格涨到24531.000', 'note': '', 'key': 158815186771390101, 'reminder_type': 'PRICE_UP', 'set_value': 24531.0, 'cur_value': 24532.0}
    
-------------------------------------------

on_recv_rsp
~~~~~~~~~~~

..  py:function:: on_recv_rsp(self, rsp_pb)


 在收到后到价提醒通知会回调到该函数，使用者需要在派生类中覆盖此方法

 注意该回调是在独立子线程中

 :param rsp_pb: 派生类中不需要直接处理该参数
 :return: 失败时返回(RET_ERROR, ERR_MSG)
 
 成功时返回(RET_OK, '', data), data为dict类型，字段如下
    
    =========================   =========================   ====================================================================
    参数                         类型                         说明
    =========================   =========================   ====================================================================
    code                        str                          股票代码
    price                       float                        当前价格
    change_rate                 str                          当前涨跌幅
    market_status               str                          触发的时间段，盘前、盘中、盘后，参见 PriceReminderMarketStatus_
    content                     str                          到价提醒文字内容
    note                        str                          备注，最多10个字符
	key                         int64                        到价提醒标识
	reminder_type               str                          到价提醒的类型，参见 PriceReminderType_
	set_value                   float                        用户设置的提醒值
	cur_value                   float                        提醒触发时的值
    =========================   =========================   ====================================================================

          
