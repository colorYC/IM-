联系人管理
=========================

保存服务器联系人
+++++++++++++++++++++++++++

::

    [JCCloudDatabase dealServerContact:contact];

输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - contact
     - JCAccountContact
     - JCAccountContact 对象

其中， JCAccountContact 有以下属性::

    @property (nonatomic, copy) NSString *serverUid;

    @property (nonatomic) JCAccountContactType type;

    @property (nonatomic, copy) NSString *displayName;

    @property (nonatomic, copy) NSString *tag;

    @property (nonatomic) bool dnd;

    @property (nonatomic) JCAccountContactChangeState changeType;

JCAccountContactChangeState 有以下状态::

    /// 新增
    JCAccountContactChangeStateAdd = 0,
    /// 更新
    JCAccountContactChangeStateUpdate,
    /// 删除
    JCAccountContactChangeStateRemove,


查询本地保存的服务器联系人
+++++++++++++++++++++++++++

::

    JCServerContactData *contactData = [JCCloudDatabase queryServerContact:serverUid];


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - serverUid
     - NSString
     -  服务器的会话 id

返回值介绍：

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - JCServerContactData
     - JCServerContactData 对象

其中，JCServerContactData(服务器联系人数据) 有以下属性::

    /// server uid
    @property (nonatomic, strong) NSString* server_uid;
    /// 类型
    @property (nonatomic) JCAccountContactType type;
    /// 昵称
    @property (nonatomic, strong) NSString* display_name;
    /// tag
    @property (nonatomic, strong) NSDictionary* tag;
    /// 免打扰
    @property (nonatomic) bool dnd;
    /// user_id
    @property (nonatomic, strong) NSString* user_id;

查询本地保存的服务器联系人列表
++++++++++++++++++++++++++++++++++++++++

::

    NSArray<JCServerContactData*> *contactDataAry = [JCCloudDatabase queryServerContacts];

返回值介绍：

.. list-table::
   :header-rows: 1

   * - 返回值类型
     - 说明
   * - NSArray<JCServerContactData*>
     - JCServerContactData 对象列表

刷新服务器联系人
++++++++++++++++++++++++++++++++++++++++

调用 JCCloudManager 类中的 refreshContacts 方法刷新服务器联系人

::

    [JCCloudManager.shared refreshContacts:^(bool, int, NSObject * _Nullable) {
        NSLog(@"刷新服务器联系人");
    }];


输入参数介绍：

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 说明
   * - block
     - CloudOperationBlock
     - 结果函数
