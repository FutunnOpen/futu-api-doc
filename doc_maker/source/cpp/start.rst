﻿=======
快速上手
=======

------------------------------

--------
环境搭建
--------

.. note::

    *   Windows 7/10 ，64位操作系统
    *   官方提供的SDK编译环境为Visual Studio 2013
    *   如需更高版本VS环境，可以升级FTAPI.sln

------------
代码快速入门
------------


.. code-block:: cpp

    InitFTApi(); //初始化环境
    FTAPI_Qot *pQot = new CreateQotApi(); //创建行情对象
    SampleConnCallback *pCallback = new SampleConnCallback();
    pQot->RegisterConnSpi(pCallback); //创建连接回调类
    pQot->RegisterQotSpi(pCallback); //创建行情数据回调类
    pQot->SetClientInfo("FTAPI_Sample", 1); //建立标识
    pQot->InitConnect("127.0.0.1", 11111, false); //开始连接

    Sleep(10);//为了代码简单，休眠等待连接初始化完成
    GetGlobalState.Request req;
    req.mutable_c2s()->set_userid(100000);
    pQot->GetGlobalState(req);//简单调用获取状态

    ReleaseQotApi(pQot);
    UnInitFTApi();

.. code-block:: cpp

    class SampleConnCallback : public FTSPI_Conn
    {
        public void OnInitConnect(FTAPI_Conn* pConn, FTAPI::i64_t nErrCode, const char* strDesc)
        {
           if (nErrCode == 0)
           {
               cout << "InitConnected" << endl;
           }
        }

        void OnDisConnect(FTAPI_Conn* pConn, FTAPI::i64_t nErrCode)
        {
           cout << "DisConnect" << endl;
        }
    }


  
    



