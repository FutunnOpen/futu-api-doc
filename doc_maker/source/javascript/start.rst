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
    this.websocket.start("127.0.0.1", 8081);


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


  
    




