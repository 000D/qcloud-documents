## 1. 接口描述
 
本接口（DescribeBmSubnetIps）用于查询黑石VPC子网IP分配列表。
接口请求域名：<font style="color:red">bmvpc.api.qcloud.com</font>


## 2. 输入参数
 以下请求参数列表仅列出了接口请求参数，正式调用时需要加上公共请求参数，见<a href="/doc/api/372/4153" title="公共请求参数">公共请求参数</a>页面。其中，此接口的Action字段为DescribeBmSubnetIps。

| 参数名称 | 是否必选  | 类型 | 描述 |
|---------|---------|---------|---------|
| vpcId | 是 | String | 子网所属的私有网络ID值，可使用vpcId或unVpcId，建议使用unVpcId，例如：vpc-kd7d06of。可通过DescribeBmVpcEx接口查询。 |
| subnetId | 是 | String | 要删除的子网ID值，可使用subnetId或unSubnetId，建议使用unsubnetId，例如：subnet-k20jbhp0。可通过DescribeBmSubnetEx接口查询。 |
 

## 3. 输出参数

| 参数名称 | 类型 | 描述 |
|---------|---------|---------|
| code | Int | 公共错误码, 0表示成功，其他值表示失败。详见错误码页面的<a href="https://cloud.tencent.com/doc/api/372/%E9%94%99%E8%AF%AF%E7%A0%81#1.E3.80.81.E5.85.AC.E5.85.B1.E9.94.99.E8.AF.AF.E7.A0.81" title="公共错误码">公共错误码</a>。|
| message | String | 模块错误信息描述，与接口相关。|
| data.cpmSet | Array | 物理机类型IP信息。|
| data.vmSet | Array | 虚拟机类型IP信息。|
| data.tgSet | Array | 托管机器类型IP信息。|

  ## 4. 错误码表
 
| 错误代码 |英文提示| 描述 |
|--------|---------|---------|
| -3047  | InvalidBmVpc.NotFound | 无效的VPC。VPC资源不存在，请再次核实您输入的资源信息是否正确，可通过DescribeBmVpcEx接口查询VPC。 |
| -3001  | InvalidInputParams | 输入参数不符合指定格式。 |
| -3051  | BmVpc.SubnetNotExist | 无效的子网。子网资源不存在，请再次核实您输入的资源信息是否正确，可通过DescribeBmSubnetEx接口查询子网。 |


## 5. 示例
 
输入
```

  https://vpc.api.qcloud.com/v2/index.php?Action=DescribeBmSubnetIps
	&<公共请求参数>
	&vpcId=vpc-kd7d06of
    &subnetId=subnet-1so5ae8m
```

输出
```

{
    "code": 0,
    "message": "",
    "codeDesc": "Success",
    "data": {
        "cpmSet": [
            "10.10.1.2"
        ],
        "vmSet": [
            "10.10.1.5"
        ],
        "tgSet": [
			"10.10.1.10"
		]
    }
}

```

