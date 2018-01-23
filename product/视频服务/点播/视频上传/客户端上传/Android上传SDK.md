对于在Android平台上传视频的场景，腾讯云点播提供了Android上传代码来实现。上传的流程可以参见[客户端上传指引](/document/product/266/9219)。

## 源代码下载

您可以在腾讯云官网更新 [Android上传demo+源代码](http://ugcupload-1252463788.file.myqcloud.com/LiteAVSDK_UGC_Upload_Android.zip)。
下载完的zip包解压后可以看到上传demo（Demo）目录，上传相关源代码在Demo/app/src/main/java/com/tencent/ugcupload/demo/videoupload目录下。

##  集成上传库和源代码

拷贝上传源代码目录Demo/app/src/main/java/com/tencent/ugcupload/demo/videoupload到您的工程目录中，需要手动修改一下package名。

将Demo/app/libs/upload目录下的所有jar包集成到您的项目中，建议您保留upload目录结构，方便以后对库进行更新。

依赖库说明：

| jar文件                       | 说明                                       |
| --------------------------- | ---------------------------------------- |
| cos-xml-android-sdk-1.2.jar | 腾讯云对象存储服务（COS）的文件上传包， 此组件用于短视频上传(TXUGCPublish)功能 |
| qcloud-core-1.2.jar         | 腾讯云对象存储服务（COS）的文件上传包， 此组件用于短视频上传(TXUGCPublish)功能 |
| okhttp-3.8.1.jar            | 一款优秀的开源 http 组件                          |
| okio-1.13.0.jar             | 一款优秀的开源网络 I/O 组件                         |
| xstream-1.4.7.jar           | 一款优秀的开源序列化组件                             |
| fastjson-1.1.62.android.jar | 一款优秀的开源json组件                            |


使用短视频上传需要网络、存储等相关的一些访问权限，可在 AndroidManifest.xml 中增加如下权限声明：

```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```

##  简单视频上传

### 初始化一个上传对象

```java
TXUGCPublish mVideoPublish = new TXUGCPublish(this.getApplicationContext(), "carol_android")
```

### 设置上传对象的回调

```java
mVideoPublish.setListener(new TXUGCPublishTypeDef.ITXVideoPublishListener() {
    @Override
    public void onPublishProgress(long uploadBytes, long totalBytes) {
        mProgress.setProgress((int) (100*uploadBytes/totalBytes));
    }

    @Override
    public void onPublishComplete(TXUGCPublishTypeDef.TXPublishResult result) {
        mResultMsg.setText(result.retCode + " Msg:" + (result.retCode == 0 ? result.videoURL : result.descMsg));
    }
});
```

### 构造上传参数

```java
TXUGCPublishTypeDef.TXPublishParam param = new TXUGCPublishTypeDef.TXPublishParam();
// signature计算规则可参考[客户端上传签名](/document/product/266/9221)
param.signature = "xxx";
param.videoPath = "xxx";
```

### 调用上传

```java
int publishCode = mVideoPublish.publishVideo(param);
```

## 高级功能

### 携带封面

在上传参数中带上封面路径即可。

```java
TXUGCPublishTypeDef.TXPublishParam param = new TXUGCPublishTypeDef.TXPublishParam();
// signature计算规则可参考[客户端上传签名](/document/product/266/9221)
param.signature = "xxx";
param.videoPath = "xxx";
param.coverPath = "xxx";
```

### 取消、恢复上传

取消上传，调用`TXUGCPublish`的`canclePublish()`。

```java
mVideoPublish.canclePublish();
```

恢复上传，用相同的上传参数（视频路径和封面路径不变）再调用一次`TXUGCPublish`的`publishVideo`。

### 断点续传

在视频上传过程中，点播支持断点续传，即当上传意外终止时，用户再次上传该文件，可以从中断处继续上传，减少重复上传时间。断点续传的有效时间是 1 天，即同一个视频上传被中断，那么 1 天内再次上传可以直接从断点处上传，超过 1 天则默认会重新上传完整视频。
上传参数中的`enableResume`为断点续传开关，默认是开启的。


## 接口描述

初始化上传对象`TXUGCPublish`

| 参数名称      | 参数描述                   | 类型      | 必填   |
| --------- | ---------------------- | ------- | ---- |
| context   | application 上下文        | Context | 是    |
| customKey | 用于区分不同的用户，建议使用app的账号id | String  | 否    |

上传`TXUGCPublish.publishVideo`
| 参数名称  | 参数描述 | 类型                                 | 必填   |
| ----- | ---- | ---------------------------------- | ---- |
| param | 上传参数 | TXUGCPublishTypeDef.TXPublishParam | 是    |

上传参数`TXUGCPublishTypeDef.TXPublishParam`

| 参数名称         | 参数描述                               | 类型      | 必填   |
| ------------ | ---------------------------------- | ------- | ---- |
| signature    | [点播签名](/document/product/266/9221) | String  | 是    |
| videoPath    | 本地视频文件路径                           | String  | 是    |
| coverPath    | 本地封面文件路径，默认不带封面文件                  | String  | 否    |
| enableResume | 是否启动断点续传，默认开启                      | boolean | 否    |

设置上传回调`TXUGCPublish.setListener`
| 参数名称     | 参数描述        | 类型                                       | 必填   |
| -------- | ----------- | ---------------------------------------- | ---- |
| listener | 上传进度和结果回调监听 | TXUGCPublishTypeDef.ITXVideoPublishListener | 是    |


进度回调`TXUGCPublishTypeDef.ITXVideoPublishListener.onPublishProgress`
| 变量名称        | 变量描述     | 类型   |
| ----------- | -------- | ---- |
| uploadBytes | 已经上传的字节数 | long |
| totalBytes  | 总字节数     | long |

结果回调`TXUGCPublishTypeDef.ITXVideoPublishListener.onPublishComplete`
| 变量名称   | 变量描述 | 类型                                  |
| ------ | ---- | ----------------------------------- |
| result | 上传结果 | TXUGCPublishTypeDef.TXPublishResult |

上传结果`TXUGCPublishTypeDef.TXPublishResult`
| 成员变量名称   | 变量说明      | 类型     |
| -------- | --------- | ------ |
| retCode  | 结果码       | int    |
| descMsg  | 上传失败的错误描述 | String |
| videoId  | 点播视频文件Id  | String |
| videoURL | 视频存储地址    | String |
| coverURL | 封面存储地址    | String |


## 错误码

SDK 通过 `TXUGCPublishTypeDef.TXVideoPublishListener` 接口来监听短视频上传相关的状态。因此，可以利用 `TXUGCPublishTypeDef.TXPublishResult` 中的 `retCode` 来确认视频上传的情况。

| 状态码  | 在 TVCConstants 中所对应的常量         | 含义                     |
| :--: | :----------------------------- | :--------------------- |
|  0   | NO_ERROR                       | 上传成功                   |
| 1001 | ERR_UGC_REQUEST_FAILED         | 请求上传失败                 |
| 1002 | ERR_UGC_PARSE_FAILED           | 请求信息解析失败               |
| 1003 | ERR_UPLOAD_VIDEO_FAILED        | 上传视频失败                 |
| 1004 | ERR_UPLOAD_COVER_FAILED        | 上传封面失败                 |
| 1005 | ERR_UGC_FINISH_REQUEST_FAILED  | 结束上传请求失败               |
| 1006 | ERR_UGC_FINISH_RESPONSE_FAILED | 结束上传响应错误               |
| 1007 | ERR_CLIENT_BUSY                | 客户端正忙(对象无法处理更多请求)      |
| 1008 | ERR_FILE_NOEXIT                | 上传文件不存在                |
| 1009 | ERR_UGC_PUBLISHING             | 视频正在上传中                |
| 1010 | ERR_UGC_INVALID_PARAM          | 上传参数为空                 |
| 1012 | ERR_UGC_INVALID_SIGNATURE      | 视频上传signature为空        |
| 1013 | ERR_UGC_INVALID_VIDOPATH       | 视频文件的路径为空              |
| 1014 | ERR_UGC_INVALID_VIDEO_FILE     | 当前路径下视频文件不存在           |
| 1015 | ERR_UGC_FILE_NAME              | 视频上传文件名太长（超过40）或含有特殊字符 |
| 1016 | ERR_UGC_INVALID_COVER_PATH     | 视频文件封面路径不对，文件不存在       |