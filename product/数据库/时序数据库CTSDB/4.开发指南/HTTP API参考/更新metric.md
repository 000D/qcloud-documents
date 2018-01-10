### 请求地址 ###
地址为实例的IP和PORT，可从控制台获取到，例如10.13.20.15:9200
### 请求路径和方法 ###
路径：`/_metric/${metric_name}/update`，`${metric_name}`为metric的名称<br/>
方法：PUT
### 请求参数 ###
无
### 请求内容 ###
请求内容包含 tags、time、fields、options 等字段，均为map类型，选填。具体如下：<br/>
tags：允许新增维度字段，但系统不会删除原有字段<br/>
time：其中的name字段不能修改，format字段允许修改<br/>
fields：允许新增新的指标字段，但系统不会删除原有字段<br/>
options属性列举如下：

| 属性名称        | 必选            | 类型            | 描述            |
|---------|---------|---------|---------|
| expire_day     | 否              | string          | 数据过期时间，过期后数据自动清理，缺省情况下永不过期 |
| refresh_interval | 否              | string          | 数据刷新频率，写入的数据从内存刷新到磁盘后可查询，默认为10秒 |
| number_of_shards | 否              | string          | 表分片数，小表可忽略，大表按照一个分片至多25G设置分片数，默认为3 |
| number_of_replicas | 否              | string          | 副本数，例如一主一副为1，默认为1 |
| rolling_period | 否              | string          | 子表时长（单位：天），为了方便做数据过期清理和提高查询效率，根据特定时间间隔划分子表，缺省情况下由数据过期时间决定 |

### 返回内容 ###
需要通过 error 字段判断请求是否成功，若返回内容有 error 字段则请求失败，具体错误详情请参照 error 字段描述。
### JSON示例说明 ###
请求：`PUT /_metric/ctsdb_test/update`

请求数据：

    
	{
		"tags":{
			"set":"string"
		},
		"time":{
			"name":"timestamp",
			"format":"epoch_second"
		},
		"fields":{
			"diskUsage":"float"
		},
		"options":{
		    "expire_day":15,
		    "number_of_shards":10
	    }
    }

返回：

    {
	    "acknowledged": true,
	    "message": "update ctsdb metric test111 success!"
    }
