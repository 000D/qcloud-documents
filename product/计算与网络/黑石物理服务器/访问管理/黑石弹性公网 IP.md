## 概述
黑石弹性公网IP支持细化到实例级别的权限管理，你可以为人员分配管理特定弹性公网IP实例的权限；或者属于特定VPC的所有弹性公网IP的管理权限。

## 预设策略
预设策略，能帮助你快速授权，而不需要编写策略，但授权粒度会粗些，以下是黑石弹性公网IP的两个预设策略，分别为：
<table>
<tr>
<th>预设策略名</th>
<th>授权范围描述</th>

</tr>
<tr>
<td>QcloudBMEIPFullAccess</td>
<td>关联后，获得所有黑石弹性公网IP实例的增、删、改、查等操作的权限</td>
</tr>

<tr>
<td>QcloudBMEIPReadOnlyAccess</td>
<td>关联后，只能获得查询黑石弹性公网IP列表及基本信息的权限</td>
</tr>

</table>


## Action、Resource、Condtion列表
以下表格，罗列了在配置黑石弹性公网IP的策略时，需要用到的action、resource、condition。相关概念请参考《[访问管理](https://cloud.tencent.com/document/product/598/10603"访问管理")》章节

Action，即操作，对应的是API。编写策略时，你可以复制表格里内容并粘贴在Action字段中。关联该策略后，即可获得特定API的调用权限<br/>

Resource，即云资源，当列表中ACtion的鉴权参数不为空时，则表示在调用API需要指定云资源，否则则不需要指定。编写策略时，你可以复制表格里内容并粘贴在策略生成器的Resource字段中，但请记得替换$eipId、$InstanceId为真实的实例ID；关联该策略后，即可获得特定资源的操作权限<br/>

注意：
>部份API鉴权时需要两种类型的实例ID，例如绑定EIP，分别需要被绑定的黑石服务器以及用于绑定的黑石弹性公网IP的实例ID，这时>
>需要把两种云产品的资源描述都写在Resource里*


Condition,即生效条件。换句话说Action和Resource需要在特定的生效条件下，才能鉴权通过。你可以灵活使用condtion以做到VPC或者Subnet粒度的权限管理，比如授权人员管理特定VPC内的所有黑石服务器

注意：
>特别说明：Describe*或者Get*指查询操作，比如拉取多个实例详情等，查询操作鉴权通过后可能会把所有实例信息都返回，而无法区别哪些是有权限哪些是没有权限的实例。但再修改、删除实例时，会再次鉴权。

<table>
<tr>
<th>Action</th>
<th>鉴权参数</th>
	<th>功能描述</th>
	<th>条件密钥</th>
</tr>

<tr>
<td>bmeip:EipBmUnBindVpcIp</td>
<td>qcs::bmeip:::eipId/$eipId</td>
	<td>黑石EIP解绑VPCIP虚拟机或者托管</td>
	<td>bmvpc:unVpcId</td>
	
</tr>

<tr>
<td>bmeip:EipBmBindVpcIp</td>
<td>qcs::bmeip:::eipId/$eipId</td>
	<td>黑石EIP绑定VPCIP（虚拟机或者托管）</td>
	<td>bmvpc:unVpcId</td>
	
</tr>

<tr>
<td>bm:UnbindEip</td>
<td>qcs::bmeip:::eipId/$eipId<br/>qcs::bm:::instance/$InstanceId</td>
	<td>解绑黑石EIP</td>
	<td>bmvpc:unVpcId</td>
	
</tr>

<tr>
<td>bm:BindEip</td>
<td>qcs::bmeip:::eipId/$eipId<br/>qcs::bm:::instance/$InstanceId</td>
	<td>绑定黑石EIP</td>
	<td>bmvpc:unVpcId</td>
	
</tr>

<tr>
<td>bmeip:EipBmModifyCharge</td>
<td>qcs::bmeip:::eipId/$eipId</td>
	<td>黑石EIP修改计费方式</td>
	<td>bmvpc:unVpcId</td>
	
</tr>

<tr>
<td>bmeip:ModifyEipAlias</td>
<td>qcs::bmeip:::eipId/$eipId</td>
	<td>更新黑石EIP名称</td>
	<td>bmvpc:unVpcId</td>
	
</tr>



<tr>
<td>bmeip:EipBmDelete</td>
<td>qcs::bmeip:::eipId/$eipId</td>
	<td>释放黑石EIP</td>
	<td>bmvpc:unVpcId</td>
	
</tr>

<tr>
<td>bmeip:EipBmApply</td>
<td>qcs::bmvpc:::unVpcId/vpc-xxx</td>
	<td>创建黑石EIP</td>
	<td></td>	
</tr>

<tr>
<td>bmeip:DescribeEipBm</td>
<td></td>
	<td>黑石EIP查询接口</td>
	<td></td>
	
</tr>

</table>


## Condition(生效条件）

灵活使用Condtion，即可做到Vpc粒度的权限管理，比如授权管理特定Vpc内的黑石弹性公网IP实例

>在使用Condtion时，做到Vpc粒度的授权，策略的Resource字段建议只需填写"*"


## 书写规范

```
"condition":
{
"Option1":{"key1":["value1","value2"]),"key2":["value1","value2"])},
"Option2":{"key1":["value1","value2"]),"key2":["value1","value2"])}
}
```

Option即操作符，理解为传入的鉴权参数和key的运算规则<br/>
Key和Value是对应的，以下是对应关系。传入的鉴权参数经过运算后应该满足key和value的要求

<table>
<tr><th>key</th><th>value</th></tr>
<tr><td>bmvpc：unVpcId</td><td>vpc-yyyyyy(Vpc的实例Id)</td></tr>
</table>

### 操作符(Option)
黑石弹性公网IP只推荐使用`for_all_value:string_equal_if_exist`</br>

for\_all\_value:string_equal\_if\_exist，用于condition有一个key多个value的情况key:value1,value2，可以做到多个vpc或者subnet的授权


### 例子
策略如下：

```
{
"version":"2.0",
"statement":[
{
"effect":"allow",
"action":[
"bmeip:EipBmModifyCharge"
],
"resource":[
"*"
],
"condition":{
"for_all_value:string_equal_if_exist":{
"bmvpc:unVpcId":"vpc-34cxlz7z"
}
}
}
]
}
```

场景：调用EipBmModifyCharge修改vpc-34cxlz7z的任一EIP实例的别名。<br/>

评估逻辑：
1.鉴权逻辑关联了effect:allow的策略且action:bm:EipBmModifyCharge和resource:*，即允许修改任一实例的别名。<br/>
2.但前提是，实例要在vpc-34cxlz7z里才能鉴权通过<br/>



## 最佳实践
本章节，我们举例两个场景的策略内容和评估逻辑，帮助你了解如何实现黑石服务器的权限分配

场景1：授权释放eip-adt6pq7f
场景2：授权绑定vpc-34cxlz7z和vpc-muinpf9p里内所有的物理服务器和EIP

### 场景1
策略如下：
```
{
"version":"2.0",
"statement":[
{
"effect":"allow",
"action":[
"bmeip:EipBmDelete"
],
"resource":[
"qcs::bmeip:::eipId/eip-adt6pq7f"
]
}
]
}
```

评估逻辑：

当调用EipBmDelete时，CAM会判断传入的EipId是否为eip-adt6pq7f，【是】则鉴权通过；【否】则鉴权失败


### 场景2
策略如下：
```
{
"version":"2.0",
"statement":[
{
"effect":"allow",
"action":[
"bm:BindEip",
"bm:UnbindEip"
],
"resource":[
"*"
],
"condition":{
"for_all_value:string_equal_if_exist":{
"bmvpc:unVpcId":[
"vpc-34cxlz7z",
"vpc-muinpf9p"
]
}
}
}
]
}
```

评估逻辑：
当调用BindEip时，CAM会对传入的instanceId和EipID做鉴权，发现满足resource（*）的要求。
但要求instanceId和EipID在vpc-34cxlz7z或者vpc-muinpf9p里，【是】则鉴权通过；【否】则鉴权失败

