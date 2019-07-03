=======
快速上手
=======

------------------------------

--------
环境搭建
--------

.. note::

    *   Windows 7/10 ，64位操作系统
    *   官方提供的SDK编译环境为Visual Studio 2013，.NET Framework 4.5
    *   如需更高版本VS环境，可以升级FTAPI4Net.sln

------------
代码快速入门
------------


.. code-block:: cpp

    FTAPI.Init(); //初始化环境
    FTAPI_Qot client = new FTAPI_Qot(); //创建行情对象
    client.SetConnCallback(new SampleConnCallback()); //创建连接回调类
    client.SetQotCallback(new SampleQotCallback()); //创建行情数据回调类
    client.SetClientInfo("FTAPI4NET_Sample", 1); //建立标识
    client.InitConnect("127.0.0.1", 11111, false); //开始连接



.. code-block:: cpp

    class SampleConnCallback : FTSPI_Conn
    {
        public void OnConnect(FTAPI client, long errCode)
        {
            Console.WriteLine("Connected");
        }

        public void OnInitConnect(FTAPI client, bool ret, string desc)
        {
            Console.WriteLine("InitConnected");
            //简单演示一下获取用户行情基本信息
            FTAPI_Qot qot = client as FTAPI_Qot;
            {
                GetGlobalState.Request req = GetGlobalState.Request.CreateBuilder().SetC2S(GetGlobalState.C2S.CreateBuilder().SetUserID(900019)).Build();
                uint serialNo = qot.GetGlobalState(req);
                Console.WriteLine("Send GetGlobalState: {0}", serialNo);
            }
        }

        public void OnDisconnect(FTAPI client, long errCode, TcpDisconnectType disConnType)
        {
            Console.WriteLine("DisConnected");
        }
    }


  
    




