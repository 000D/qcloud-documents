## 1 总览
腾讯云短信服务接入流程：
![](//mc.qcloudimg.com/static/img/54c5e0236d0a4185dc45b8052ff4a598/image.png)

## 2 注册腾讯云
### 2.1 注册腾讯云账号
1)	注册入口[腾讯云官网-注册入口链接](http://manage.qcloud.com/developerCenter/registUser.php)
2)	验证帐号。使用QQ号码进行验证。
3)	登录验证。登录QQ进行验证。
4)	完善资料。填写帐号相关资料。
5)	邮件激活。点击邮件链接进行激活。
更多详情请参考[注册指南](http://bbs.qcloud.com/thread-2378-1-1.html)
### 2.2 资质审核
1)	完善资料：
[完善资料指南](http://bbs.qcloud.com/forum.php?mod=viewthread&tid=2358&extra=page%3D1)
2)	资质审核：
[资质审核常见问题](http://bbs.qcloud.com/forum.php?mod=viewthread&tid=2364&extra=page%3D1)

## 3 申请短信服务
### 3.1 完善业务资料
1)	[短信服务介绍页](http://cloud.tencent.com/product/sms.html)
![](//mc.qcloudimg.com/static/img/f3d9037d726fd9d022943835feb502f4/image.png)
2)	点击“立即使用”，如果是个人用户或未经过企业资质认证的客户请根据提示进入账户管理完善资料并通过审核，详情请参见[2.2 资质审核](https://cloud.tencent.com/doc/product/382/%E6%8E%A5%E5%85%A5%E6%8C%87%E5%8D%97#2.2-.E8.B5.84.E8.B4.A8.E5.AE.A1.E6.A0.B8)
![](//mc.qcloudimg.com/static/img/0bf4ca3b61db958d5313a48b0c014399/image.png)
### 3.2 进入控制台
资料完善后，通过管理中心导航或控制台链接进入[控制台](http://console.cloud.tencent.com/sms)。

## 4 创建应用
1)	创建应用。进入短信控制台，点击应用列表左上角按钮“创建应用”：
![](//mc.qcloudimg.com/static/img/c996d30ff1ee445283abb063f146cb80/image.png)
进入到创建应用界面，您可以在这里填写应用的相关信息。
![](//mc.qcloudimg.com/static/img/521d83b2d63cd21d7d164e5d5c4b49cf/image.png)
2)	创建应用后，点击“详情”，如下图：
![](//mc.qcloudimg.com/static/img/7ade6588fcf75cb826373f5e98b78456/image.png)
进入应用信息界面，可以获取该应用的SdkAppID和AppKey 。
![](//mc.qcloudimg.com/static/img/08ef51c6f1768f1854ab89742873646d/image.png)
<font color=DarkRed>注：SdkAppID对应的AppKey需要业务方高度保密。</font>

## 5 短信内容配置
### 5.1	配置短信签名
签名定义：短信签名是附加在短信内容中的标识，可以是公司名称或产品名称，用于标识公司或业务，要求2~8个字，例如【腾讯云平台】
![](//mc.qcloudimg.com/static/img/96ab73f1b781c4f28db173548fd699cf/image.png)
配置方法：客户可以在应用详情的“内容设置-短信签名”中创建短信签名，短信签名备案通过后即可使用，您可以通过签名卡片右上角标识获取签名所处审核状态
![](//mc.qcloudimg.com/static/img/41e720ab75ae73623211a439546d7aaf/image.png)
注：短信签名审核，正常情况下半个工作日内完成审核。海外短信签名请在海外文本短信页面下申请。
### 5.2	配置短信内容
内容定义：短信内容即发送短信的完整内容，只有通过审核的内容模板才能发送。
配置方法：客户可以在应用详情的“内容设置-短信正文内容”创建短信内容模版，创建的短信内容模版审核通过后即可使用
![](//mc.qcloudimg.com/static/img/7d3e6b24db524f76a77b6120fe7d7447/image.png)
点击“创建正文模板”按钮进入正文模板创建页面
![](//mc.qcloudimg.com/static/img/a3c934e20e3bc453f9df57b7eafba991/image.png)
注：短信内容模版审核，正常情况下2个小时内完成审核。海外短信模板请在海外文本短信页面下申请。
### 5.3 配置频率限制
为了保障业务和通道安全，减少业务被刷后的经济损失，短信默认的频率限制策略为：
1、同一号码同一内容30秒内最多发送1条；
2、同一手机号一个自然天最多发生10条；
个人客户不提供修改频率限制的权限。企业客户可以在短信控制台的“应用配置-基础配置-短信发送频率”页面，设置或修改相应的频率限制策略：
![](//mc.qcloudimg.com/static/img/7ee8dd6892d2fbb59ffe8959279fa440/image.png)
### 5.4 添加告警联系人
在短信控制台的“应用配置-通知与告警-添加告警联系人”页面，添加告警联系人后，可以及时收到签名、内容模版的审核及频率限制告警通知：
（注：强烈建议使用该功能，这样如果命中了频率限制策略，会收到告警通知，减少业务被刷后的经济损失）
![](//mc.qcloudimg.com/static/img/eb48d970c482cd638c528e9f95e3ab24/image.png)
### 5.5 配置回调
为方便客户精细化了解短信发送相关信息，腾讯云短信服务提供了完善的回调能力。例如配置了短信接收状态回调地址，腾讯云收到运营商回调信息后会及时将回调信息推送到业务指定的回调地址。目前腾讯云短信支持短信状态回调，短信回复回调，语音按键回调。
配置方法：客户可以在应用详情的回调配置选择需要的回调并配置回调地址。
![](//mc.qcloudimg.com/static/img/1539d61a5f836da537dce10f458c6ea8/image.png)

## 6 购买套餐包
国内文本短信采用预付费的方式，进入到[套餐包管理界面](https://console.cloud.tencent.com/sms/packageList)，您可以[购买短信套餐包](https://buy.cloud.tencent.com/sms)。套餐包仅可用于国内文本短信，企业客户使用的语音和国际短信产品目前采用月结后付费的方式。
![](//mc.qcloudimg.com/static/img/ec6d3c61ad1b464b67b32aaab582f1dc/image.png)