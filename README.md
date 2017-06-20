# elk-Configuration-instance

es官网文档：https://www.elastic.co/guide/index.html

 filebeat 5.4.1 + logstash 5.4.1 + elasticsearch 5.4.1 + kibana 5.4.1
 
 ![image](https://github.com/xiaoxiamin/elk-Configuration-instance/blob/master/picture/QQ%CD%BC%C6%AC20170620160929.png)
 
 filebeat: 收集日志，将数据发送到Logstash和Elasticsearch
 
 logstash: 收集、转换和解析日志
 
 Elasticsearch: 是一种分布式，基于JSON的搜索和分析引擎，旨在提供水平可扩展性，最高可靠性和易于管理。（搜索、分析和存储数据）
 
 kibana: 可视化数据，图形展示，用于配置和管理Elastic Stack的可扩展用户界面
 
 # Install ELK
 
 ## Preparation before installation：
 
 install jdk8+
 
### vi /etc/security/limits.conf 
```
* soft nofile 65536

* hard nofile 131072

* soft nproc 2048

* hard nproc 4096
```
 ### vi /etc/security/limits.d/90-nproc.conf 

修改如下内容：

`* soft nproc 1024`

#修改为

`* soft nproc 2048`

### vi /etc/sysctl.conf 

添加下面配置：

`vm.max_map_count=655360`

并执行命令：`sysctl -p`

 
 ## 1.install filebeat
 
 download filebeat :
 
 wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-5.4.1-x86_64.rpm
 
 wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-5.4.1-linux-x86_64.tar.gz 
 
 tar zxvf filebeat-5.4.1-linux-x86_64.tar.gz
 
 `nohup ./filebeat-5.4.1-linux-x86_64/filebeat -e -c filebeat-5.4.1-linux-x86_64/filebeat.yml &`
  
 ## 2.install logstash
 
 download logstash:
 
 wget https://artifacts.elastic.co/downloads/logstash/logstash-5.4.1.rpm
 
 wget https://artifacts.elastic.co/downloads/logstash/logstash-5.4.1.tar.gz
 
 tar zxvf logstash-5.4.1.tar.gz
 
 `nohup /usr/local/logstash/bin/logstash -f /usr/local/logstash/config/nginx.conf &`


 ## 3.install elasticsearch
 
 download es:
 
 wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.4.1.rpm
 
 wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.4.1.tar.gz
 
 tar zxvf elasticsearch-5.4.1.tar.gz
 
 groupadd es && useradd -g es es
 
 chown es:es /elasticsearch/ -R
 
 su - es 
 
`./elasticsearch-5.4.1/bin/elasticsearch -d`

 #### ERROR:
 
 system call filters failed to install; check the logs and fix your configuration or disable system call filters at your own risk

原因：
这是在因为Centos6不支持SecComp，而ES5.2.0默认bootstrap.system_call_filter为true进行检测，所以导致检测失败，失败后直接导致ES不能启动。

解决：
在elasticsearch.yml中配置bootstrap.system_call_filter为false，注意要在Memory下面:
```
bootstrap.memory_lock: false
bootstrap.system_call_filter: false
``` 
  
  ## 4.install kibana
  
  download kibana:
  
  wget https://artifacts.elastic.co/downloads/kibana/kibana-5.4.1-linux-x86_64.tar.gz
  
  wget https://artifacts.elastic.co/downloads/kibana/kibana-5.4.1-x86_64.rpm
  
  rpm -ivh kibana-5.4.1-x86_64.rpm
  
  `service kibana start`
 
## 5.install plugin

wget https://artifacts.elastic.co/downloads/packs/x-pack/x-pack-5.2.2.zip

bin/elasticsearch-plugin install x-pack

bin/kibana-plugin install x-pack

elk docker版本：http://elk-docker.readthedocs.io/
