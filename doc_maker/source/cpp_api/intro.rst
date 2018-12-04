使用说明
==========

 .. _ProtoBuf: ../protocol/intro.html#id4
 .. _Sample: api_sample.html
 .. _cpp-futu-api: https://github.com/FutunnOpen/cpp-futu-api
 
-----------------------------------------------------------------------------

开源项目 cpp-futu-api_ 用c++编程语言实现了futu-api协议的异步请求及推送数据的接口通道，
基于此接口通道和api协议的 ProtoBuf_ 文件定义，应用层可以灵活、扩展二次封装功能接口，具体封装可参见 Sample_ 范例工程说明

-----------------------------------------------------------------------------

下载地址
---------------

github: cpp-futu-api_ (即将开源)


-----------------------------------------------------------------------------

代码结构
---------------

.. code:: structure

    |-- OM  #基础库
    |   |-- Include
    |   |-- Lib     #各平台静态数据连接库
    |   |-- Proj    #各平台工程配置及编译目录
    |   `-- Src
    |
    |-- ThirdLib    #第三方库
    |   |-- Include
    |   |-- Lib     #各平台静态数据连接库
    |   |-- Proj    #各平台工程配置及编译目录
    |   `-- Src
    |
    `-- FTAPI       #富途API
        |-- Proj    #各平台工程配置及编译目录
            |-- Windows
                |-- VS2013
                    |-- FTAPI.sln  #解决方案
                    |-- Build
                        |-- Bin    #dll/exe
                        |-- Lib
        |-- Src
            |-- FTAPI              # FTAPI源代码
            |-- FTAPI_Sample       # Sample源代码
            |-- include_FTAPI.h    # FTAPI 对外头文件
            
         
-----------------------------------------------------------------------------

工程编译
-----------

 + OM目录是富途自研的跨平台基础开发库， 封装了异步事件处理，网络请求，文件读写，字符串，解压缩，日志等基础操作公共接口，后续将开源到github。
 
 + ThirdLib目录是目前用到的第三方开源库， 包括curl , Google protobuf, jsoncpp, minizip, openssl, pugixml, regex, sqlite3, zlib, Google Breakpad等，后续将开源到github。
 
 + FTAPI目录包含futu-api的c++接口及sample开源工程。
 
 + 当前工程配置要求OM, ThirdLib, FTAPI在同一级目录!
 
 + c++接口仅提供基础的协议请求通道，强烈建议在使用前先参考FTAPI_Sample开源工程。

 
Windows
~~~~~~~~~~~~~

 1. 推荐Windows 7/10，Visual Studio 2013 IDE开发环境。
 2. 编译方法: 用vs2013打开"./FTAPI/Src/FTAPI/Proj/Windows/VS2013/FTAPI.sln" 编译即可。
 3. 外部使用Include 头文件: "./FTAPI/Src/Include_FTAPI.h"， Lib 库文件: "./FTAPI/Proj/Windows/VS2013/Build/Lib/Release(Debug)/FTAPI.lib"，Dll动态链接库文件: "./FTAPI/Proj/Windows/VS2013/Build/Bin/Release(Debug)/FTAPI.dll(OM.dll)"。
 4. FTAPI 项目编译后同时生成了可执行范例文件: "./FTAPI/Proj/Windows/VS2013/Build/Bin/Release(Debug)/FTAPI_Sample.exe"，具体运行参见 Sample_ 说明。
 
Mac
~~~~~~~~~~~~~

 1. 即将支持
 
linux
~~~~~~~~~~~~~

 1. 即将支持
    