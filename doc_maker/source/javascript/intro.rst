.. note::

  Futu OpenAPI 文档已于2020年10月16日全新升级，请移步至 `新文档 <https://openapi.futunn.com/futu-api-doc/>`_ 

  旧的github文档将不再更新，并于2020年11月16日正式停止访问。


.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

=====
介绍
=====

.. _FutuOpenD: ../intro/FutuOpenDGuide.html
.. _intro: ../intro/intro.html
.. _GetGlobalState: ../protocol/base_define.html#getglobalstate-proto-1002

JavaScript WebSocket API简介
-------------
  * JavaScript WebSocket API依赖FutuOpenD网关客户端，需要先运行登录 FutuOpenD_ , 并且在xml里面配置websocket_port选择。

  * Sample提供了几个简单的行情和交易获取demo，可以用于上手学习。

  * 具体支持交易和行情品种参考\ `FutuOpenD网关客户端简介 <../intro/intro.html>`_
  
  * `下载入口 <https://www.futunn.com/download/openAPI>`_

接口框架
-------------
 * 为了保证性能最大，我们的中间层采用C++编写，然后提供JavaScript接口调用层

 .. image:: ../_static/websocket.png

代码结构
-------------

.. code-block:: text

	|-- js
	|   |-- src
	|	|   |-- ft-websocket
	|	|   |   |-- main.js
	|	|   |   |-- base.js
	|	|   |   |-- proto

调用须知
-------------
  * 所有接口以异步回调方式完成
  * 中间层已经发送连接初始化协议，因此JavaScript接口不需重复调用
  * 交易协议中Common.PacketID需要用到连接ID，该信息可以在2.14及以上版本FutuOpenD的 GetGlobalState_ 协议回包中获取

