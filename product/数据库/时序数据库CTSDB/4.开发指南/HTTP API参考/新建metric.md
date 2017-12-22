
## 请求地址 ##
地址为实例的 IP 和 PORT，可从控制台获取到，例如 10.13.20.15:9200

## 请求路径和方法 ##
路径：`/_metric/${metric_name}`，${metric_name} 为新建的 metric 的名称
方法：PUT
注意：metric 名称必须为小写字母，不能以下划线开头，并且不能包含逗号
## 请求参数 ##
无
## 请求内容 ##
| 参数名称        | 必选            | 类型            | 描述            |
|---------|---------|---------|---------|
| tags            | 是              | map             | 维度列，至少包含一个维度，支持的数据类型：text（带有分词、全文索引的字符串）、string（不分词的字符串）、long, integer, short, byte, double, float，date，boolean。格式如：{"region": "string","set":  "long","host": "string"} |
| time            | 是              | map             | 时间列相关配置，例如：{"name": "timestamp", "format":  "epoch_second"}   |
| fields          | 是              | map             | 指标列，为了节省空间，建议使用最适合实际业务使用的类型，支持的数据类型：string（字符串）、long, integer, short, byte,double, float，date，boolean。例如：{"cpu_usage":"float"}  |
| options         | 否              | map             | 常用的调优配置信息，例如：{"expire_day":7,"refresh_interval":"10s","number_of_shards":5,"number_of_replicas": 1,"rolling_period":1}  |

> **注意**
>
> - time字段的name默认为timestamp，时间格式（format）完全兼容es的时间格式。
> - options选项及解释如下：expire_day：数据过期时间，过期后数据自动清理，缺省情况下-1（代表永不过期）
	> refresh_interval：数据刷新频率，写入的数据从内存刷新到磁盘后可查询。认为10秒
	> number_of_shards：表分片数，小表可以忽略，大表按照一个分片至多25G设置分片数，默认为3
	> number_of_replicas：副本数，例如一主一副为1，缺省为1
	> rolling_period：子表时长（单位：天），CTSDB存储数据时，为了方便做数据过期和提高查询效率，根据特定时间间隔划分子表，缺省情况下由数据过期时间决定，下面具体说明缺省子表时长和过期时间的关系
	
|过期时间 |    子表时长|
|---------|---------|
|  ≤15天     |       1天|
| >15天, ≤3个月|   7天|
| ＞3个月|          30天|
| 永不过期 |        无穷大|

## 返回内容 ##
需要通过'error'字段判断请求是否成功，若返回内容有error字段则请求失败，具体错误详情请参照error字段描述

## JSON示例说明 ##
请求：`POST /_metric/ctsdb_test`

请求数据：
    
    {
    "tags":{
    "region":"string"
    },
    "time":{
    "name":"timestamp",
    "format":"epoch_second"
    },
    "fields":{
    "cpuUsage":"float"
    },
    "options":{
    "expire_day":7,
    "refresh_intercal":"10s",
    "number_of_shards":5
    }
    }

成功的返回：

	{
    "acknowledged": true,
    "message": "create ctsdb metric ctsdb_test success!"
    }

失败的返回：

    {
    "error": {
    "reason": "table ctsdb_test already exist",
    "type": "metric_exception"
    },
    "status": 201
    }
