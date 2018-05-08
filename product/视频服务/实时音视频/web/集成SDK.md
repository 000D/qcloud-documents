## 预备知识
###  H5 支持的平台

| 操作系统平台  | 浏览器/webview  | 版本要求  |  备注|
| ------------------------- | -------- | ---------------------- |------- |
| iOS          | Safari ( Only ) | 11.1.2 | 由于 Safari 的实现仍有偶现的 bug，产品化方案建议先规避，待苹果解决后再使用<br > 对于iOS可以考虑使用我们的[小程序解决方案](/document/product/647/17000) |
| Android      | TBS | 43600                |   [TBS 介绍](http://x5.tencent.com/)   |
| Android      | Chrome | 60+               | 需要支持 H264  |
| Mac          | Chrome | 47+                |      |
| Windows(PC)  | Chrome | 52+                |  | |

> 基于 TBS 内核的 webview，需满足版本 >= 43600，我们的[ 示例代码 ](/document/product/647/16924#.E6.A3.80.E6.B5.8B.E6.98.AF.E5.90.A6.E6.94.AF.E6.8C.81webrtc)中有获取 TBS 版本的方法。


## 基本名词

* 腾讯云账户信息

| 名词          | 含义                                       |
| ----------- | ---------------------------------------- |
| appid       | 商户在腾讯云注册后，会产生一个在腾讯云的唯一标示，称为 appid         |
| sdkappid    | 商户可以在互动直播控制台创建多个应用，以对应自己不同的 App。每个应用都会有一个 sdkappid 来标示 |
| accounttype | 每个sdkappid在账户体系页面都会有一个accounttype参数，登录的时候会用到 |

> appid 是 125xxxxxxx 的数字，可以在[ 实时音视频控制台 ](https://console.cloud.tencent.com/ilvb)的顶部获取。
> sdkappid 是  14000xxxxx 的数字 ，[ 实时音视频控制台 ](https://console.cloud.tencent.com/ilvb)创建应用，在【App 基础设置】下可查看。
> accounttype 需要您在 [ 实时音视频控制台 ](https://console.cloud.tencent.com/ilvb)创建应用后，查看 【App 基础设置】-【帐号集成体系】中找到（ 如果没有，编辑保存即可 ）


* 用户信息

| 名词      | 含义                                       |
| ------- | ---------------------------------------- |
| userId  | 也叫 identifier，在 App 中标示用户的身份,一般大家都把他叫做用户名    |
| userSig | 身份签名，相当于登录密码的作用。每个 userId 都有一个有一定期限的签名，在请求的时候带上，以便腾讯云鉴别用户的身份。 |


* 视频通话信息

| 名词      | 含义                                       |
| ------- | ---------------------------------------- |
| spear 角色 | 一个视频用户的分辨率、码率、帧率等信息的配置信息集合名，可以在应用的 Spear 引擎页面维护 |
| roomId  | 用来标识一个视频通话房间。roomId 相同的用户才能相互看到             |
| privateMapKey  | 房间权限key，相当于进入指定房间roomId的钥匙             |

下载 [sign_src.zip](http://dldir1.qq.com/hudongzhibo/mlvb/sign_src_v1.0.zip) 可以获得服务端签发 userSig 和 privateMapKey 的计算代码（生成 userSig 和 privateMapKey 的签名算法是 **ECDSA-SHA256**）。

## 接入准备工作

1. 注册腾讯云帐号，申请开通互动直播业务；
2. 在互动直播页面，新建应用。得到 sdkappid 和 accounttype；
3. 参考[ usersig 计算文档 ](https://cloud.tencent.com/document/product/268/7656)，计算出测试用户名的 usersig；
4. 如果所在网络有防火墙，请确定有开放以下端口：

| 协议      | 端口号                                       |
| ------- | ---------------------------------------- |
| TCP | 8687 、443 |
| UDP  | 8000 、 8800 <br/>42000、42200 <br/>42201、42800 <br/>52000、52200 |

使用 CDN 引入SDK。
### 在页面中引入 WebRTCAPI.min.js

```html
<script src="https://sqimg.qq.com/expert_qq/webrtc/2.2/WebRTCAPI.min.js"></script>
```


