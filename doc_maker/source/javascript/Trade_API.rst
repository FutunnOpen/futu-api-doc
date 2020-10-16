.. note::

  Futu OpenAPI 文档已于2020年10月16日全新升级，请移步至 `新文档 <https://openapi.futunn.com/futu-api-doc/>`_ 

  旧的github文档将不再更新，并于2020年11月16日正式停止访问。


.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

========
交易API
========

--------------

  .. _GetAccList: ../protocol/trade_protocol.html#trd-getacclist-proto-2001
  .. _UnlockTrade: ../protocol/trade_protocol.html#trd-unlocktrade-proto-2005
  .. _SubAccPush: ../protocol/trade_protocol.html#trd-subaccpush-proto-2008
  .. _GetFunds: ../protocol/trade_protocol.html#trd-getfunds-proto-2101
  .. _GetPositionList: ../protocol/trade_protocol.html#trd-getpositionlist-proto-2102
  .. _GetMaxTrdQtys: ../protocol/trade_protocol.html#trd-getmaxtrdqtys-proto-2111
  .. _GetOrderList: ../protocol/trade_protocol.html#trd-getorderlist-proto-2201
  .. _PlaceOrder: ../protocol/trade_protocol.html#trd-placeorder-proto-2202
  .. _ModifyOrder: ../protocol/trade_protocol.html#trd-modifyorder-proto-2205
  .. _GetOrderFillList: ../protocol/trade_protocol.html#trd-getorderfilllist-proto-2211
  .. _GetHistoryOrderList: ../protocol/trade_protocol.html#trd-gethistoryorderlist-proto-2221
  .. _GetHistoryOrderFillList: ../protocol/trade_protocol.html#trd-gethistoryorderfilllist-proto-2222
  .. _UpdateOrder: ../protocol/trade_protocol.html#trd-updateorder-proto-2208
  .. _UpdateOrderFill: ../protocol/trade_protocol.html#trd-updateorderfill-proto-2218
  
---------------------------------------------------



成员函数
---------------------

================================    ==============================================
函数名（点开链接可查看具体协议）        功能简介
================================    ==============================================
GetAccList_                         获取交易账户列表
UnlockTrade_                        解锁
SubAccPush_                         订阅接收推送数据的交易账户
GetFunds_                           获取账户资金
GetPositionList_                    获取账户持仓
GetMaxTrdQtys_                      获取最大交易数量
GetOrderList_                       获取当日订单列表
PlaceOrder_                         下单
ModifyOrder_                        改单
GetOrderFillList_                   获取当日成交列表
GetHistoryOrderList_                获取历史订单列表
GetHistoryOrderFillList_            获取历史成交列表
================================    ==============================================

交易推送接收接口函数
-------------------------------

.. code-block:: js

    this.websocket.onPush = onPush;

    function onPush(cmd, response) {
          const obj = ftWebsocket.findCmdObj(cmd);
          //通过cmd来区分具体的推送协议
          if (obj && obj.description) {
            console.log(obj.description, response);
          }
        },


