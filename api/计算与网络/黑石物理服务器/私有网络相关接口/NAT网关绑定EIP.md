## 功能描述
EipBindBmNatGateway 接口用于将EIP绑定到黑石NAT网关，NAT网关使用该EIP作为访问外网的源IP地址，将流量发送到Internet

接口请求域名：bmvpc.api.qcloud.com

## 请求
### 请求示例
```
GET https://bmvpc.api.qcloud.com/v2/index.php?Action=EipBindBmNatGateway
    &<公共请求参数>
    &natId=<NAT网关ID>
    &vpcId=<vpc网络ID>
    &autoAllocEipNum=<分配IP的个数>
```
### 请求参数
以下请求参数列表仅列出了接口请求参数，正式调用时需要加上公共请求参数，见<a href="/doc/api/372/4153" title="公共请求参数">公共请求参数</a>页面。其中，此接口的Action字段为EipBindNatGateway。

| 参数名称 | 必选  | 类型 | 描述 |
|---------|---------|---------|---------|
<<<<<<< HEAD
| natId | 是 | String | 黑石网关统一ID，例如：nat-df5dfd |
| vpcId | 是 | String | 私有网络ID值，可使用vpcId或unVpcId，建议使用unVpcId，例如：vpc-kd7d06of，可通过<a href="https://www.qcloud.com/document/api/386/6646" title="DescribeBmVpcEx">DescribeBmVpcEx</a>接口查询。 |
| assignedEipSet.n | 否 | Array | 弹性IP。assignedEipSet 和 autoAllocEipNum 这两个入参需至少传一个，例如：assignedEipSet.0=183.23.0.0.1 |
| autoAllocEipNum | 否 | Int | 需要新申请的弹性IP个数, 取值范围[0, 4]。assignedEipSet 和 autoAllocEipNum 这两个入参需至少传一个|
=======
| natId | 是 | string | 黑石网关统一ID，例如：nat-df5dfd |
| vpcId | 是 | string | 私有网络ID值，可使用vpcId或unVpcId，建议使用unVpcId，例如：vpc-kd7d06of，可通过<a href="https://cloud.tencent.com/document/api/386/6646" title="DescribeBmVpcEx">DescribeBmVpcEx</a>接口查询。 |
| assignedEipSet.n | 否 | array | 弹性IP。assignedEipSet 和 autoAllocEipNum 这两个入参需至少传一个，例如：assignedEipSet.0=183.23.0.0.1 |
| autoAllocEipNum | 否 | int | 需要新申请的弹性IP个数, 取值范围[0, 4]。assignedEipSet 和 autoAllocEipNum 这两个入参需至少传一个|
>>>>>>> origin/master


## 响应
### 响应示例
```
{
	"code": 0,
	"message": "",
	"data": {
		"taskId": <NAT异步任务ID>
	}
}
```
### 响应参数

| 参数名称 | 类型 | 描述 |
|---------|---------|---------|
| code | Int | 错误码。0: 成功, 其他值: 失败|
| message | String | 错误信息|
<<<<<<< HEAD
| data | Array | data中包含操作的任务ID，创建结果可调用<a href="https://www.qcloud.com/document/api/386/9356" title="查询NAT网关操作状态">查询NAT网关操作状态</a>查询 |
=======
| data | Array | 返回操作的任务ID，创建结果可调用<a href="https://cloud.tencent.com/document/api/386/9356" title="查询NAT网关操作状态">查询NAT网关操作状态</a>查询 |
>>>>>>> origin/master

## 错误码
 以下错误码表仅列出了该接口的业务逻辑错误码，更多公共错误码详见<a href="https://cloud.tencent.com/doc/api/245/4924" title="VPC错误码">VPC错误码</a>。
 
| 错误代码 | 英文提示 | 错误描述 |
|---------|---------|---------|
| -3047 | InvalidBmVpc.NotFound | 无效的VPC。VPC资源不存在，请再次核实您输入的资源信息是否正确，可通过<a href="https://cloud.tencent.com/document/api/386/6646" title="DescribeBmVpcEx">DescribeBmVpcEx</a>接口查询VPC。 |
| 13014 | BmVpcNat.NotFound | 无效的NAT网关，NAT网关资源不存在。请再次核实您输入的资源信息是否正确，可通过DescribeBmNatGateway接口查询NAT网关。 |
| 13010 | BmVpcNat.InvalidEip | 绑定NAT网关的弹性IP不存在。 |
| 13011 | BmVpcNat.InvalidEipVpcId | 弹性IP所属VPC与NAT网关不一致。 |
| 13013 | BmVpcNat.EipUsed | 绑定NAT网关的弹性IP已被使用。 |
| 13015 | BmVpcNat.EipLimitExceeded | 绑定NAT网关的弹性IP达到上限。 |


## 实际案例
### 请求
```
GET https://bmvpc.api.qcloud.com/v2/index.php?
	Action=EipBindBmNatGateway
	&SecretId=AKID1ub7R1JoyBF7nHqjk7IH8nGWaR6Yezwd
	&Nonce=4557
	&Timestamp=1507692902
	&Region=gz
	&vpcId=300006
	&natId=nat-et8e970y
	&autoAllocEipNum=1
	&Signature=xhpWkOBXHyEdddxK2KIH%2F14bMrc%3D
```

### 响应
```
{
	"code": 0,
	"message": "",
	"data": {
		"taskId": 9641
	}
}
```