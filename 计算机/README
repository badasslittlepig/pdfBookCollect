[TOCM]

[TOC]

日志系统搭建
#一、环境需求
	git、docker
	  yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
	  yum -y install git socat openssh-server openssh-clients nc elinks links yum-utils device-mapper-persistent-data lvm2 docker-ce python epel-release python-pip 
	  yum update -y
	  pip install docker-compose
	  systemctl start docker
	  systemctl enable docker
	  docker network create golang_backend_general_network
	  git clone http://git.wowkai.cn/openplatform/docker-contain-service.git
	  cd docker-contain-service
	  docker-compose up -d couchdb
	  docker ps -a
	![coudb启动图](https://git.wowkai.cn/openplatform/docker-contain-service/blob/master/image/docker_couchdb_start.png "coudb启动图")
	
	dokcer logs containerID 查看是否有报错信息
	
	请参照文档：[http://docs.couchdb.org/en/latest/setup/single-node.html](http://docs.couchdb.org/en/latest/setup/single-node.html "http://docs.couchdb.org/en/latest/setup/single-node.html")
	
	docker restart couchdb_service 
	
	kafka
	
	java 环境 1.8
	  yum -y install java-1.8.0-openjdk*
	  检测  java --version
	  
	logstash 安装包 
	  wget https://artifacts.elastic.co/downloads/logstash/logstash-7.1.0.zip
	  unzip logstash-7.1.0.zip
	  
#二、文件配置
######logstash配置格式：
```javascript
	input {
  		file {
    		path => "/home/download_data/test_log/server0.2019-05-20"
    		type => "nginx-access"
    		discover_interval => 15                     #logstash 每隔多久去检查一次被监听的 path 下是否有新文件。默认值是 15 秒。
    		sincedb_path => "/etc/logstash/.sincedb"    #定义sincedb文件的位置
    		start_position => "beginning"               #定义文件读取的位置
  		}
	}

	filter{
  		grok{
    		match => [
			#支持格式：2019-05-20 23:54:04.547  INFO [order-service,f46780230572f523,468e212cde719169,false] 5432 --- [io-10204-exec-8] c.j.m.common.log.SpringRequestLogFilter  : PerfLog_0:{"type":"SPRING_REQ","uri":"/order/list_app","httpMethod":"POST","header":null,"param":"{}","body":"{\"orderStatus\":0,\"orderType\":2,\"userId\":null,\"tenantId\":null,\"pageNum\":1,\"pageSize\":10}"}
			"message","(?<create_time>\S+\s+\S+)\s\s(?<level>\S+)\s(?<msg_id>\S+)\s(?<position>\d+.*\s\:)\s(?<data>.*)"
			]
    	}
	}

	output {
		#输出调试代码
  		#stdout {
    		#codec => rubydebug
  		#}
		#输出至kafka
		kafka {
    		bootstrap_servers => "host:port"    #生产者
    		topic_id => "kafka_topic_name"     #设置写入kafka的topic
    		compression_type => "gzip"           #消息压缩模式，默认是none，可选gzip、snappy。
  		}
	}
	```
	
######kafka接收数据的格式


######golang配置文件:
文件路径：xxx/data_analysis_service/core.yaml
	
	debug: "true"

	service:
  	  host: 8089                    #启动端口号

	#kafka配置
	kafka:
  	  host: "129.211.134.91"  #host地址
  	  port: "993"                    #端口
  	  topic_one: "log"            #kafka topic_名字
  	  topic_key: "log"            #kafa   
	
	#couchDB配置
	couchDbLog:
  	  host: "129.211.130.181"         #host 地址
  	  port: 306                              #端口
  	  username: "123"                    #用户名
  	  password: "ilovejuz"
  	  database: "数据库名"

	logNotice:
  	  noticeUri: "http://127.0.0.1:8089/task/log_notice"			#通知地址
  	  oauthUri: "https://oauth.wowkai.cn/oauth2/token"			#授权地址
  	  appKey: "5d0a594a-275d-4935-b80c"									#应用key
  	  appSecret: "WPDNRPYBICYUHFAJJMODOYZMJYRUC"			#应用密钥
  	  errLevel:															#报错等级配置，格式：等级:"1"  0-不通知 1-通知
    	  error: "1"
    	  warn: "1"
    	  notice: "0"
	
	
