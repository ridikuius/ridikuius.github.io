---
layout: post
title: "FileBeat+Elasticsearch+kibana日志集成环境搭建"
date: 2016-06-21
excerpt: "FileBeat+Elasticsearch+kibana日志集成环境搭建与使用."
tags: [ELK,FileBeat,日志]
comments: true
---

# FileBeat + Elasticsearch + kibana

日志收集+搜索分析+可视化

## FileBeat

### 简介

Filebeat是一个开源的文件收集器，主要用于获取日志文件，并把它们发送到logstash或elasticsearch。

### 安装

#### 下载

`https://www.elastic.co/downloads/beats/filebeat`

#### 安装（windows）

* `PS > cd 'D:\Filebeat'`
* `PS C:\Program Files\Filebeat> PowerShell.exe -ExecutionPolicy UnRestricted -File .\install-service-filebeat.ps1`

#### 配置

Filebeat的配置文件是./filebeat/filebeat.yml，遵循YAML语法。具体可以配置如下几个项目：

*   Filebeat
*   Output
*   Shipper
*   Logging(可选)
*   Run Options（可选）

Filebeat的部分主要定义prospector的列表，定义监控哪里的日志文件，关于如何定义的详细信息可以参考filebeat.yml中的注释，下面主要介绍一些需要注意的地方。

*   paths：指定要监控的日志，没有对配置目录做递归处理：

> d:\itone\program\logs\*。

*   encoding：指定被监控的文件的编码类型，使用plain和utf-8都是可以处理中文日志的。
`encoding: GBK`
*   input_type：指定文件的输入类型log(默认)或者stdin。

*   exclude_lines：在输入中排除符合正则表达式列表的那些行。

*   include_lines：包含输入中符合正则表达式列表的那些行（默认包含所有行），include_lines执行完毕之后会执行exclude_lines。

*   exclude_files：忽略掉符合正则表达式列表的文件（默认为每一个符合paths定义的文件都创建一个harvester）。

*   fields：向输出的每一条日志添加额外的信息，比如“level:debug”，方便后续对日志进行分组统计。默认情况下，会在输出信息的fields子目录下以指定的新增fields建立子目录，例如fields.level。

> fields: 
> level: debug

*   ignore_older：可以指定Filebeat忽略指定时间段以外修改的日志内容，比如2h（两个小时）或者5m(5分钟)。

*   close_older：如果一个文件在某个时间段内没有发生过更新，则关闭监控的文件handle。默认1h,change只会在下一次scan才会被发现

*   force_close_files：Filebeat会在没有到达close_older之前一直保持文件的handle，如果在这个时间窗内删除文件会有问题，所以可以把force_close_files设置为true，只要filebeat检测到文件名字发生变化，就会关掉这个handle。

*   scan_frequency：Filebeat以多快的频率去prospector指定的目录下面检测文件更新（比如是否有新增文件），如果设置为0s，则Filebeat会尽可能快地感知更新（占用的CPU会变高）。默认是10s。
* multiline 多行配置
   pattern：正则表达式
   negate：true 或 false；默认是false，匹配pattern的行合并到上一行；true，不匹配pattern的行合并到上一行
   match：after 或 before，合并到上一行的末尾或开头

```
 multiline.pattern: ^\[
 multiline.negate: true
 multiline.match: after
```

## Elasticsearch

### 简介

http://es.xiaoleilu.com/index.html
Elasticsearch是一个基于[Apache Lucene(TM)](https://lucene.apache.org/core/)的开源搜索引擎。
Elasticsearch不仅仅是Lucene和全文搜索，我们还能这样去描述它：
*   分布式的实时文件存储，每个字段都被索引并可被搜索
*   分布式的实时分析搜索引擎
*   可以扩展到上百台服务器，处理PB级结构化或非结构化数据

### 安装

#### 下载

`https://www.elastic.co/downloads/elasticsearch`

#### 配置

##### 绑定的IP地址

设置绑定的ip地址，可以是ipv4或ipv6的，默认为0.0.0.0
`network.host: ["0.0.0.0"]`

##### 交互的IP地址

设置其它节点和该节点交互的ip地址，如果不设置它会自动判断，值必须是个真实的ip地址。
`network.publish_host: ["192.168.0.1"]`

#### 启动（windows）

* `cd d:\elasticsearch`
* `.\bin\elasticsearch`

#### 访问`localhost:9200`验证启动成功

```
name———Elasticsearch实例的名字，默认情况下它将从名字列表中随机选择一个，其设置是在config/elasticsearch.yml文件中；

version———版本号，以json格式表示了一组信息，其中：

number字段代表了当前运行Elasticserch的版本号；

build_snashot字段代表了当前版本是否是从源代码构建而来；

lucene_version表示Elasticsearch所基于的Lucene的版本；

tagline———包含了Elasticsearch的第一个tagline:"You Know, for Search"。
```

#### 将elasticsearch安装为服务

`d:\elasticsearch\bin>elasticsearch-service install`

## kibana

### 简介

Kibana是一个基于浏览器页面的Elasticsearch前端展示工具。Kibana全部使用HTML语言和Javascript编写的。

### 安装

#### 下载

`https://www.elastic.co/downloads/kibana`

#### 解压并启动（windows）

* `CD d:\kibana`
* `.\bin\kibana`

#### 访问`localhost:5601`验证启动成功


## 检索

### 全文搜索

在搜索栏输入debug，会返回所有字段值中包含debug的文档
![](index_files/ea98a6b1-1e8d-498f-bb50-ce2df3a0b169.png)

### 字段

也可以按页面左侧显示的字段搜索
限定字段全文搜索：`field:value`
精确搜索：关键字加上双引号 `filed:"value"`
`offset:624179` 搜索http状态码为404的文档

字段本身是否存在
`_exists_:message`：返回结果中需要有http字段
`_missing_:message`：不能含有http字段

### 通配符

`?` 匹配单个字符
`*` 匹配0到多个字符

`runni?g`, `Wrapper*AppMain`

> `?` `*` 不能用作第一个字符，例如：`?text` `*text`

### 正则

es支持部分[正则](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-regexp-query.html#regexp-syntax)功能
`mesg:/mes{2}ages?/`

### 模糊搜索

`~`:在一个单词后面加上`~`启用模糊搜索

`first~` 也能匹配到 frist

还可以指定需要多少相似度
`cromm~0.3` 会匹配到 from 和 chrome
数值范围0.0 ~ 1.0，默认0.5，越大越接近搜索的原始值

### 近似搜索

在短语后面加上`~`
`"select where"~3` 表示 select 和 where 中间隔着3个单词以内

### 范围搜索

数值和时间类型的字段可以对某一范围进行查询
`offset:[628053 TO  628176]``
[ ] 表示端点数值包含在范围内，{ } 表示端点数值不包含在范围内

### 逻辑操作

`AND`
`OR`

`+`：搜索结果中必须包含此项
`-`：不能含有此项
`+running  -TopoMetricValueProvider`：结果中必须存在apache，不能有jakarta，test可有可无

### 分组

`(com.its.itone.core.jsonrpc.tools.GroovyRpcServlet RO running) AND debug`

### 字段分组

`message:(com.its.itone.core.jsonrpc.tools.GroovyRpcServlet RO running) AND debug`

### 转义特殊字符

`+ - && || ! () {} [] ^" ~ * ? : \`
以上字符当作值搜索的时候需要用`\`转义





