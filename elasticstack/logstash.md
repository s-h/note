## input

### kafka

    kafka{
       bootstrap_servers => ["192.168.1.1:9092,192.168.1.2:9092,192.168.1.3:9092"]
       client_id => "logstash1"
       group_id => "logstashgroup"
       auto_offset_reset => "earliest"
       consumer_threads => 5
       decorate_events => true
       topics => ["nginx"]
       id => "logstash1"
       codec => json
       tags => "nginx"
    }

### syslog

    syslog{
      type => "syslog"
      port => 5000
    }

## filter
### grok
(?<port>\d+) #使用自定义正则
%{PORT:port} #使用预定义正则

    grok {
      patterns_dir => ["/etc/logstash/patterns"]
      match => { "message" => "%{TOMCATDATE:EventDate}(,\d+)?\s%{THREADPOOL:ThreadPool}\s*%{LOGLEVEL:LogLevel}\s*%{GREEDYDATA:applog}" }
	  remove_field => ["message"]
	}

### date

    date {
	  match => ["EventDate","yyyy-MM-dd HH:mm:ss"]
	}


### mutate
结合控制语句

    if "192.168.1.254" in [host] {
    mutate {
      add_field => {"alias" => "route-254"}
    }

## output
### elasticsearch

	if "nginx" in [tags] {
	    elasticsearch {
	      hosts => ["192.168.1.1:9200","192.168.1.2:9200","192.168.1.3:9200"]
	      index => "nginx-access-%{+YYYY-MM-dd}"
	    }

### kafka

    kafka {
      codec => json
      bootstrap_servers => ["192.168.1.1:9092,192.168.1.2:9092,192.168.1.3:9092"]
      topic_id => "syslog"
    }

## 控制语句

