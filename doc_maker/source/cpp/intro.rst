
.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

====
介绍
====

.. _FutuOpenD: ../intro/FutuOpenDGuide.html
.. _intro: ../intro/intro.html


Cpp API简介
-------------
  * Cpp API依赖FutuOpenD网关客户端，需要先运行登录 FutuOpenD_
  
  * API使用VS2013编译，如需使用更高编译平台版本，需自行编译FTAPI以及\ `Google Protobuf <https://github.com/protocolbuffers/protobuf>`_ （3.5.1）静态库。

  * Sample提供了几个简单的行情和交易获取demo，可以用于上手学习。

  * 具体支持交易和行情品种参考\ `FutuOpenD网关客户端简介 <../intro/intro.html>`_

  * 下载地址: 暂时请到FUTU OPEN API的QQ开发群里下载（108534288，229850364）

接口框架
-------------
 * FTAPIChannel模块是用于初始化连接，维护连接，该模块提供动态库不提供源码。
 * FTAPI在FTAPIChannel基础上新增了一些业务上的收发接口，提供静态库以及源码。

   
代码结构
-------------

.. code-block:: text

	|-- Bin        API所需的动态静态库，包括API通道动态库，pb静态库，API静态库。
	|-- Include    API头文件
	|-- Src        API源码
	`-- Sample     演示demo
	
调用须知
-------------
  * FTSPI_Conn、FTSPI_Qot、FTSPI_Trd调用和回调会在不同线程，注意线程安全问题。

  * 所有接口以异步回调方式完成