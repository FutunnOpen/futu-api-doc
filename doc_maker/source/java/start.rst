=========
快速上手
=========


----------
环境搭建
----------

.. note::

    *   Windows 7/10 ，64位操作系统。Mac 10.11或以上。CentOS 7或Ubuntu 16.04以上。
    *   官方提供的SDK编译环境为OpenJDK 8，Windows下需要32位，Linux和Mac下需要64位。
    *   如需更高版本JDK，可以自己设置编译环境。

--------------
代码快速入门
--------------


.. code-block:: cpp

    FTAPI.init(); //初始化环境
    FTAPI_Conn_Qot client = new FTAPI_Conn_Qot(); //创建行情对象
    client.setConnSpi(new SampleQotCallback()); //创建连接回调类
    client.setQotSpi(new SampleQotCallback()); //创建行情数据回调类
    client.setClientInfo("FTAPI4J_Sample", 1); //建立标识
    client.initConnect("127.0.0.1", (short)11111, false); //开始连接



.. code-block:: cpp

    class SampleQotCallback implements FTSPI_Qot, FTSPI_Conn
    {
        @Override
        public void onInitConnect(FTAPI_Conn client, long errCode, String desc) 
        {
            System.out.printf("Qot onInitConnect: ret=%b desc=%s connID=%d\n", errCode, desc, client.getConnectID());
            //简单演示一下获取用户行情基本信息
            FTAPI_Conn_Qot qot = (FTAPI_Conn_Qot)client;
            {
               GetGlobalState.Request req = GetGlobalState.Request.newBuilder().setC2S(
                GetGlobalState.C2S.newBuilder().setUserID(Config.userID)
                ).build();
                int seqNo = qot.getGlobalState(req);
                System.out.printf("Send GetGlobalState: %d\n", seqNo);
            }
        }

        @Override
        public void onDisconnect(FTAPI_Conn client, long errCode) {
            System.out.printf("Qot onDisConnect: %d\n", errCode);
        }

        @Override
        public void onReply_GetGlobalState(FTAPI_Conn client, int nSerialNo, GetGlobalState.Response rsp) {
            System.out.printf("Reply: GetGlobalState: %d  %s\n", nSerialNo, rsp.toString());
        }
    }


  
    




