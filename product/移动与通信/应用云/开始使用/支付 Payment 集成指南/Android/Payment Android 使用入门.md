# 应用云 Payment 服务 Android 使用入门

## 准备工作

在开始使用应用云 payment 服务前，确保您已经完成：

[安装和配置SDK](https://github.com/tencentyun/qcloud-documents/blob/master/product/%E5%AD%98%E5%82%A8%E4%B8%8ECDN/_Drafts/ApplicationBoard/%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/Core/Android/GettingStarted.md)

## 添加 SDK

如果希望将 payment 库集成至自己的某个项目中，可以通过 gradle 远程依赖或者 jar 包两种方式集成。

### 通过 gradle 远程依赖集成

如果您使用 Android Studio 作为开发工具或者使用 gradle 编译系统，**我们推荐您使用此方式集成依赖**。

#### 1. 使用 jcenter 作为仓库来源

在工程根目录下的 build.gradle 使用 jcenter 作为远程仓库：

```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        ...
    }
}

allprojects {
    repositories {
         jcenter()
    }
}
```

#### 2. 添加 Payment 库依赖

在您的应用级 build.gradle（通常是 app/build.gradle）添加 payment 的依赖：

```
dependencies {
    //增加这行
    compile 'com.tencent.tac:tac-payment:1.0.0'
}
```

然后，点击您 IDE 的 【gradle】 同步按钮，会自动将依赖包同步到本地。

#### 3. 手动添加 QQ 支付渠道包

如果您需要接入 QQ 支付，那么您必须手动添加[mqqopenpay.jar](http://tac-android-libs-1253960454.cosgz.myqcloud.com/jars/mqqopenpay.jar)到您工程的 `libs` 目录。

> 因为 `mqqopenpay.jar` 没有上传到 jcenter 仓库下，因此我们暂时无法自动帮您添加

### 手动集成

如果您无法采用远程依赖的方式，您可以通过以下方式手动集成。

#### 1. 下载服务资源压缩包

1. 下载 [应用云核心框架资源包](http://tac-android-libs-1253960454.cosgz.myqcloud.com/tac-core-1.0.0.zip)，并解压。
2. 下载 [应用云 Payment 资源包](http://tac-android-libs-1253960454.cosgz.myqcloud.com/tac-payment-1.0.0.zip)，并解压。

#### 2. 集成 jar 包

将资源文件中的所有 jar 包拷贝到您工程的 `libs` 目录。

#### 3. 集成 资源文件

1. 将Payment 资源包中的 `assets` 目录下的文件拷贝到您工程的 `assets` 目录下
2. 将Payment 资源包中的 `res` 目录下的所有文件拷贝到您工程的 `res` 目录下，注意 `values.xml` 需要和您原来的字符串文件合并。

#### 4. 修改 AndroidManifest.xml 文件

首先添加如下 `permission` 声明：

```
<!-- 必选权限 -->
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
<uses-permission android:name="android.permission.GET_TASKS"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>

<!-- 可选权限 -->
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/>
<uses-permission android:name="android.permission.READ_PHONE_STATE"/>
```

然后添加如下 `Activity` 声明：

```
<activity
    android:name="com.tencent.openmidas.proxyactivity.APMidasPayProxyActivity"
    android:theme="@android:style/Theme.Translucent.NoTitleBar"
    android:configChanges="orientation|keyboardHidden|screenSize"
    android:process=":openMidas"
    android:screenOrientation="portrait"/>
<activity
    android:name="com.tencent.openmidas.wx.APMidasWXPayActivity"
    android:theme="@android:style/Theme.Translucent.NoTitleBar"
android:process=":openMidas"
android:exported="true" />
<activity
    android:name="com.tencent.openmidas.qq.APMidasQQWalletActivity"
    android:launchMode="singleTop"
    android:theme="@android:style/Theme.Translucent.NoTitleBar"
    android:configChanges="orientation|keyboardHidden"
    android:process=":openMidas"
    android:exported="true" >
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.BROWSABLE"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:scheme="qwallet100703379"/>
    </intent-filter>
</activity>
```
