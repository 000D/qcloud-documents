##  简介
腾讯互动课堂（Tencent Interact Class，TIC）SDK 是一个提供在线教育场景下综合解决方案的 SDK，它对腾讯云已经技术积累多年的`iLiveSDK`、`IMSDK`和`BoardSDK`、`COSSDK`  四个SDK进行了集成封装，提供了多人音视频、多人即时通信、多人互动画板、文档云端转码预览等功能。适用于在线互动课堂、在线会议、你画我猜等场景。

##  开发前必看

### 名词解释
#### 统一名词
请参考 [名词解析](https://cloud.tencent.com/document/product/647/17230)。

#### TIC 特有名词
* roomID：课堂ID，一个课堂的唯一标识，在调用TICSDK的创建房间和加入房间时，由外部传入。

### 接入概览

![](https://main.qcloudimg.com/raw/1924c82283cd5e15da0518d97154e0bb.png)

接入 TICSDK 之后，整体的架构如上图，腾讯云互动课堂解决方案提供了强大的能力支持，在此基础上，客户只需要完成一些必须的工作和自己的业务层逻辑，即可快速搭建起一个全平台的在线互动课堂产品。

介绍下业务侧需要完成的主要工作：
#### 业务侧登录和 userSig/privateMapKey 生成

TICSDK 虽然有登录/登出接口，但是登录时使用的是`identifier`和`userSig`，客户不能直接使用，还需实现业务层的登录逻辑，如图：

![](https://main.qcloudimg.com/raw/e5e4e33ea06db665a249844f928f0094.png)

业务终端先用业务侧的用户名密码登录开发者自己的服务器，然后开发者服务器使用腾讯云提供的工具生成登录TICSDK所需要的`userSig`（这一步相当于在腾讯云后台注册了一个账号，用户名为业务侧的用户名，密码为生成的`userSig`），并将其返回终端，终端拿到后就可以使用用户名(即`identifier`)和`userSig`去调用 TICSDK 的登录接口，最后腾讯云后台负责校验登录信息的正确性，返回登录结果。

开发者需要做的就是图片`开发者服务器`上标注的，调用工具生成 userSig，并返回给终端的逻辑（通常做法为在业务侧登录接口回包中返回`userSig`）。

当然，如果在开发调试阶段，也可以使用实时音视频控制台生成`identifier`和`userSig`用于测试开发。

**privateMapKey** (房间票据)，相当于进入指定房间 (roomId) 的钥匙。由开发者业务服务器签发，传给客户端，客户端在调用 TICSDK 进房接口时填入（必填项）。

详情请参考 [生成签名](https://cloud.tencent.com/document/product/647/17275)。

#### 界面布局

当然，客户还需要自己完成自己的产品的界面布局，但是为了进一步减小开发者的接入开发成本，我们之后有计划开发一套在线课堂通用的 UI 模板，敬请期待。

#### 课堂管理
TICSDK有创建课堂的接口，但是课堂 ID (roomID) 由外面传入，TICSDK 内部并不维护`roomID`，所以业务在调用 TICSDK 接口创建课堂时，需要维护`roomID`的唯一性（创建重复的课堂 TICSDK 将会返回创建失败及错误信息）。

另根据具体业务需要，业务侧可能还需要维护一个当前的课堂列表。

#### COS 签名生成
如果客户业务需要在课堂展示本地文档，则需要用到腾讯云对象存储服务（COS），在对对象存储进行操作时，需要 COS 签名用于检验身份，这个 COS 签名需要开发者的后台生成，传给客户端，详情请参考 [业务后台接入腾讯云服务](/document/product/680/17910)。


##  客户端 SDK 文档

* [Windows SDK](/document/product/680/17883)
* [Web SDK](/document/product/680/17887)
* [Android SDK](/document/product/680/17881)
* [iOS SDK](/document/product/680/17891)

