# iislog 在ELK 的處理流程

- 更改conf.d 的index
 - 更改conf.d 的內容
 - grok debugger 的測試
- 檢查logstash 的運行方式
 - 停止logstash 
- 單獨啟動logstash 測試內容
 - debug for logstash


[microsoft iis 6.0 說明文件](https://www.microsoft.com/technet/prodtechnol/WindowsServer2003/Library/IIS/676400bc-8969-4aa7-851a-9319490a9bbb.mspx?mfr=true)

```shell
cat /etc/logstash/conf.d/turboteam-iislog-witsbpm.conf

input {
    file {
        path => [
                 "/diskZ/elklog/witsbpm/u_ex*.log"
                ]
        start_position => "beginning"
        type => "iislog-witsbpm"
    }
}
filter {
  if [type] == "iislog-witsbpm" {
 #ignore log comments
  if [message] =~ "^#" {
    drop {}
  }
grok {
break_on_match => false
    # check that fields match your IIS log 6 with HTTP/ as follow
#Fields: date time s-ip cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) sc-status sc-substatus sc-win32-status time-taken
#2016-07-17 00:00:10 fe80::19c8:6be6:16c:1703%13 GET /wfs7/Default.aspx - 80 - fe80::19c8:6be6:16c:1703%13 - 200 0 0 2191

    match => ["message", "%{TIMESTAMP_ISO8601:log_timestamp} %{IPORHOST:s_ip} %{WORD:cs_method} %{NOTSPACE:cs_uri_stem_name} %{NOTSPACE:cs_uri_query} %{NUMBER:s_port} %{NOTSPACE:cs_username} %{IPORHOST:c_ip} %{NOTSPACE:cs_useragent} %{NUMBER:cs_status} %{NUMBER:cs_substatus} %{NUMBER:sc_win32_status} %{NUMBER:time_taken}"]
  }
  mutate {
    convert => { "time_taken" => "integer" }
    convert => { "sc_bytes" => "integer" }
    convert => { "cs_bytes" => "integer" }
  }

  #Set the Event Timesteamp from the log
    date {
      # +8 for next day
      timezone => "Etc/UTC"
      match => [ "log_timestamp", "YYYY-MM-dd HH:mm:ss" ]
      #timezone => "Asia/Taipei"
  }

  mutate {
    remove_field => [ "log_timestamp"]
  }
}
}

output {
  if [type] == "iislog-witsbpm" {
  elasticsearch {
    template_overwrite => true
    hosts => ["localhost:9200"]
    index => "logstash-witsbpm-ok-%{+YYYY.MM.dd}"
  }
  #stdout { codec => rubydebug }
  }
}

```

