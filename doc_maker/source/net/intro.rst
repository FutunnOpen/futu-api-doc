
.. role:: strike
    :class: strike
.. role:: red-strengthen
    :class: red-strengthen

=====
介绍
=====

.. _FutuOpenD: ../intro/FutuOpenDGuide.html
.. _intro: ../intro/intro.html


.NET API简介
-------------
  * .NET API依赖FutuOpenD网关客户端，需要先运行登录 FutuOpenD_

  * .NET API开源了调用库代码，FTAPI4Net.sln采用VS2013编译，要求平台.NET Framework 4.5。或者FTAPI4NetCore.sln采用VS2017编译，采用NetCore2.1。Windows下使用32位.Net，Linux和Mac使用64位.NET。 用户可以根据需要采用更新的VS版本升级源码后编译目标调用库

  * Sample提供了几个简单的行情和交易获取demo，可以用于上手学习。

  * 具体支持交易和行情品种参考\ `FutuOpenD网关客户端简介 <../intro/intro.html>`_
  
  * 下载地址: 暂时请到FUTU OPEN API的QQ开发群里下载（108534288，229850364）

接口框架
-------------
 * 为了保证性能最大，我们的中间层采用C++编写，然后提供C#接口调用层

 .. image:: ../_static/NETAPI.png

.. note::
   因为涉及到底层Native线程和.NET线程回调的问题，回调时需要特别注意自己的代码所处的线程。建议简单过滤后，把消息抛到上层主线程处理。

代码结构
-------------

.. code-block:: text

	|-- FTAPI4Net
	|   |-- lib 所有依赖的库，包含第3方库和API库。即用户不必自己生成API库。
	|   |-- FTCAPI.cs  Native接口导入类
	|   |-- pb  pb自动生成文件，用于组包解包pb
	|   |-- FTAPI_Trd.cs 交易接口和交易操作函数
	|   |-- FTAPI_Qot.cs 行情接口和交易操作函数
	|   |-- FTAPI.cs 连接层接口
	|    
	`-- Sample 演示demo

调用须知
-------------
  * FTSPI_Conn、FTSPI_Qot、FTSPI_Trd调用和回调会在不同线程，注意线程安全问题。

  * 所有接口以异步回调方式完成

  * FTAPI4Net\lib目录下的FTAPIChannel.dll件记得拷贝到程序运行目录下

