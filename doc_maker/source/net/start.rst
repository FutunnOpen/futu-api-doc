====
快速上手
====

------------------------------

--------
环境搭建
--------

.. note::

    *   Windows 7/10 ，64位操作系统
    *   官方提供的SDK编译环境为Visual Studio 2013，.NET Framework 4.5
    *   如需更高版本VS环境，可以升级FTAPI4Net.sln

--------
代码快速入门
--------

.. code-block:: cpp

    FTAPI.Init(); //初始化环境
    FTAPI_Qot client = new FTAPI_Qot(); //创建行情对象
    client.SetConnCallback(new SampleConnCallback()); //创建连接回调类
    client.SetQotCallback(new SampleQotCallback()); //创建行情数据回调类
    client.SetClientInfo("FTAPI4NET_Sample", 1); //建立标识
    client.InitConnect("127.0.0.1", 11111, false); //开始连接


  
    




