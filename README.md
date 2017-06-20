# elk-Configuration-instance

 filebeat 5.4.1 + logstash 5.4.1 + elasticsearch 5.4.1 + kibana 5.4.1
 
 # Install ELK
 
 ## 1.install filebeat
 
 download filebeat :
 
 wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-5.4.1-x86_64.rpm
 
 wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-5.4.1-linux-x86_64.tar.gz 
 
 tar zxvf filebeat-5.4.1-linux-x86_64.tar.gz
 
 nohup ./filebeat-5.4.1-linux-x86_64/filebeat -e -c filebeat-5.4.1-linux-x86_64/filebeat.yml &
  
 ## 2.install logstash
 
 download logstash:
 
 wget https://artifacts.elastic.co/downloads/logstash/logstash-5.4.1.rpm
 
 wget https://artifacts.elastic.co/downloads/logstash/logstash-5.4.1.tar.gz
 
 tar zxvf logstash-5.4.1.tar.gz
 
 nohup /usr/local/logstash/bin/logstash -f /usr/local/logstash/config/nginx.conf &
 

 ## 3.install elasticsearch
 
 download es:
 
 wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.4.1.rpm
 
 wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.4.1.tar.gz
 
 tar zxvf elasticsearch-5.4.1.tar.gz
 
 groupadd es && useradd -g es es
 
 chown es:es /elasticsearch/ -R
 
 su - es 
 
./elasticsearch-5.4.1/bin/elasticsearch -d
 
  ## 4.install kibana
  
  download kibana:
  
  wget https://artifacts.elastic.co/downloads/kibana/kibana-5.4.1-linux-x86_64.tar.gz
  
  wget https://artifacts.elastic.co/downloads/kibana/kibana-5.4.1-x86_64.rpm
  
  rpm -ivh kibana-5.4.1-x86_64.rpm
  
  service kibana start
 
## 5.install plugin

wget https://artifacts.elastic.co/downloads/packs/x-pack/x-pack-5.2.2.zip

bin/elasticsearch-plugin install x-pack

bin/kibana-plugin install x-pack

