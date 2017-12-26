
## 应用云 Crash 服务 Android 接入指南

### 准备工作

在开始使用应用云 Crash 服务前，您需要：

 1. 新建或者打开一个 Android 项目。
 2. 配置了TAC服务框架，配置方式请参见[这里]()

### 集成 Crash 服务到你的应用

#### 通过远程依赖集成 (<font color='red'>推荐</font>)

你需要在 module 下的 build.gradle 添加依赖：

```

dependencies {
    ......
    
    compile 'com.tencent.tac:crash:1.0.0' 

}
```

#### 本地集成

1. 下载 Messaging 服务资源打包文件，并解压。下载资源文件请点击[这里]()。
2. 将资源文件中的 libs 目录拷贝到您的 module 的根目录下。
3. 将解压后的 jniLibs 目录拷贝到您的 module 的 ./source/main 下，这里您可以根据自己的平台来删减 so 文件。
4. 打开您自己 module 下的 AndroidManifest.xml 文件，需要添加如下权限。

```
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.READ_LOGS" />
```

### 配置 Crash 服务实例

在启动 Crash 服务前，您可以在代码中修改 Crash 服务的相关配置。

```
// 每次调用 newDefaultOptions(Context) 方法会新建一个配置对象，如果您使用了多个 TAC 服务，请不要重复调用。
TACApplicationOptions applicationOptions = TACApplicationOptions.newDefaultOptions(this);

// 这里获取 Messaging 服务的配置对象，您可以通过这个对象来配置 Messaging 服务。
TACCrashOptions crashOptions = applicationOptions.sub("crash");
```
请注意，每次调用 newDefaultOptions(Context) 方法会新建一个配置对象，如果您使用了多个 TAC 服务，请不要重复调用。

### 启动 Crash 服务

集成好 Crash 服务后，你可以很方便的启动 Crash 服务，具体代码如下：

```
// 首先获取 TACCrashService 实例
TACCrashService crashService = TACCrashService.getInstance();

// 调用 start 接口启动 Crash 服务，context 这里最好是使用 application context。
crashService.start(context);
```


### 添加符号表和mapping文件上传插件

如果你使用了 so 文件或者对代码进行了混淆，那么需要添加插件来上传符号表和mapping文件。

1、在项目根目录下的 build.gradle 文件中添加依赖

```
buildscript {
	 ......
	 
    dependencies {
        ......
        
        classpath 'com.tencent.tac:crash-plugin:1.0.0'
    }
}
```

2、在 module 下的 build.gradle 文件中添加插件

```
......
 
apply plugin: 'com.tencent.tac.crash'
```
