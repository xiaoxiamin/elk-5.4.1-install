# elk-configuration-instance

es官网文档：https://www.elastic.co/guide/index.html

elk中文文档：https://kibana.logstash.es/content/

 filebeat 5.4.1 + logstash 5.4.1 + elasticsearch 5.4.1 + kibana 5.4.1
 
 ![image](https://github.com/xiaoxiamin/elk-Configuration-instance/blob/master/picture/QQ%CD%BC%C6%AC20170620160929.png)
 
 filebeat: 收集日志，将数据发送到Logstash和Elasticsearch
 
 logstash: 收集、转换、过滤和解析日志  (插件化：input、codec、filter、output)
        
               工作流程：input | filter | output
           
 Elasticsearch: 是一种分布式，基于JSON的搜索和分析引擎，旨在提供水平可扩展性，最高可靠性和易于管理。（搜索、分析和存储数据）
 
 kibana: 可视化数据，图形展示，用于配置和管理Elastic Stack的可扩展用户界面


elk docker版本：http://elk-docker.readthedocs.io/


### kibana实现效果：

![images](https://github.com/xiaoxiamin/elk-Configuration-instance/blob/master/picture/QQ%CD%BC%C6%AC20170627142423.png)

### dashboard实现效果：
![images](https://github.com/xiaoxiamin/elk-Configuration-instance/blob/master/picture/QQ%CD%BC%C6%AC20170627142838.png)
