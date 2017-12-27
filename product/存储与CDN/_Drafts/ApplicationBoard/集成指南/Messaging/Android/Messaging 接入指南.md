
## 应用云 Messaging 服务 Android 接入指南

### 准备工作

在开始使用应用云 Messaging 服务前，您需要：

 1. 新建或者打开一个 Android 项目。
 2. 配置了TAC服务框架，配置方式请参见[这里]()


### 集成 Messaging 服务到你的应用

#### 通过远程依赖集成 (<font color='red'>推荐</font>)

你需要在 module 下的 build.gradle 文件中添加如下内容：

```
android {
    ......
    defaultConfig {

        // 官网上注册的包名。注意 application ID 和当前的应用包名以及官网上注册应用的包名必须一致。
        applicationId "你的包名"
        ......
    }
    ......
}

dependencies {
    ......

    compile 'com.tencent.tac:messaging:1.0.0'
}

```

#### 本地集成

1. 下载 Messaging 服务资源打包文件，并解压。下载资源文件请点击[这里]()。
2. 将资源文件中的 libs 目录拷贝到您的 module 的根目录下。
3. 将解压后的 jniLibs 目录拷贝到您的 module 的 ./source/main 下，这里您可以根据自己的平台来删减 so 文件。
4. 打开您自己 module 下的 AndroidManifest.xml 文件，然后按照下载的资源文件中的 AndroidManifest.xml 作为范例来修改。

### 配置 Messaging 服务实例

在启动Messaging 服务前，您可以在代码中修改 Messaging 服务的相关配置。

```
// 每次调用 newDefaultOptions(Context) 方法会新建一个配置对象，如果您使用了多个 TAC 服务，请不要重复调用。
TACApplicationOptions applicationOptions = TACApplicationOptions.newDefaultOptions(this);

// 这里获取 Messaging 服务的配置对象，您可以通过这个对象来配置 Messaging 服务。
TACMessagingOptions messagingOptions = applicationOptions.sub("messaging");

```
请注意，每次调用 newDefaultOptions(Context) 方法会新建一个配置对象，如果您使用了多个 TAC 服务，请不要重复调用。

### 注册 Messaging 服务回调

通过注册 Messaging 服务广播接收器，你可以收到 Messaging 服务的通知。你需要继承 TACMessagingReceiver 类，然后将子类在 manifest.xml 文件中进行如下注册：

```

<receiver android:name="TACMessagingReceiver子类，如com.tencent.tac.tacmessaging.MyReceiver">
	<intent-filter>
	    <action android:name="com.tencent.tac.messaging.action.CALLBACK" />
	</intent-filter>
</receiver>

```

注册后的通知如下：

```
/**
 * 注册结果回调。
 */
void onRegisterResult(Context context, int errorCode, TACMessagingToken token);

/**
 * 收到应用内消息回调。
 */
void onTextMessage(Context context, TACMessagingText message);

/**
 * 收到通知消息。
 */
void onNotificationShowed(Context context, TACNotification notification, int notificationId);

/**
 * 通知消息被处理。
 */
void onNotificationClicked(Context context, TACNotification notification, long actionType);

/**
 * 反注册结果回调
 */
void onUnregisterResult(Context context, int code);

```

### 启动 Messaging 服务

集成好 Messaging 服务后，需要您在 Application 的 onCreate() 方法中启动服务，具体代码如下：

```
// 首先获取 TACMessagingService 实例
TACMessagingService messagingService = TACMessagingService.getInstance();

// 调用 start 接口启动 Messaging 服务，context 这里最好使用 application context。
messagingService.start(context);

```

### 停止 Messaging 服务

停止 Messaging 服务，建议在不需要接收推送的时候调用。

```
messagingService.stop(context);

```

