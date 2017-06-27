# elk-5.4.1版本配置实例

es官网文档：https://www.elastic.co/guide/index.html

elk中文文档：https://kibana.logstash.es/content/

elk docker版本：http://elk-docker.readthedocs.io/
 
 ![image](https://github.com/xiaoxiamin/elk-Configuration-instance/blob/master/picture/QQ%CD%BC%C6%AC20170620160929.png)
 
 - `filebeat`: 收集日志，将数据发送到Logstash和Elasticsearch
 
 - `logstash`: 收集、转换、过滤和解析日志  (插件化：input、codec、filter、output)
               工作流程：input | filter | output
           
 - `Elasticsearch`: 是一种分布式，基于JSON的搜索和分析引擎，旨在提供水平可扩展性，最高可靠性和易于管理。（搜索、分析和存储数据）
 
 - `kibana`: 可视化数据，图形展示，用于配置和管理Elastic Stack的可扩展用户界面


## lostash配置

### input插件

以下例子使用beats插件，开放本机5044端口提供给beat连接并接收beats发送的数据，也可使用file、stdin、redis、beats等其他插件，具体每个插件的参数可参考官网文档。

```       input {
           beats {
            port => 5044
           }
          }
```

### filter插件

使用grok正则匹配nginx的访问日志，匹配每一段的内容定义别名：

```
 grok {
  match => { 'message' => '%{IPV4:clientip} %{USER:ident} %{USER:auth} \[%{HTTPDATE:time}\] "%{WORD:method} %{URIPATHPARAM:request} HTTP/%{NUMBER:httpversion}" %{NUMBER:http_status} %{NUMBER:bytes} %{QS:http_referer} %{QS:user_agent} %{QS:x_forwarded_for}' }
 }
```

此部分针对es的时区utc的问题，给默认时间加8小时：

```
    mutate{
        gsub => [
       "time", "[+]", "T"
                 ]
      }
        mutate{
      replace => ["time","%{time}+08:00"]
      }
}

```

### output插件

将filter处理后的日志发送到elasticsearch中，指定index 默认索引以logstash-localtime：

```
output {
 elasticsearch {
 hosts => ["192.168.1.151:9200"]
 user => elastic
 password => changeme
 manage_template => false
 index => "%{[@metadata][beat]}-%{+YYYY.MM.dd}"
 document_type => "%{[@metadata][type]}"
 flush_size => 20000
 idle_flush_time => 10
 }
}
```


#### filebeat、elasticsearch、kibana的配置可参考项目中对应的配置文件

### kibana实现效果：

![images](https://github.com/xiaoxiamin/elk-Configuration-instance/blob/master/picture/QQ%CD%BC%C6%AC20170627142423.png)

### dashboard实现效果：
![images](https://github.com/xiaoxiamin/elk-Configuration-instance/blob/master/picture/QQ%CD%BC%C6%AC20170627142838.png)
