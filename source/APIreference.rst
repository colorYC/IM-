API 参考
=========================

登录类
------------------------

`JCClient（登录类） <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCClient.html>`_

`JCClientCallback（回调代理） <https://developer.juphoon.com/portal/reference/V2.0/ios/Protocols/JCClientCallback.html>`_

`JCClientState（登录状态） <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCClientState.html>`_

`JCClientReason（原因枚举） <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCClientReason.html>`_


IM 消息
------------------------

- JCCloudManager
     - 主要用于初始化、管理与cloud相关的所有回调和会话管理
- JCCloudDatabase
     - 会话信息的数据库操作（如打开/关闭数据库、获取会话相关的信息、查询、搜索、保存会话信息以及会话的管理等）
- JCMessageWrapper
     - 主要用于消息管理，包括发送消息，重发、转发、回复、消息已读、撤回、拉取消息、获取会话列表等
- JCMessageFetchManager
     - 主要用于会话同步
- JCOperationCacheDeal
     - 主要用于返回操作的结果
- JCGroupWrapper
     - 主要用于群组管理，例如创建群、解散群等操作


一对一音视频通话类
------------------------

`JCCall（一对一通话类） <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCCall.html>`_

`JCCallItem（通话对象） <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCCallItem.html>`_

`JCCallChangeParam（状态变化集合） <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCCallChangeParam.html>`_

`JCCallCallback（回调代理） <https://developer.juphoon.com/portal/reference/V2.0/ios/Protocols/JCCallCallback.html>`_

`JCCallDirection（通话方向） <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCCallDirection.html>`_

`JCCallNetWork（通话网络状态） <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCCallNetWork.html>`_

`JCCallReason（通话结束错误原因） <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCCallReason.html>`_

`JCCallState（通话状态） <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCCallState.html>`_


媒体设备类
------------------------

`JCMediaDevice（设备模块） <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDevice.html>`_

`JCMediaDeviceCallback（回调代理） <https://developer.juphoon.com/portal/reference/V2.0/ios/Protocols/JCMediaDeviceCallback.html>`_

`JCMediaDeviceVideoCanvas（视频对象，用于UI层方便操作视频） <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDeviceVideoCanvas.html>`_

`JCMediaDeviceRender（渲染模式） <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCMediaDeviceRender.html>`_

`JCMediaDeviceRotateAngle（窗口与屏幕角度） <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCMediaDeviceRotateAngle.html>`_

`JCMediaDeviceVideoPixelFormat（视频像素格式） <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCMediaDeviceVideoPixelFormat.html>`_
