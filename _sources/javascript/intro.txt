
.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

=====
介绍
=====

.. _FutuOpenD: ../intro/FutuOpenDGuide.html
.. _intro: ../intro/intro.html


JavaScript WebSocket API简介
-------------
  * JavaScript WebSocket API依赖FutuOpenD网关客户端，需要先运行登录 FutuOpenD_ , 并且在xml里面配置websocket_port选择。

  * Sample提供了几个简单的行情和交易获取demo，可以用于上手学习。

  * 具体支持交易和行情品种参考\ `FutuOpenD网关客户端简介 <../intro/intro.html>`_
  
  * 下载地址: 暂时请到FUTU OPEN API的QQ开发群里下载（108534288，229850364）

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

