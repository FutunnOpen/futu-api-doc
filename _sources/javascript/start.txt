=========
快速上手
=========

------------------------------

----------
环境搭建
----------

.. note::

    *   建议chrome 70版本或以上

------------
代码快速入门
------------


.. code-block:: js

    import ftWebsocket from "@/components/ft-websocket/main.js";
    this.websocket = new ftWebsocket();
	//参数1指定监听地址
	//参数2指定Websocket服务端口
	//参数3指定是否启用SSL，如果需要启用则需要在FutuOpenD配置相关选项
	//参数4指定连接的密钥，否则会连接超时，密钥在在FutuOpenD可配置，UI在不指定的情况下会随机指定
    this.websocket.start("127.0.0.1", 33333, false, null);


.. code-block:: js

    //获取帐号列表
    GetAccList() {
      const req = {
        c2s: {
          userID: 0
        }
      };
      this.websocket
        .GetAccList(req)
        .then(response => {
          console.log(response);
        })
        .catch(error => {
          console.log("error:", error);
        });
    },


  
    




