﻿## 第四课 音视频事件与成员状态

### 导语
欢迎来到第四课!

在本节课程中，我们的客户端将学习如果监听房间中成员的音视频事件及成员状态。

### 预备知识
本课程要求用户已完成完成前面课程的学习。

### 相关概念
本课程涉及到的概念有:
> * **视频数据类型**
> 腾讯云支持帐号同时上行一路主流和一路辅流，这里用于区分视频流的来源称为视频类型，目前支持的视频类型有: 摄像头，屏幕分享，播片(PC端产生)

> |视频类型|流类型|常量|描述|
> |--|--|--|--|
> |摄像头|主流|CommonConstants.Const_VideoType_Camera|通过摄像头采集数据产生|
> |屏幕分享|辅流|CommonConstants.Const_VideoType_Screen|通过分享屏幕产生|
> |播片|辅流|CommonConstants.Const_VideoType_File|通过播放视频文件产生|

### 成员状态类
本课程需要监控成员状态，所以首先我们需要定义一个成员信息类:
```Java
public class MemeberInfo {
    private String identifier;      // 用户id
    private boolean openCamera = false; // 是否开启摄像头
    private boolean openMic = false;    // 关闭Mic

    public MemberInfo(String id){
        identifier = id;
    }

    //自动生成的get和set方法
}
```
这里主记录成员的摄像头和mic状态

### 成员表
为了管理房间内所有成员的状态，我们在房间模块中添加一个HashMap:
```Java
private HashMap<String, MemeberInfo> members = new HashMap<>();     // 成员表
```

并创建一个方法用于通知界面更新:
```Java
    private void notifyUpdate(){
        ArrayList<MemberInfo> tmpList = new ArrayList<>();
        for (Map.Entry<String, MemberInfo>entry : members.entrySet()){
            tmpList.add(entry.getValue());
        }
        roomView.onMemberStatusUpdate(tmpList);
    }
```

### 监听房间内音视频事件
要监听房间内的音视频事件，只需要在创建房间和加入房间的option中配置接口即可:
```Java
    // 创建房间
    public int createRoom(int roomId){
        ILiveRoomOption option = new ILiveRoomOption()
                .groupType("Public")    // 使用公开群
                .exceptionListener(this)
                .roomDisconnectListener(this)
                .setRoomMemberStatusLisenter(this); // 监听房间内音视频事件

        return ILiveRoomManager.getInstance().createRoom(roomId, option, new ILiveCallBack() {
            @Override
            public void onSuccess(Object data) {
                roomView.onEnterRoom();
            }

            @Override
            public void onError(String module, int errCode, String errMsg) {
                roomView.onEnterRoomFailed(module, errCode, errMsg);
            }
        });
    }
    // 加入房间
    public int joinRoom(int roomId){
        ILiveRoomOption option = new ILiveRoomOption()
                .groupType("Public")  // 使用公开群
                .exceptionListener(this)
                .roomDisconnectListener(this)
                .setRoomMemberStatusLisenter(this) // 监听房间内音视频事件
                .autoCamera(false)
                .autoMic(false);

        return ILiveRoomManager.getInstance().joinRoom(roomId, option, new ILiveCallBack() {
            @Override
            public void onSuccess(Object data) {
                roomView.onEnterRoom();
            }

            @Override
            public void onError(String module, int errCode, String errMsg) {
                roomView.onEnterRoomFailed(module, errCode, errMsg);
            }
        });
    }
```
然后在房间内只要有上行的音频或视频，都会收到通知:
```Java
boolean onEndpointsUpdateInfo(final int eventid, final String[] updateList);
```
其中updateList为用户id的数组，eventid为具体的事件:

|取值|事件|描述|
|--|--|--|
|3|ILiveConstants.TYPE_MEMBER_CHANGE_HAS_CAMERA_VIDEO|有上行摄像头视频事件|
|4|ILiveConstants.TYPE_MEMBER_CHANGE_NO_CAMERA_VIDEO|上行摄像头视频停止事件|
|5|ILiveConstants.TYPE_MEMBER_CHANGE_HAS_AUDIO|有上行音频事件|
|6|ILiveConstants.TYPE_MEMBER_CHANGE_NO_AUDIO|上行音频停止事件|
|7|ILiveConstants.TYPE_MEMBER_CHANGE_HAS_SCREEN_VIDEO|有上行屏幕分享事件|
|8|ILiveConstants.TYPE_MEMBER_CHANGE_NO_SCREEN_VIDEO|上行屏幕分享停止事件|
|9|ILiveConstants.TYPE_MEMBER_CHANGE_HAS_FILE_VIDEO|有上行播片事件|
|10|ILiveConstants.TYPE_MEMBER_CHANGE_NO_FILE_VIDEO|上行播片停止事件|

同时，大家可能会注意到我上面使用的是Public类型的群组，因为这种有人员上限的群组才有成员进出的IM事件通知，更多群组信息参考[房间内的群组消息互动](c_msg.md)
由于成员通知是在消息里的，所以我们需要在房间模块监听消息:
```Java
MessageObservable.getInstance().addObserver(this);
```
然后再处理GroupTips消息，处理成员进出事件:
```Java
     @Override
    public void onNewMessage(ILiveMessage message) {
        if (ILiveMessage.ILIVE_CONVERSATION_GROUP == message.getConversationType() &&
                ILiveRoomManager.getInstance().getIMGroupId().equals(message.getPeer())){
            // 过滤是当前群组的消息
            if (ILiveMessage.ILIVE_MSG_TYPE_OTHER == message.getMsgType()){
                // 处理其它消息
                ILiveOtherMessage otherMessage = (ILiveOtherMessage)message;
                TIMElem elem = otherMessage.getTIMElem();   // 取出消息内容
                if (elem.getType() == TIMElemType.GroupTips){   // 群事件消息
                    TIMGroupTipsElem groupTipsElem = (TIMGroupTipsElem)elem;
                    switch (groupTipsElem.getTipsType()){
                        case Join:      // 用户加入
                            if (!members.containsKey(groupTipsElem.getOpUser())) {
                                members.put(groupTipsElem.getOpUser(), new MemberInfo(groupTipsElem.getOpUser()));
                                notifyUpdate();
                            }
                            break;
                        case Quit:      // 用户退出
                            members.remove(groupTipsElem.getOpUser());
                            notifyUpdate();
                            break;
                    }
                }
            }
        }
    }
```
收到音视频事件也通知上层:
```Java
    @Override
    public boolean onEndpointsUpdateInfo(int eventid, String[] updateList) {
        switch (eventid){
            case ILiveConstants.TYPE_MEMBER_CHANGE_HAS_CAMERA_VIDEO:
                for (String id : updateList){
                    if (!members.containsKey(id))
                        members.put(id, new MemberInfo(id));
                    members.get(id).setOpenCamera(true);
                }
                notifyUpdate();
                break;
            case ILiveConstants.TYPE_MEMBER_CHANGE_NO_CAMERA_VIDEO:
                for (String id : updateList){
                    if (members.containsKey(id))
                        members.get(id).setOpenCamera(false);
                }
                notifyUpdate();
                break;
            case ILiveConstants.TYPE_MEMBER_CHANGE_HAS_AUDIO:
                for (String id : updateList){
                    if (!members.containsKey(id))
                        members.put(id, new MemberInfo(id));
                    members.get(id).setOpenMic(true);
                }
                notifyUpdate();
                break;
            case ILiveConstants.TYPE_MEMBER_CHANGE_NO_AUDIO:
                for (String id : updateList){
                    if (members.containsKey(id))
                        members.get(id).setOpenMic(false);
                }
                notifyUpdate();
                break;
        }

        // 这里需要返回false，否则需要上层自己实现视频请求及渲染
        return false;
    }
```

### 获取房间成员
通过上面的机制，我们知道这里成员的进出事件其实是群组系统提供的
所以在成员加入房间后，也可以通过群组接口获取群组内的成员(也就是房间成员):
```Java
    private void syncGroupMember(){
        TIMGroupManager.getInstance().getGroupMembers(ILiveRoomManager.getInstance().getIMGroupId(),
                new TIMValueCallBack<List<TIMGroupMemberInfo>>() {
                    @Override
                    public void onError(int i, String s) {
                        // 失败处理
                    }

                    @Override
                    public void onSuccess(List<TIMGroupMemberInfo> timGroupMemberInfos) {
                        for (TIMGroupMemberInfo info : timGroupMemberInfos){
                            if (!members.containsKey(info.getUser())){
                                members.put(info.getUser(), new MemberInfo(info.getUser()));
                            }
                        }
                    }
                });
    }
```
而音视频状态则不用单独获取，用户在加入房间后，就会通过onEndpointsUpdateInfo收到当前(包括在用户加入前的)打开的摄像头或麦克风的事件
### UI开发
这一课中，我需要可以创建一个成员状态图标，用于展示房间内成员，以及状态，并可以实时更新

### [源码下载](http://dldir1.qq.com/hudongzhibo/ILiveSDK/Demo/Android/demo_status.zip)

### 常见问题

- 实时音视频聊天室如何获取成员进出事件
> 实时音视频聊天室是不限制人数的，出于性能考虑，腾讯云服务器是不通知成员状态的，如果业务层有这样的需求，可以巧秒地利用群组消息实现，在成员加入成功后，发一条群自定义消息: 我来了，其它成员解析到这条消息就知道了，退出也可以同样实现。

- 创建房间时配置imsupport时SDK做了什么
> 在imsupport为true时，SDK中createRoom方法中会根据用户配置的群组类型，群组IM(没有配置则直接使用房间号)创建一个IM群组，用于消息通讯。加入房间时配置imsupport为true时则会加入(如果群组不存在会导致加入房间失败)。调用createRoom的用户在quitRoom时会自动解散群组(如果异常退出可能会导致群组仍存在)

[上一课 加入房间并视频互动](c_join.md)
