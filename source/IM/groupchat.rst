群聊集成
=========================

.. highlight:: objective-c

前提条件
----------------------

- 支持 iOS 9.0 或以上版本的 iOS 真机设备

- 有效的菊风开发者账号（`免费注册 <http://developer.juphoon.com/signup>`_ ）


AppKey 获取
-------------------------

AppKey 是应用在 菊风云平台 中的唯一标识。需要在 SDK 初始化的时候使用，AppKey 获取请参考 :ref:`创建应用 <创建应用>` 。


SDK 下载
-------------------------

点击 `iOS SDK <>`_ 进行下载。如果已经下载了 SDK，请直接进行 SDK 配置。


导入 SDK
-------------------------

SDK 有两种导入方式：CocoaPods 导入和手动导入。


CocoaPods 导入
>>>>>>>>>>>>>>>>>>>>>>>>>>

SDK 支持使用 CocoaPods 导入并管理 SDK。 需安装 CocoaPods 环境，请参照 `CocoaPods 安装 <https://cocoapods.org/>`_ 。

CocoaPods 环境配置好后，按下面的步骤导入 SDK

1. cd 至项目根目录

2. 执行 pod init

3. 执行 open -e Podfile

4. 在 Podfile 文件中添加下面内容
::

    pod 'JphoonSDK', '~> 1.20.0'

1.20.0 为当前最新版本，如需指定具体版本，请注意 `pod 使用规范 <https://guides.cocoapods.org/using/the-podfile.html>`_  。

5. 执行 pod install

6. 双击打开 .xcworkspace 即可


手动导入
>>>>>>>>>>>>>>>>>>>>>>>>>>

您可以在工程中使用静态库或者动态库，此处介绍使用静态库的配置方法。如果想使用动态库，请参考动态库的配置说明文档 :ref:`iOS 导入动态库<iOS 导入动态库>` 。

.. note::

        如果您已经集成了多家音视频引擎，则推荐使用动态库。

只有完成 SDK 的配置之后，您才可以集成 JC SDK 提供的功能，请按以下操作完成静态库的配置。

**导入静态库**

在 Mac 环境下打开下载的 iOS SDK，在 sdk 文件夹内包含了 lib、framework、include 和 3rd 四个文件夹。

.. image:: images/imLib.png

``拷贝文件``

将 sdk 文件夹拷贝到您工程所在的目录下。

``工程设置``

**1. 导入 SDK**

打开 Xcode，进入 TARGETS > Project Name > Build Phases > Link Binary with Libraries 菜单，点击 ‘+’ 符号，导入 sdk framework 文件夹下的 JCSDKOC.framework、JCCloudWrapper.framework 和 lib 文件夹下的两个 .a 文件，如下图：

.. image:: images/inputimlib.png

**2. 导入 SDK 依赖的库**

继续点击 ‘+’ 符号，导入下图红框中的库：

.. image:: images/inputdepenlib.png

**3. 设置路径**

点击 ‘Build Settings’，找到 Framework Search Paths 、Header Search Paths（头文件路径） 和 Library Search Paths（库文件路径）。并设置 Framework Search Paths、Header Search Paths 和 Library Search Paths，如下图：

.. image:: images/searchpath.png

.. note:: 在完成第1步导入 JCSDKOC.framework、JCCloudWrapper.framework 和两个 .a 文件后，Xcode 会自动生成该路径，如果 Xcode 没有自动生成路径，用户要根据 JCSDKOC.framework、JCCloudWrapper.framework 、include 和 lib 库文件所在目录，手动设置路径。

**4. 设置 Enable Bitcode 为 NO**

点击 ‘Build Settings’，找到 Enable Bitcode 设置为 NO，如下图：

.. image:: images/bitcodeset.png

**5. 设置 Other Linker Flags 的参数为 -ObjC**

点击 ‘Build Settings’，找到 Other Linker Flags 并添加参数 -ObjC，如下图：

.. image:: images/otherlinkflag.png

**6.设置预处理宏定义**

点击 ‘Build Settings’，找到 Preprocessor Macros，在右侧输入 ZPLATFORM=ZPLATFORM_IOS，**如果设置了 APNs 推送**，则还需要在 Preprocessor Macros 下的 Debug 中输入 DEBUG，如下图：

.. image:: images/preprocessorset.png

.. note::

    DEBUG 宏定义的目的是为了区分推送环境是 release 还是 debug，环境不对会导致推送失败。

**7. 设置 Documentation Comments 为 NO**

点击 ‘Build Settings’，找到 Documentation Comments 并设置为 NO，如下图：

.. image:: images/documentset.png

**8. 设置后台运行模式**

点击 ‘Signing & Capabilities’，点击 +Capability 找到 Background Modes，勾选红框内的 Audio, AirPlay, and Picture in Picture ，如下图：

.. image:: images/backgroundmode.png

**权限设置**

**9. 设置麦克风和摄像头权限**

点击 ‘Info’，然后添加麦克风和摄像头的权限，如下图：

.. image:: images/permission.png

.. list-table::
   :header-rows: 1

   * - Key
     - Type
     - Value
   * - Privacy - Microphone Usage Description
     - String
     - 使用麦克风的目的，如语音通话。
   * - Privacy - Camera Usage Description
     - String
     - 使用摄像头的目的，如视频通话。

**10. 编译运行**

以上步骤进行完后，编译工程，如果提示 succeeded，恭喜您已经成功配置 SDK，可以进行 SDK 初始化了。

.. note:: SDK 不支持模拟器运行，请使用真机。


引入 SDK
-------------------------

引入头文件

使用 SDK 功能前，需要 import 头文件，Swift 项目需要在工程的 Bridging-Header.h 文件中添加 SDK 的引用。
::

    #import <JCCloudWrapper/JCCloudWrapper.h>

初始化
-------------------------

开发者在使用 JC SDK 所有功能之前，必须先调用初始化方法初始化 SDK。 在 App 的整个生命周期中，开发者只需要将 SDK 初始化一次。
::

    JCClientCreateParam *param = [[JCClientCreateParam alloc] init];
    param.sdkLogLevel = JCLogLevelInfo;
    param.sdkInfoDir = @"SDK 信息存放路径";
    param.sdkLogDir = @"日志存放路径";
    [JCCloudManager.shared initialize:@"your appkey" createParam:param];


参数介绍：

输入参数

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 必填
     - 说明
   * - appKey
     - NSString
     - 是
     - 创建应用获取的AppKey，如果还未获取 AppKey，请参考 :ref:`创建应用 <创建应用>` 来获取。
   * - createParam
     - JCClientCreateParam
     - 否
     - 创建参数，通过该参数可以设置 SDK 信息存储目录，日志路径以及日志打印的等级，如果不设置则使用默认值。

返回参数

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - bool
     - 初始化是否成功

其中，JCClientCreateParam 对象有以下属性
::

    /// sdk信息存储目录
    @property (nonatomic, copy) NSString* __nonnull sdkInfoDir;

    /// sdk日志目录
    @property (nonatomic, copy) NSString* __nonnull sdkLogDir;

    /// sdk日志等级 JCLogLevel
    @property (nonatomic) JCLogLevel sdkLogLevel;

日志等级（JCLogLevel）有四种::

    /// Disable
    JCLogLevelDisable,
    /// Error
    JCLogLevelError,
    /// Info
    JCLogLevelInfo,
    /// Debug
    JCLogLevelDebug


销毁SDK调用反初始化接口
::

    [JCCloudManager.shared uninitialize];


账号管理
-----------------------

账号管理主要包括登录和设置昵称。

登录
>>>>>>>>>>>>>>>>>>>>>

**登录介绍**

登录涉及 JCClient 类及其回调 JCClientCallback，其主要作用是负责登录、登出管理及帐号信息存储。

只有登录成功后才能进行平台上的各种业务。服务器分为鉴权模式和非鉴权模式

 - 鉴权模式: 服务器会检查用户名和密码

 - 免鉴权模式: 只要用户保证用户标识唯一即可, 服务器不校验

.. note::

    目前只支持免鉴权模式，免鉴权模式下当账号不存在时会自动去创建该账号。

在 App 整个生命周期，开发者只需要调用一次登录方法进行登录。之后无论是网络出现异常或者 App 有前后台的切换等，SDK 都会负责自动重连服务器。除非用户主动调用登出接口，或者因为帐号在其他设备登录导致该设备被登出。

登录过程如下：

.. image:: images/loginflow.png

**发起登录**

登录之前，可以通过 loginParam 登录参数进行登录的相关配置，如服务器地址的设置或者使用代理服务器登录，如不设置则按照默认值登录，具体如下：

::

        JCClientLoginParam* loginParam = [[JCClientLoginParam alloc] init];
        //默认国内环境 http:cn.router.justalkcloud.com:8080
        loginParam.serverAddress = @"服务器地址";
        //如果使用代理服务器登录
        loginParam.httpsProxy = @"代理服务器地址";
        //发起登录
        [JCCloudManager.shared.client login:@"手机号码" password:@"密码" loginParam:loginParam];

其中，服务器地址包括国际环境服务器地址和国内环境服务器地址：

**国际环境** 服务器地址默认为 ``http:intl.router.justalkcloud.com:8080`` 。

**国内环境** 服务器地址默认为 ``http:cn.router.justalkcloud.com:8080`` 。

开发者可以使用 **自定义服务器地址 **。

参数介绍：

输入参数

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 必填
     - 说明
   * - userId
     - NSString
     - 是
     - 用户名，为英文数字和'+' '-' '_' '.'，长度不要超过64字符，'-' '_' '.'不能作为第一个字符
   * - password
     - NSString
     - 是
     - 密码，免鉴权模式密码可以随意输入，但不能为空
   * - loginParam 登录参数，nil则按照默认值登录
     - JCClientLoginParam
     - 否
     - 登录参数，nil则按照默认值登录

返回参数

.. list-table::
   :header-rows: 1

   * - 返回类型
     - 说明
   * - bool
     - true 表示正常执行调用流程，false 表示调用异常，异常错误通过 JCClientCallback 通知

其中，JCClientLoginParam 对象有以下属性
::

    /// 服务器地址，默认国内环境 http:cn.router.justalkcloud.com:8080
    @property (nonatomic, copy) NSString* __nonnull serverAddress;

    /// 设备id，一般模拟器使用，因为模拟器可能获得的设备id都一样
    @property (nonatomic, copy) NSString* __nonnull deviceId;

    /// https代理, 例如 192.168.1.100:3128
    @property (nonatomic, copy) NSString* __nullable httpsProxy;

    /// 登录账号不存在的情况下是否内部自动创建该账号，默认为 true
    @property (nonatomic) bool autoCreateAccount;

    /**
     * @brief 终端类型，如果需要多终端登录，则需要为每一类型的设备设置一个类型
     *
     * 例如需要手机端和PC端同时能登录，则手机端设置 moblie，pc端设为 pc，
     * 在调用 login 接口时会把同一类型登录的其他终端踢下线
     * 调用 relogin 接口如果有该类型终端的登录用户则会登录失败
     */
    @property (nonatomic, strong) NSString* __nonnull terminalType;


登录操作发起后，SDK 与菊风服务器的连接状态将发生变化，当 SDK 与菊风服务器的连接状态发生变化时，SDK 会通过 JCClientCallback 回调上报，开发者可通过实现对应的回调方法进行相应的处理。

登录成功之后，首先会触发登录状态改变（onClientStateChange）回调
::

    -(void)onClientStateChange:(JCClientState)state oldState:(JCClientState)oldState
    {
        if (state == JCClientStateIdle) { // 未登录
           ...
        } else if (state == JCClientStateLogining) { // 登录中
           ...
        } else if (state == JCClientStateLogined) {  // 登录成功
           ...
        } else if (state == JCClientStateLogouting) {  // 登出中
           ...
        }
    }


参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - state
     - JCClientState
     - 当前状态值
   * - oldState
     - JCClientState
     - 之前状态值


其中，JCClientState 有::

    /// 未初始化
    JCClientStateNotInit,
    /// 未登陆
    JCClientStateIdle,
    /// 登陆中
    JCClientStateLogining,
    /// 登陆成功
    JCClientStateLogined,
    /// 登出中
    JCClientStateLogouting,


之后通过 onLogin 回调上报登录结果
::

    -(void)onLogin:(bool)result reason:(JCClientReason)reason {
        if (result) {
            //界面处理
        } else {
            //界面处理
        }
    }


参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - result
     - bool
     - true 表示登陆成功，false 表示登陆失败
   * - reason
     - JCClientReason
     - 当 result 为 false 时该值有效


其中，JCClientReason 有
::

    /// 正常
    JCClientReasonNone,
    /// sdk 未初始化
    JCClientReasonSDKNotInit,
    /// 无效的参数
    JCClientReasonInvalidParam,
    /// 函数调用失败
    JCClientReasonCallFunctionError,
    /// 当前状态无法再次登录
    JCClientReasonStateCannotLogin,
    /// 超时
    JCClientReasonTimeOut,
    /// 网络异常
    JCClientReasonNetWork,
    /// appkey 错误
    JCClientReasonAppKey,
    /// 账号密码错误
    JCClientReasonAuth,
    /// 无该用户
    JCClientReasonNoUser,
    /// 被强制登出
    JCClientReasonServerLogout,
    /// 其他设备已登录
    JCClientReasonAnotherDeviceLogined,
    /// 本地请求失败
    JCClientReasonLocalRequest,
    /// 发消息失败
    JCClientReasonSendMessage,
    /// 服务器忙
    JCClientReasonServerBusy,
    /// 服务器不可达
    JCClientReasonServerNotReach,
    /// 服务器不可达
    JCClientReasonServerForbidden,
    /// 服务器不可用
    JCClientReasonServerUnavaliable,
    /// DNS 查询错误
    JCClientReasonDnsQuery,
    /// 服务器内部错误
    JCClientReasonInternal,
    /// 无资源
    JCClientReasonNoResource,
    /// 没有回应验证码
    JCClientReasonNoNonce,
    /// 无效验证码
    JCClientReasonInvalidAuthCode,
    /// token不匹配
    JCClientReasonTokenMismatch,
    /// 其他错误
    JCClientReasonOther = 100,

登录成功之后，SDK 会自动保持与服务器的连接状态，直到用户主动调用登出接口，或者因为帐号在其他设备登录导致该设备被登出。


登出
>>>>>>>>>>>>>>>>>>>>>

登出是指断开与菊风服务器的连接，登出后不能进行平台上的各种业务操作。

登出过程如下：

.. image:: images/logoutflow.png

登出发起
::

    [JCCloudManager.shared.client logout];

登出同样会触发登录状态改变（onClientStateChange）回调

之后将通过 onlogout 回调上报登出结果
::

    -(void)onLogout:(JCClientReason)reason {
        NSLog(@"登出原因是%d", reason);
    }


参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - reason
     - JCClientReason
     - 登出原因


设置昵称
>>>>>>>>>>>>>>>>>>>>>

开发者可以通过 JCClient 类中的 displayName 属性设置昵称
::

    /**
     *  @brief 昵称，用于通话，消息等，可以更直观的表明身份
     */
    @property (nonatomic, copy) NSString* __nonnull displayName;


示例代码::

    client.displayName = @"小张";

登录集成成功之后，即可进行相关业务的集成。


业务集成
----------------------

群聊主要涉及以下几个的类

.. list-table::
   :header-rows: 1

   * - 名称
     - 描述
   * - JCCloudManager
     - 主要用于初始化、管理与cloud相关的所有回调和会话管理
   * - JCCloudDatabase
     - 会话信息的数据库操作（如打开/关闭数据库、获取会话相关的信息、查询、搜索、保存会话信息以及会话的管理等）
   * - JCMessageWrapper
     - 主要用于消息管理，包括发送消息，重发、转发、回复、消息已读、撤回、拉取消息、获取会话列表等
   * - JCGroupWrapper
     - 主要用于群组管理，例如创建群、解散群等操作
   * - JCMessageFetchManager
     - 主要用于会话同步
   * - JCOperationCacheDeal
     - 主要用于返回操作的结果


群组管理
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

群组管理包括创建群、删除群、更新群以及查询群功能。

创建群组
++++++++++++++++++++++++++++++

创建群组需要传入群成员对象，首先调用下面的方法构造群成员对象
::

    //构造 JCGroupMember
    JCGroupMember *member = [[JCGroupMember alloc] init:@"群groupId" userId:@"登录cloud平台的账号" uid:@"uid" displayName:@"群昵称" memberType:JCGroupMemberTypeMember changeState:JCGroupChangeStateAdd];


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - groupId
     - NSString
     - 群组唯一标识
   * - userId
     - NSString
     - 用户标识
   * - uid
     - NSString
     - 服务器端用户标识，当通知成员变化时，changeState 为 JCGroupChangeStateRemove 时只能通过此参数来判断，不能通过 userId
   * - displayName
     - NSString
     - 昵称
   * - memberType
     - JCGroupMemberType
     - 成员类型
   * - changeState
     - JCGroupChangeState
     - 成员变化状态


JCGroupMember 对象的详细信息请参考 API reference。

返回值介绍：

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - instancetype
     - 返回 JCGroupItem 对象


然后调用下面的方法创建群组
::

    NSArray<JCGroupMember *> *memberList = [NSArray array];
    JCGroupMember *member1 = [[JCGroupMember alloc] init:@"群groupId" userId:@"登录cloud平台的账号" uid:@"uid" displayName:@"群昵称" memberType:JCGroupMemberTypeMember changeState:JCGroupChangeStateAdd];
    JCGroupMember *member2 = [[JCGroupMember alloc] init:@"群groupId" userId:@"登录cloud平台的账号" uid:@"uid" displayName:@"群昵称" memberType:JCGroupMemberTypeMember changeState:JCGroupChangeStateAdd];
    memberList = @[member1, member2];
    [JCGroupWrapper createGroup:memberList groupName:@"群组名称" type:JCGroupTypeNormal customProperties:nil usingBlock:^(bool, int, NSObject * _Nullable) {
        NSLog(@"创建群组");
    }];


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - members
     - NSArray<JCGroupMember*>
     - 成员列表，uid, memberType 和 displayname 需要赋值
   * - groupName
     - NSString
     - 群名字
   * - type
     - JCGroupType
     - 群类型
   * - customProperties
     - NSDictionary<NSString*, NSObject*>
     - 群自定义属性
   * - block
     - GroupOperationBlock
     - 结果函数


相关回调

创建群会触发 onGroupAdd（新增群）回调
::

    -(void)onGroupAdd:(JCGroupData*)group {
        NSLog(@"新增群");
    }

参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - group
     - JCGroupData
     - JCGroupData 对象


解散群组
++++++++++++++++++++++++++++++

调用下面的方法解散群组
::

    [JCGroupWrapper dissolve:@"groupServerUid" usingBlock:^(bool, int, NSObject * _Nullable) {
        NSLog(@"解散群组");
    }];


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - groupServerUid
     - NSString
     - 群 ServerUid
   * - block
     - GroupOperationBlock
     - 结果函数


相关回调

解散群组会触发 onGroupDelete 回调，可以在该回调中进行后续的处理
::

    -(void)onGroupDelete:(JCGroupData*)group {
        NSLog(@"删除群");
    }


参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - group
     - JCGroupData
     - JCGroupData 对象


更新群组
++++++++++++++++++++++++++++++

更新群组包括增删成员、设置成员的角色、修改群相关属性，如名称等、上传头像、拉取群消息等。

添加成员
^^^^^^^^^^^^^^^^^^^^^^^^^

调用下面的方法向群组中添加成员
::

    NSArray<JCGroupMember *> *memberList = [NSArray array];
    JCGroupMember *member1 = [[JCGroupMember alloc] init:@"群groupId" userId:@"登录cloud平台的账号" uid:@"uid" displayName:@"群昵称" memberType:JCGroupMemberTypeMember changeState:JCGroupChangeStateAdd];
    JCGroupMember *member2 = [[JCGroupMember alloc] init:@"群groupId" userId:@"登录cloud平台的账号" uid:@"uid" displayName:@"群昵称" memberType:JCGroupMemberTypeMember changeState:JCGroupChangeStateAdd];
    memberList = @[member1, member2];
    [JCGroupWrapper addMembers:@"群 ServerUid" members:memberList usingBlock:^(bool, int, NSObject * _Nullable) {
        NSLog(@"添加群成员");
    }];


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - groupServerUid
     - NSString
     - 群 ServerUid
   * - members
     - NSArray<JCGroupMember*>
     - 成员列表，uid 和 displayname 需要赋值
   * - block
     - GroupOperationBlock
     - 结果函数


踢掉人员
^^^^^^^^^^^^^^^^^^^^^^^^^

调用下面的方法踢掉群组中的人员
::

    NSArray<NSString*>* uidAry = [NSArray array];
    [uidAry arrayByAddingObject:@"uid1"];
    [JCGroupWrapper kickMembers:@"群 ServerUid" memberServerUids:uidAry usingBlock:^(bool, int, NSObject * _Nullable) {
        NSLog(@"剔除成员");
    }];


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - groupServerUid
     - NSString
     - 群 ServerUid
   * - memberServerUids
     - NSArray<NSString*>
     - 成员列表
   * - block
     - GroupOperationBlock
     - 结果函数

相关回调

删除群成员会触发 onGroupMemberDelete 回调
::

    -(void)onGroupMemberDelete:(JCGroupMemberData*)member {
        NSLog(@"删除群成员");
    }


参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - member
     - JCGroupMemberData
     - JCGroupMemberData 对象


设置普通成员
^^^^^^^^^^^^^^^^^^^^^^^^^

如果想把某个管理员设置为普通群成员，可以调用下面的方法，**只有当前群主才可以进行此操作**
::

    [JCGroupWrapper modifyToMember:@" 群 ServerUid" memberServerUid:@"成员 serverUid" usingBlock:^(bool, int, NSObject * _Nullable) {
        NSLog(@"设置普通成员");
    }];


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - groupServerUid
     - NSString
     - 群 ServerUid
   * - memberServerUid
     - NSString
     - 成员 serverUid
   * - block
     - GroupOperationBlock
     - 结果函数


设置管理员
^^^^^^^^^^^^^^^^^^^^^^^^^

如果想把某个成员设置为管理员，可以调用下面的方法，**只有当前群主才可以进行此操作**
::

    [JCGroupWrapper modifyToManager:@" 群 ServerUid" memberServerUid:@"成员 serverUid" usingBlock:^(bool, int, NSObject * _Nullable) {
        NSLog(@"设置管理员");
    }];


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - groupServerUid
     - NSString
     - 群 ServerUid
   * - memberServerUid
     - NSString
     - 成员 serverUid
   * - block
     - GroupOperationBlock
     - 结果函数


设置群主
^^^^^^^^^^^^^^^^^^^^^^^^^

如果想把某个成员设置为群主，可以调用下面的方法，**只有当前群主才可以进行此操作**
::

    [JCGroupWrapper modifyToOwner:@" 群 ServerUid" memberServerUid:@"成员 serverUid" usingBlock:^(bool, int, NSObject * _Nullable) {
        NSLog(@"设置管理员");
    }];


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - groupServerUid
     - NSString
     - 群 ServerUid
   * - memberServerUid
     - NSString
     - 成员 serverUid
   * - block
     - GroupOperationBlock
     - 结果函数


修改自己的群昵称
^^^^^^^^^^^^^^^^^^^^^^^^^

如果想修改自己的群昵称，可以调用下面的方法
::

    [JCGroupWrapper changeDisplayName:@" 群 ServerUid" displayName:@"新的昵称" usingBlock:^(bool, int, NSObject * _Nullable) {
        NSLog(@"修改群昵称");
    }];


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - groupServerUid
     - NSString
     - 群 ServerUid
   * - displayName
     - NSString
     - 昵称
   * - block
     - GroupOperationBlock
     - 结果函数


设置群自定义属性
^^^^^^^^^^^^^^^^^^^^^^^^^

如果想设置群自定义属性，可以调用下面的方法
::

    NSDictionary<NSString*, NSObject*> *customProperties = [NSDictionary dictionary];
    [customProperties setObject:@"object" forKey:@"key"];
    [JCGroupWrapper setGroupCustomProperties:@" 群 ServerUid" displayName:@"新的昵称" customProperties:customProperties usingBlock:^(bool, int, NSObject * _Nullable) {
        NSLog(@"设置群自定义属性");
    }];


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - groupServerUid
     - NSString
     - 群 ServerUid
   * - customProperties
     - NSDictionary<NSString*, NSObject*>
     - 群自定义属性集
   * - block
     - GroupOperationBlock
     - 结果函数


群备注更新
^^^^^^^^^^^^^^^^^^^^^^^^^

调用下面的方法更新群备注
::

/**
 * @brief 群备注更新
 * @param groupServerUid 群 ServerUid
 * @param nickName 群备注名
 * @param tag 标签，内部会将该 NSDictionary 转为 json
 * @param block 结果函数
 */
+(void)updateComment:(NSString* __nonnull)groupServerUid nickName:(NSString* __nullable)nickName tag:(NSDictionary<NSString*, NSObject*>* __nullable)tag usingBlock:(GroupOperationBlock)block;

输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - members
     - NSArray<JCGroupMember*>
     - 成员列表，uid, memberType 和 displayname 需要赋值
   * - groupName
     - NSString
     - 群名字
   * - type
     - JCGroupType
     - 群类型
   * - customProperties
     - NSDictionary<NSString*, NSObject*>
     - 群自定义属性
   * - block
     - GroupOperationBlock
     - 结果函数


更改群名称
^^^^^^^^^^^^^^^^^^^^^^^^^

调用下面的方法更改群名称
::

/**
 * @brief 更改群名称
 * @param groupServerUid 群 ServerUid
 * @param groupName 群名字
 * @param block 结果函数
 */
+(void)changeGroupName:(NSString* __nonnull)groupServerUid groupName:(NSString* __nonnull)groupName usingBlock:(GroupOperationBlock)block;

输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - members
     - NSArray<JCGroupMember*>
     - 成员列表，uid, memberType 和 displayname 需要赋值
   * - groupName
     - NSString
     - 群名字
   * - type
     - JCGroupType
     - 群类型
   * - customProperties
     - NSDictionary<NSString*, NSObject*>
     - 群自定义属性
   * - block
     - GroupOperationBlock
     - 结果函数

上传群头像
^^^^^^^^^^^^^^^^^^^^^^^^^

调用下面的方法上传群头像
::

/**
 * @brief 上传群头像，最终是群的 customProperties 会增加 "Icon"（JCGroupIconPropertyKey 在 JCCloudConstants.h 中） 字段，存的是服务器文件链接
 * @param path 头像文件路径
 * @param block 结果函数，block 的 obj 为 groupServerUid
 */
+(void)updateGroupIcon:(NSString* __nonnull)groupServerUid path:(NSString*)path usingBlock:(GroupOperationBlock)block;

拉取群消息
^^^^^^^^^^^^^^^^^^^^^^^^^

调用下面的方法拉取群消息
::


/**
 *  @brief 更新群信息
 *  @param block 结果函数
 */
+(void)refreshGroupInfo:(NSString* __nonnull)groupServerId usingBlock:(GroupOperationBlock)block;

输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - members
     - NSArray<JCGroupMember*>
     - 成员列表，uid, memberType 和 displayname 需要赋值
   * - groupName
     - NSString
     - 群名字
   * - type
     - JCGroupType
     - 群类型
   * - customProperties
     - NSDictionary<NSString*, NSObject*>
     - 群自定义属性
   * - block
     - GroupOperationBlock
     - 结果函数


拉取服务器更新
^^^^^^^^^^^^^^^^^^^^^^^^^

调用下面的方法拉取服务器更新
::


/**
 *  @brief 拉取服务器更新
 *  @param block 结果函数
 */
+(void)refreshGroups:(GroupOperationBlock)block;


离开群组
^^^^^^^^^^^^^^^^^^^^^^^^^

调用下面的方法离开群组
::

/**
 * @brief 离开，群主必须转移群主后才能离开
 * @param groupServerUid 群 ServerUid
 * @param block 结果函数
 */
+(void)leave:(NSString* __nonnull)groupServerUid usingBlock:(GroupOperationBlock)block;

输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - members
     - NSArray<JCGroupMember*>
     - 成员列表，uid, memberType 和 displayname 需要赋值
   * - groupName
     - NSString
     - 群名字
   * - type
     - JCGroupType
     - 群类型
   * - customProperties
     - NSDictionary<NSString*, NSObject*>
     - 群自定义属性
   * - block
     - GroupOperationBlock
     - 结果函数


查询群组
++++++++++++++++++++++++++

查询所有群组
^^^^^^^^^^^^^^^^^^^^^^^^^^^

调用下面的方法查询群
::

/**
 *  @brief   查询所有群组
 *  @return 群组列表
 */
+(NSArray<JCGroupData*>*)queryGroups;

输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - type
     - JCConversationType
     - 会话类型，一对一和群聊
   * - serverUid
     - NSString
     - 服务器会话 uid，一对一实际是对方的个人 uid，群组 id 要创建成功才能获得
   * - name
     - NSString
     - 会话名字，只针对一对一会话有效
   * - lastActiveTime
     - long
     - 最后活跃时间,  <=0 则按当前时间

返回值介绍：

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - long
     - 会话id，没有返回 -1


查询单个群组
^^^^^^^^^^^^^^^^^^^^^^^^^^^


/**
 *  @brief   查询群
 *  @param  serverUid 群服务器 uid
 *  @return 群组对象
 */
+(JCGroupData*)queryGroup:(NSString* __nonnull)serverUid;


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - type
     - JCConversationType
     - 会话类型，一对一和群聊
   * - serverUid
     - NSString
     - 服务器会话 uid，一对一实际是对方的个人 uid，群组 id 要创建成功才能获得
   * - name
     - NSString
     - 会话名字，只针对一对一会话有效
   * - lastActiveTime
     - long
     - 最后活跃时间,  <=0 则按当前时间

返回值介绍：

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - long
     - 会话id，没有返回 -1


查询群成员列表
^^^^^^^^^^^^^^^^^^^^^^^^^^^

/**
 *  @brief   查询群成员列表
 *  @param  serverUid 群服务器 uid
 *  @return 成员列表
 */
+(NSArray<JCGroupMemberData*>*)queryGroupMembers:(NSString* __nonnull)groupServerUid;


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - type
     - JCConversationType
     - 会话类型，一对一和群聊
   * - serverUid
     - NSString
     - 服务器会话 uid，一对一实际是对方的个人 uid，群组 id 要创建成功才能获得
   * - name
     - NSString
     - 会话名字，只针对一对一会话有效
   * - lastActiveTime
     - long
     - 最后活跃时间,  <=0 则按当前时间

返回值介绍：

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - long
     - 会话id，没有返回 -1


查询单个成员
^^^^^^^^^^^^^^^^^^^^^^^^^^^

/**
 *  @brief   查询单个成员
 *  @param  serverUid 群服务器 uid
 *  @param  memberServerUid 成员ServerUid
 *  @return 成员对象
 */
+(JCGroupMemberData*)queryGroupMember:(NSString* __nonnull)serverUid memberServerUid:(NSString* __nonnull)memberServerUid;


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - type
     - JCConversationType
     - 会话类型，一对一和群聊
   * - serverUid
     - NSString
     - 服务器会话 uid，一对一实际是对方的个人 uid，群组 id 要创建成功才能获得
   * - name
     - NSString
     - 会话名字，只针对一对一会话有效
   * - lastActiveTime
     - long
     - 最后活跃时间,  <=0 则按当前时间

返回值介绍：

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - long
     - 会话id，没有返回 -1


查询创建的群
^^^^^^^^^^^^^^^^^^^^^^^^^^^

/**
 *  @brief  查询创建的群
 *  @param  memberSeverUid 创建者 serverUid
 *  @return 群列表
 */
+(NSArray<JCGroupData*>*)queryOwnedGroups:(NSString* __nonnull)memberSeverUid;

输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - type
     - JCConversationType
     - 会话类型，一对一和群聊
   * - serverUid
     - NSString
     - 服务器会话 uid，一对一实际是对方的个人 uid，群组 id 要创建成功才能获得
   * - name
     - NSString
     - 会话名字，只针对一对一会话有效
   * - lastActiveTime
     - long
     - 最后活跃时间,  <=0 则按当前时间

返回值介绍：

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - long
     - 会话id，没有返回 -1

查询加入的群
^^^^^^^^^^^^^^^^^^^^^^^^^^^

/**
 *  @brief  查询加入的群
 *  @param  memberSeverUid 创建者 serverUid
 *  @return 群列表
 */
+(NSArray<JCGroupData*>*)queryJoinedGroups:(NSString* __nonnull)memberSeverUid;


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - type
     - JCConversationType
     - 会话类型，一对一和群聊
   * - serverUid
     - NSString
     - 服务器会话 uid，一对一实际是对方的个人 uid，群组 id 要创建成功才能获得
   * - name
     - NSString
     - 会话名字，只针对一对一会话有效
   * - lastActiveTime
     - long
     - 最后活跃时间,  <=0 则按当前时间

返回值介绍：

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - long
     - 会话id，没有返回 -1


搜索包括关键字的群
^^^^^^^^^^^^^^^^^^^^^^^^^^^

/**
 * @brief 搜索包括关键字的群（群名，群别名，群成员），没有匹配成员则 JCGroupSearchData 的 member 为空
 * @param key 搜索关键字
 * @param includNickName 是否包含搜索群的 nickName
 */
+(NSArray<JCGroupSearchData*>*)searchGroup:(NSString* __nonnull)key includNickName:(bool)includNickName;


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - type
     - JCConversationType
     - 会话类型，一对一和群聊
   * - serverUid
     - NSString
     - 服务器会话 uid，一对一实际是对方的个人 uid，群组 id 要创建成功才能获得
   * - name
     - NSString
     - 会话名字，只针对一对一会话有效
   * - lastActiveTime
     - long
     - 最后活跃时间,  <=0 则按当前时间

返回值介绍：

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - long
     - 会话id，没有返回 -1

*解散群组*

*加入群组*

*退出群组*

*群组成员查询*

*刷新群组信息*

*同步用户群组*

*增删群成员*

*修改群名称*

*设置群昵称*

*设置群头像*

*群公告设置*

群聊会话管理
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

JCCloudDatabase.h

*获取全部会话*

*删除全部会话*

*会话未读数*

*会话免打扰*

*会话置顶*

*添加成员*

消息管理
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

JCMessageWrapper.h

*支持的消息类型：文字、文件、图片、表情、位置、语音消息、小视频*

*消息发送*

*消息接收*

*历史消息获取*

*消息回执*

*消息撤回*

*消息转发*

*消息删除*

*消息搜索*

*消息同步*

*发送@消息*

JCMessageFetchManager.h

相关回调

更新群会触发 onGroupUpdate 回调
::


    -(void)onGroupUpdate:(JCGroupData*)group {
        NSLog(@"更新群");
    }


参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - group
     - JCGroupData
     - JCGroupData 对象

添加群成员会触发 onGroupMemberAdd 回调
::

    -(void)onGroupMemberAdd:(JCGroupMemberData*)member {
        NSLog(@"添加群成员");
    }


参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - member
     - JCGroupMemberData
     - JCGroupMemberData 对象

群成员更新会触发 onGroupMemberUpdate 回调
::

    -(void)onGroupMemberUpdate:(JCGroupMemberData*)member {
        NSLog(@"群成员更新");
    }


参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - member
     - JCGroupMemberData
     - JCGroupMemberData 对象

