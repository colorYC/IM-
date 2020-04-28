参考文件
======================

.. _创建应用:

创建应用
--------------------------------

**1.注册开发者账号**

开始之前请先 `注册成为开发者 <http://developer.juphoon.com/signup>`_ 。

如果您已经拥有开发者帐号，请直接 `登录 <http://developer.juphoon.com/signin>`_ ，并开始创建您的应用。

登录之后，您可以进行资源下载、创建应用或者进行应用的管理。

**2.创建应用**

登录开发者帐号后，点击 “添加应用”，输入应用名称以创建应用。

.. image:: images/appcreate1.png

.. note:: 应用名称创建后不可修改。

创建完成后，系统会自动为您的应用生成 **AppKey**。

点击 “显示" 即可以看到 AppKey。

.. image:: images/appkey1.png

**AppKey 是应用在菊风云平台中的唯一标识。** 

在创建应用时，``同一个控制台帐号中创建的多个应用属于同一个域。同域中的应用是可以互通的。`` 所以同一帐号下的应用即使 AppKey 不同，也可以进行业务往来。例如，从一个应用登录的用户可以呼叫另一个应用登录的用户。

应用创建完成后，即可对应用进行设置，应用设置包括基本设置和高级设置。 其中，免签权模式表示帐号由用户自行生成，用户名、密码等信息保存在菊风服务器中，服务器不检查密码。

.. image:: images/signature1.png

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _iOS 导入动态库:

iOS 导入动态库
---------------------

在 Mac 环境下解压下载的压缩包，解压后的文件夹内有 JCSDK 文件夹。JCSDK 文件夹里包含了 JCSDKOC.framework 和 JCCloudWrapper.framework。

**拷贝文件**

将 JC SDK 文件夹拷贝到您工程所在的目录下，如下图（仅供参考）：

.. image:: images/imshareLib.png

**工程设置**

1. 导入文件和库

点击 ‘General’，在 “Embedded Binaries” 一栏，点击 ‘+’ 符号，然后导入 JCSDK 文件夹下的 JCSDKOC.framework 和 JCCloudWrapper.framework

.. image:: images/inputlib_dly.png

点击 ‘General’，点击 ‘+’ 符号，在 “Linked Frameworks and Libraries” 一栏，导入 SDK 依赖的其他库，如下图：

.. image:: images/depenlib.png

2. 设置 Framework Search Paths 路径

点击 ‘Build Settings’，找到 Framework Search Paths 和 Header Search Paths，在右侧输入路径。如下图：

.. image:: images/searchpath_dly.png

.. note:: 在设置 Framework Search Paths 时，一般在完成第1步导入 JCSDKOC.framework 和 JCCloudWrapper.framework 后，Xcode 会自动生成该路径
       如果 Xcode 没有自动生成路径，用户要根据 JCSDKOC.framework 、JCCloudWrapper.framework 和头文件所在目录，手动设置路径。

3. 设置 Enable Bitcode 为 NO

点击 ‘Build Settings’，找到 Enable Bitcode 设置为 NO，如下图：

.. image:: images/bitcodeset.png

4. 设置预处理宏定义

点击 ‘Build Settings’，找到 Preprocessor Macros，在右侧输入 ZPLATFORM=ZPLATFORM_IOS，如下图：

.. image:: images/preprocessorset.png

5. 设置 Documentation Comments 为 NO

点击 ‘Build Settings’，找到 Documentation Comments 设置为 NO，如下图：

.. image:: images/documentset.png

6. 设置后台运行模式

点击 ‘Capabilities’，找到 Background Modes，勾选红框内的 Audio, AirPlay, and Picture in Picture，如下图：

.. image:: images/backgroundmode.png

**权限设置**

7. 设置麦克风和摄像头的权限

点击 ‘Info’，然后添加麦克风和摄像头权限，如下图：

.. image:: images/permission.png

8. 编译运行

以上步骤进行完后，编译工程，如果没有报错，恭喜您，您已经成功配置 SDK，可以进行下一步了。