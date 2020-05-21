ServerUid 说明
===========================

.. highlight:: objective-c

ServerUid 概念介绍
---------------------------

集成 IM 消息之前，您需要了解下面几个关键的概念。

UserId 即登录用户名，是使用 JCClient 的 login 方法登录时传入的 userId 参数。例如使用“小李”为账号进行登录时，userId 就是“小李”。

ServerUid 是菊风云平台的一个标识，分为 用户 ServerUid、群组 ServerUid。

- 用户 ServerUid

用户 ServerUid 是菊风云平台为每个用户分配的唯一标识，创建用户时由服务器生成。例如 userId 为 “小李”，他对应的 serverUid 是“10032_22”。

- 群组 ServerUid

群组 ServerUid 是菊风云平台为每个群组分配的唯一标识，创建群时由服务器生成。


.. note::

  会话使用 ServerUid 进行唯一标识，一对一会话使用对端用户的 ServerUid 作为会话的 ServerUid，群聊会话使用群组的 ServerUid 作为会话的 ServerUid。


ServerUid 获取
---------------------------

根据 Juphoon Cloud 账号查询
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

调用 JCAccount 类里的 queryServerUid 方法查询用户的 ServerUid

::

    // 查询 serverUid
    [JCCloudManager.shared.account queryServerUid:@[@"userId1", @"userId2"]];


输入参数

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - userIdArray
     - NSArray<NSString*>
     - 查询的账号数组


返回参数

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - int
     - 返回操作id


查询结果通过 onQueryServerUidResult 回调上报
::

    //查询账号 serverUid 结果回调
    - (void)onQueryServerUidResult:(int)operationId result:(BOOL)queryResult serverUids:(NSDictionary<NSString*, NSString*>*)serverUids {
        NSLog(@"serverUids 为:%@", serverUids);
    }


参数介绍

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - operationId
     - int
     - 操作ID, 由queryServerUid接口返回
   * - result
     - BOOL
     - 查询是否成功, 成功但不一定所有查询 userId 都有 serverUid
   * - serverUids
     - NSDictionary<NSString*, NSString*>
     - 查询结果


获取群组 ServerUid
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

获取群组 ServerUid 有以下几种方法：

- 通过查询所有群组获得

::

   /**
    *  @brief   查询所有群组
    *  @return 群组列表
    */
    +(NSArray<JCGroupData*>*)queryGroups;


返回参数

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - NSArray<JCGroupData*>
     - JCGroupData对象数组

- 通过查询创建的群获得
::

    /**
     *  @brief  查询创建的群
     *  @param  memberSeverUid 创建者 serverUid
     *  @return 群列表
     */
    +(NSArray<JCGroupData*>*)queryOwnedGroups:(NSString* __nonnull)memberSeverUid;

输入参数

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - memberSeverUid
     - NSString
     - 创建者 serverUid


返回参数

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - NSArray<JCGroupData*>
     - JCGroupData对象数组


- 通过查询加入的群的获得
::

    /**
     *  @brief  查询加入的群
     *  @param  memberSeverUid 创建者 serverUid
     *  @return 群列表
     */
    +(NSArray<JCGroupData*>*)queryJoinedGroups:(NSString* __nonnull)memberSeverUid;

输入参数

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - memberSeverUid
     - NSString
     - 创建者 serverUid


返回参数

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - NSArray<JCGroupData*>
     - JCGroupData对象数组


获取会话 ServerUid
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

通过 JCConversationData 实例对象的 server_uid 属性获取会话 ServerUid。

JCConversationData 实例对象可通过 queryConversations 方法获取
::

    /**
     *  @brief  查询所有会话
     */
    +(NSArray<JCConversationData*>*)queryConversations;

返回参数

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - NSArray<JCConversationData*>
     - JCConversationData对象数组

::

    //获取JCConversationData实例对象
    NSArray<JCConversationData*>* conversationData = [JCCloudDatabase queryConversations];
    //获取会话 ServerUid
    NSString *conversationServerUid = conversationData[0].server_uid;
