
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

  * Sample提供了几个简单的行情和交易获取demo，可以用于上手学习。

  * 具体支持交易和行情品种参考\ `FutuOpenD网关客户端简介 <../intro/intro.html>`_

  * `下载入口 <https://www.futunn.com/download/openAPI>`_

接口框架
-------------
 * FTAPIChannel是C接口，供Cpp，Java，C#等语言底层调用，提供连接创建销毁，初始化，加密解密功能，是连接到FutuOpenD的底层通道。
 * FTAPIChannel可用于各种编译选项无需重新编译，该模块提供动态库不提供源码。
 * FTAPI在FTAPIChannel基础上新增了一些业务上的收发接口，提供源码，打包静态库方便使用。
 * Windows下FTAPI静态库使用VS2013 /MT编译，如需使用更高编译平台版本、/MD、动态库等，需自行编译FTAPI以及\ `Google Protobuf <https://github.com/protocolbuffers/protobuf>`_ （3.5.1）。
 * 请保证使用API的工程，API库，Protobuf库编译选项一致。
 * Src/protobuf目录下打包有Google Protobuf（3.5.1）源码，如需编译请参考该目录下"pb库编译说明.txt"。
   
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
