本文描述的是进阶 API 的说明及其使用。

## 概览
### [基础] 接口 & 事件通知
> 了解一下接口的使用，就已经可以实现主要功能。

#### 接口
| API                   |  描述            |
| -------------------- | -------- |
| [WebRTCAPI.fn.detectRTC](#webrtcapi.fn.detectrtc)     | 检测是否支持 WebRTC |
| [WebRTCAPI](#webrtcapi)     | 初始化 |
| [WebRTCAPI.createRoom](#webrtcapi.createroom)     | 创建或进入音视频房间 |
| [WebRTCAPI.quit](#webrtcapi.quit)     | 退出音视频房间 |

#### 事件通知
| 事件                   |  描述            |
| -------------------- | -------- |
| [onLocalStreamAdd](#onlocalstreamadd)     | 本地视频流新增/更新 |
| [onRemoteStreamUpdate](#onremotestreamupdate)     | 远端视频流新增/更新 |
| [onRemoteStreamRemove](#onremotestreamremove)     | 远端视频流断开 |
| [onWebSocketClose](#onwebsocketclose)     |  websocket 断开 |
| [onRelayTimeout](#onrelaytimeout)     | 视频流 server 超时断开 |
| [onKickout](#onkickout)     | 被踢下线（同一个用户重复登录） |

----

> 如需一些满足一些特定场景的需求，可以了解以下接口。

### [进阶]接口 & 事件通知
#### 接口
| API                   |  描述            |
| -------------------- | -------- |
| [WebRTCAPI.startRTC](#webRTCAPI.startRTC)   | 主动推流才需要用到 |
| [WebRTCAPI.getLocalStream](#webrtcapi.getlocalstream)     | 获取本地音频/音频流 |
| [WebRTCAPI.openVideo](#webrtcapi.openVideo)     | 推流期间重新打开视频采集 |
| [WebRTCAPI.closeVideo](#webrtcapi.closeVideo)     | 推流期间暂时关闭视频采集 |
| [WebRTCAPI.openAudio](#webrtcapi.openAudio)     | 推流期间重新打开音频采集 |
| [WebRTCAPI.closeAudio](#webrtcapi.closeAudio)     | 推流期间暂时关闭音频采集 |
| [WebRTCAPI.changeSpearRole](#webrtcapi.changeSpearRole)     | 切换音视频参数设定 |
| [WebRTCAPI.getVideoDevices](#webrtcapi.getVideoDevices)     | 枚举视频采集设备 |
| [WebRTCAPI.getAudioDevices](#webrtcapi.getAudioDevices)     | 枚举音频采集设备 |
| [WebRTCAPI.chooseVideoDevice](#webrtcapi.chooseVideoDevice)     | 选择视频采集设备 |
| [WebRTCAPI.chooseAudioDevice](#webrtcapi.chooseAudioDevice)     | 选择音频采集设备 |

#### 事件通知
> 以下事件通知

| 事件                   |  描述            |
| -------------------- | -------- |
| [onPeerConnectionAdd](#onpeerconnectionadd)     | PeerConnection 新增通知 ，请确保您已经了解了 peer connection 通知的作用和意义 |


## 使用说明
### WebRTCAPI.fn.detectRTC
#### 具体功能
检测是否支持 WebRTC
```javascript
WebRTCAPI.fn.detectRTC(function(info){
    if( !info.support ) {
        alert('不支持WebRTC')
    }
});
```
#### info 字段

| 字段  | 含义    |  备注|
| ------------------------- | -------- | ---------------------- |
| isTBS      | 是否是TBS |   [什么是TBS](https://x5.tencent.com/tbs/index.html)              |
| TBSversion      | TBS版本号 |                 |
| isTBSValid      | TBS版本号是否支持WebRTC |                 |
| support      | 是否支持WebRTC |  |
| h264Support      | 是否支持H.264 |必须支持H.264 |

-----

### WebRTCAPI
#### 具体功能
初始化 WebRTC

```javascript
var RTC = new WebRTCAPI( options , succ , error)
```
#### 参数

| 参数                   | 类型       | 描述            |
| -------------------- | -------- | ------------- | ---- |
| options         | object | 参数      |
| succ         | function | 成功回调      |
| error         | function | 失败回调      |

#### Options

| 参数               | 类型      | 描述                                       | 备注           |
| ---------------- | ------- | ---------------------------------------- | ------------ |
| **sdkAppId**         | integer | 应用的 sdkappid（如有疑义请看[ 集成SDK ](/document/product/647/16863)）              | 必填           |
| **accountType**      | integer | 账户类型（ 如有疑义请看[ 集成SDK](/document/product/647/16863) )                     | 必填           |
| **userId**           | string  | 用户的唯一标识，也就是我们常说的用户名（如有疑义请看 [集成SDK](/document/product/647/16863)） | 必填           |
| **userSig**          | string  | 必要，身份签名，相当于登录密码的作用 （如有疑义请看[ 集成SDK ](/document/product/647/16863)）                     | 必填           |
| closeLocalMedia | boolean | 是否关闭自动推流（如果置为 true，则在完成加入/建房操作后，不会发起本端的推流，如需推流，需要由业务主动调推流接口 ） | 非必填，默认 false |
| audio            | boolean | 是否启用音频采集                                 | 非必填，默认 true  |
| video            | boolean | 是否启用视频采集                                 | 非必填，默认 true  |

#### 代码示例
```javascript
    var RTC = new WebRTCAPI( {
        "userId": userId,
        "sdkAppId":  sdkappid,
        "accountType":  accountType,
        "userSig": userSig
    } );
```
----
### WebRTCAPI.createRoom
#### 具体功能
创建或进入音视频房间
```javascript
    var RTC = new WebRTCAPI( ... )
    ...
    //注意：这里必须在 WebRTCAPI 的初始化成功的回调中调用
    RTC.createRoom( options, succ , error );
```

#### 参数

| 参数                   | 类型       | 描述            |
| -------------------- | -------- | ------------- |
| options         | object | 参数      |         
| succ         | function | 成功回调      |      
| error         | function | 失败回调      |      

#### Options

| 参数               | 类型      | 描述                                  |
| ---------------- | ------- | ---------------------------------------- |
| **roomid**         | integer | 房间 id          | 必填           |
| **privateMapKey**  | string  | 房间权限key，相当于进入指定房间roomID的钥匙 （如有疑义请看[ 集成SDK ](/document/product/647/16863)）                     | 必填           |
| **role**      | string | 切换画面设定的用户角色[ 控制台 - SPEAR引擎配置 ](https://cloud.tencent.com/document/product/268/10620)                    | 必填           |


#### 代码示例
```javascript
var RTC = new WebRTCAPI({
    "userId": "username",
    "sdkAppId":  1400012345,
    "accountType":  12345,
    "userSig": "xxxxxxxxxxxxxxxxxxxxxxxxx",
}, function(data){
    console.debug( ' 初始化成功 ')
    RTC.createRoom( {
        roomid : 123456,
        privateMapKey: "xxxxxxxxxxxxxxxxxxxxx",
        role : "user",
    }, function(){
        console.debug( ' 进房房间成功 ')
    } ,  function(data){
        console.debug( ' 进入房间失败 ' , data)
    } );
}, function(data){
    console.debug( ' 初始化失败 ' , data)
});
```
---
### WebRTCAPI.quit
#### 具体功能
退出音视频房间
```javascript
    var RTC = new WebRTCAPI( ... )
    ...
    //注意：这里必须在 WebRTCAPI 的初始化成功的回调中调用
    RTC.quit(  succ , error );
```

#### 参数

| 参数               | 类型      | 描述                                       | 备注           |
| -------------------- | -------- | ------------- | ---- |
| succ         | function | 成功回调      |    非必填 |
| error         | function | 失败回调      |非必填 |

#### 代码示例
```javascript
    var RTC = new WebRTCAPI( ... )
    ...
    //注意：这里必须在 WebRTCAPI 的初始化成功的回调中调用
    RTC.quit(  function(){
        //退出成功
    } , function(){
        //退出失败
    } );
```
---

>说明:
事件通知的基本写法如下：
>```javascript
>    var RTC = new WebRTCAPI( { ... } );
>
>    ......
>
>    RTC.on( 'EVENT_NAME' , function(data){
>
>    })
>  ```

### onLocalStreamAdd
#### 具体功能
本地视频流新增/更新
```javascript
    var RTC = new WebRTCAPI( { ... } );

    RTC.on( 'onLocalStreamAdd' , function( data ){
        if( data && data.stream){
            var stream = data.stream
            document.querySelector("#localVideo").srcObject = stream
        }
    })
```
#### data
| 参数                   | 类型       | 描述            |
| -------------------- | -------- | ------------- | ---- |
| stream         | Stream | 本地视频流Stream      |

---

### onRemoteStreamUpdate
#### 具体功能
远端视频流新增/更新

```javascript
    var RTC = new WebRTCAPI( { ... } );

    RTC.on( 'onRemoteStreamUpdate' , function( data ){
        if( data && data.stream){
            var stream = data.stream
            console.debug( data.userId + 'enter this room with unique videoId '+ data.videoId  )
            document.querySelector("#remoteVideo").srcObject = stream
        }else{
            console.debug( 'somebody enter this room without stream' )
        }
    })
```
#### data
| 参数                   | 类型       | 描述            |
| -------------------- | -------- | ------------- | ---- |
| userId     | Stream  | 视频流所属用户的userId（ identifier ）    |
| stream     | Stream  | 视频流 Stream，可能为 null( 每一个用户进来 不管是否推流，都会触发这个回调)  |
| videoId    | string  | 视频流Stream的唯一id ,由 tinyid + "_" + 由随机字符串 组成      |
| videoType: | Integer | 0 : NONE , 1:AUDIO 音频,   2：主路 MAIN   7：辅路 AID |

----

### onRemoteStreamRemove
#### 具体功能
远端视频流断开
```javascript
    var RTC = new WebRTCAPI( { ... } );

    RTC.on( 'onRemoteStreamRemove' , function( data ){
        console.debug( data.userId + ' leave this room with unique videoId '+ data.videoId  )
    })
```

#### data
| 参数                   | 类型       | 描述            |
| -------------------- | -------- | ------------- | ---- |
| userId         | Stream | 远端视频流所属用户的 userId（ identifier ）    |
| videoId         | Stream | 远端视频流 Stream 的唯一 ID    |

-----

### onWebSocketClose
#### 具体功能
websocket 断开
```javascript
    var RTC = new WebRTCAPI( { ... } );

    RTC.on( 'onWebSocketClose' , function( data ){

    })
```
---
### onRelayTimeout
#### 具体功能
视频流server超时断开
```javascript
    var RTC = new WebRTCAPI( { ... } );

    RTC.on( 'onRelayTimeout' , function( data ){
        //视频服务器超时
    })
```
----
### onKickout
#### 具体功能
被踢下线(同一个用户重复登录)
```javascript
    var RTC = new WebRTCAPI( { ... } );

    RTC.on( 'onKickout' , function( data ){
        //退出房间
    })
```

---

> 以下就是进阶的接口。

### WebRTCAPI.startRTC
#### 具体功能
主动发起推流/拉流

```javascript
    var RTC = new WebRTCAPI({ ... });
    ... 

    RTC.startRTC();
```
---

### WebRTCAPI.getLocalStream
#### 具体功能
获取本地音频/音频流
> **注意：**
> 这里的 “ 获取 ”，是指拿到 [ Stream ](https://developer.mozilla.org/en-US/docs/Web/API/MediaStream),而不是调起音视频设备采集，所以需要在成功推流后调用才有意义。
```javascript
    var RTC = new WebRTCAPI({ ... });
    ...

    var stream = RTC.getLocalStream();
    document.getElementById("#localVideo").srcObject = stream
```
---
### WebRTCAPI.closeAudio
#### 具体功能
不采集音频（静音）
```javascript
    var RTC = new WebRTCAPI({ ... });
    ...

    RTC.closeAudio();
```
----
### WebRTCAPI.openAudio
#### 具体功能
采集音频标识（取消静音）
```javascript
    var RTC = new WebRTCAPI({ ... });
    ...

    RTC.openAudio();
```

### WebRTCAPI.closeVideo
#### 具体功能
不采集视频
```javascript
    var RTC = new WebRTCAPI({ ... });
    ...

    RTC.closeVideo();
```
---
### WebRTCAPI.openVideo
#### 具体功能
打开视频采集
> 这里的openVideo是在已经进行音视频推流的时候，关闭了视频的情况下再打开采集。
```javascript
    var RTC = new WebRTCAPI({ ... });
    ...

    RTC.openVideo();
```
---
### WebRTCAPI.getLocalMediaStatus
#### 具体功能
获取当前视频采集配置
```javascript
    var RTC = new WebRTCAPI({ ... });
    ...
    var status = RTC.getLocalMediaStatus();
    //status.video  true | false (表示当前是否采集视频)
    //status.audio true | false (表示当前是否采集音频)
```
----
### WebRTCAPI.changeSpearRole
#### 具体功能
切换画面设定的用户角色
```javascript
    var RTC = new WebRTCAPI({ ... });
    ...
    RTC.changeSpearRole( "role_name" );
    //status.video  true | false (表示当前是否采集视频)
    //status.audio true | false (表示当前是否采集音频)
```


----
### WebRTCAPI.getVideoDevices
#### 具体功能
枚举摄像头
```javascript
    var RTC = new WebRTCAPI({ ... });
    ...
    RTC.getVideoDevices( function(devices){
        //devices 是枚举当前设备的视频输入设备的数组(DeviceObject)
        // 例如 ：[device,device,device]
        // 这些device将在选择摄像头的时候使用
    })
```
---

### WebRTCAPI.chooseVideoDevice
#### 具体功能
选择摄像头
```javascript
/*
 * params:
 *   DeviceObject device
 * return:
 *   null
 */
    var RTC = new WebRTCAPI({ ... });
    ...
    RTC.chooseVideoDevice( device );

```

---

### WebRTCAPI.getAudioDevices
#### 具体功能
枚举麦克风
```javascript
    var RTC = new WebRTCAPI({ ... });
    ...
    RTC.getAudioDevices( function(devices){
        //devices 是枚举当前设备的音频输入设备的数组(DeviceObject)
        // 例如 ：[device,device,device]
        // 这些device将在选择麦克风的时候使用
    })
```

---
### WebRTCAPI.chooseAudioDevice
#### 具体功能
切换画面设定的用户角色
```javascript
/*
 * params:
 *   DeviceObject device
 * return:
 *   null
 */
    var RTC = new WebRTCAPI({ ... });
    ...
    RTC.chooseAudioDevice( device );

```

---

### onPeerConnectionAdd
#### 具体功能
PeerConnection 连接通知

```javascript
    RTC.on( 'onPeerConnectionAdd' , function( info ){ })
```

#### info

| 参数        | 类型     | 描述           |
| --------- | ------ | ------------ |
| userId | String | 连接所属用户用户名 |

#### 代码示例
```javascript
    RTC.on( 'onPeerConnectionAdd' , function( info ){
        //由业务决定，是否要建立peerconnection
        if( info.userId === '指定用户名'){
            WebRTCAPI.startRTC({ userId: info.userId );
        }else{
            console.debug('不建立连接')
        }
    })
```
