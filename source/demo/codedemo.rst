运行 Demo 源码
==================================

iOS
----------------------------

想要快速体验 Demo，请按以下步骤操作：


1. 获取 AppKey
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

请参考 :ref:`创建应用<创建应用>` 来获取您的 AppKey 。

.. note::

       同一个账号下创建的应用属于同一个域，同域中的应用可以互通。


2. Demo 源码下载
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

请点击这里（替换新链接） 进行 Demo 源码下载。

3. 编译运行
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

3.1 解压下载的 Demo 源码压缩包，并打开工程
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

解压后的目录如下：

.. image:: images/chatsdk.png

JuphoonCommon 为工程所依赖的独立模块，提供 http 账号管理功能。

3.2 设置自己的 AppKey
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

在下图红框标记的代码中输入自己的 AppKey

.. image:: images/chatkey.png

3.3 运行 Demo
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

连接 iOS 真机，首先编译 JuphoonCommon 工程，编译成功后会生成 JuphoonCommon.framework。

打开 JuphoonChat 工程，点击 ‘General’，在 “Linked Frameworks and Libraries” 一栏，点击 ‘+’ 号，将生成的 JuphoonCommon.framework 添加到工程中。如下图：

.. image:: images/demorun.png

点击 run，编译运行 Demo 程序。

.. note:: 当前 RealmSwift 版本为使用 Swift 5.1 编译器编译的，开发者需要根据自己当前的 Xcode 版本下载对应的 RealmSwift 库进行替换。

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


**示例图片**

.. image:: images/pic1.png
   :width: 300
   :height: 520

.. image:: images/pic2.png
   :width: 300
   :height: 520

.. image:: images/pic3.png
   :width: 300
   :height: 520

.. image:: images/pic4.png
   :width: 300
   :height: 520

.. image:: images/pic5.jpeg
   :width: 300
   :height: 520

.. image:: images/pic6.png
   :width: 300
   :height: 520

.. image:: images/pic7.png
   :width: 300
   :height: 520

.. image:: images/pic8.png
   :width: 300
   :height: 520

.. image:: images/pic9.png
   :width: 300
   :height: 520