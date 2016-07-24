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

### 檢查和停止logstash

```shell
➜  ps -ef | grep logstash
polin     1906 31953  0 21:36 pts/0    00:00:00 grep --color=auto --exclude-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn logstash
logstash  5630     1  3 18:58 ?        00:05:46 /usr/bin/java -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -Djava.awt.headless=true -XX:CMSInitiatingOccupancyFraction=75 -XX:+UseCMSInitiatingOccupancyOnly -XX:+HeapDumpOnOutOfMemoryError -Djava.io.tmpdir=/var/lib/logstash -Xmx500m -Xss2048k -Djffi.boot.library.path=/opt/logstash/vendor/jruby/lib/jni -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -Djava.awt.headless=true -XX:CMSInitiatingOccupancyFraction=75 -XX:+UseCMSInitiatingOccupancyOnly -XX:+HeapDumpOnOutOfMemoryError -Djava.io.tmpdir=/var/lib/logstash -XX:HeapDumpPath=/opt/logstash/heapdump.hprof -Xbootclasspath/a:/opt/logstash/vendor/jruby/lib/jruby.jar -classpath : -Djruby.home=/opt/logstash/vendor/jruby -Djruby.lib=/opt/logstash/vendor/jruby/lib -Djruby.script=jruby -Djruby.shell=/bin/sh org.jruby.Main --1.9 /opt/logstash/lib/bootstrap/environment.rb logstash/runner.rb agent -f /etc/logstash/conf.d -l /diskZ/ming/log/logstash/logstash.log -w 1

➜  sudo monit summary
The Monit daemon 5.14 uptime: 36d 9h 36m

Process 'zerotier-one'              Running
Process 'logstash'                  Running
System 'VM42.localdomain'           Running

➜  sudo monit stop logstash
➜  sudo monit summary
The Monit daemon 5.14 uptime: 36d 9h 36m

Process 'zerotier-one'              Running
Process 'logstash'                  Not monitored
System 'VM42.localdomain'           Running

```

### 變更conf 文件
－ 更改log 的位置
```
input {
    file {
        path => [
                 "/diskZ/elklog/testing/u_ex*.log"
                ]
        start_position => "beginning"
        type => "iislog-witsbpm"
    }
}
```


### 修改為爭取的conf 文件名字

－ conf 文件名字
```
-rw-r--r--  1 root root 1578 2016-07-21 09:04 turboteam-iislog-bpm.conf
```

－ 修改conf 內容
```
input {
    file {
        path => [
                 "/diskZ/elklog/bpmiis/u_ex*.log"
                ]
        start_position => "beginning"
        type => "iislog-bpm"
    }
}
filter {
  if [type] == "iislog-bpm" {
 #ignore log comments
  if [message] =~ "^#" {
    drop {}
  }

output {
  if [type] == "iislog-bpm" {
  elasticsearch {
    template_overwrite => true
    hosts => ["localhost:9200"]
    index => "logstash-iis-bpm-%{+YYYY.MM.dd}"
  }
  #stdout { codec => rubydebug }
  }
}
```

/opt/logstash/bin/logstash -f turboteam-iislog-bpm.conf --configtest
Configuration OK

- 將iislog 複製到 /diskZ/elklog/bpmiis 中
-rw-rw-r-- 1 polin polin 1660229 2016-07-21 09:12 u_ex160717.log

- 單獨執行logstash

/opt/logstash/bin/logstash -f turboteam-iislog-bpm.conf 
➜  conf.d git:(test) ✗ /opt/logstash/bin/logstash -f turboteam-iislog-bpm.conf
Settings: Default pipeline workers: 4
Pipeline main started

- 檢查執行結果

```
esh | grep bpm

yellow open   logstash-iis-bpm-2016.07.17                        3   1       6295            0        3mb            3mb

```

- 檢查logstash 的運行狀況
- 啟動logstash 的運行狀況
- 

sudo monit
Process 'logstash'
  status                            Not monitored
  monitoring status                 Not monitored
  data collected                    Tue, 19 Jul 2016 21:36:40

Process 'logstash'
  status                            Initializing
  monitoring status                 Initializing
  data collected                    Tue, 19 Jul 2016 21:36:40

Process 'logstash'
  status                            Running
  monitoring status                 Monitored
  pid                               15063
  parent pid                        1
  uid                               496
  effective uid                     496
  gid                               0
  uptime                            1m
  children                          0
  memory                            225.2 MB
  memory total                      225.2 MB
  memory percent                    2.8%
  memory percent total              2.8%
  cpu percent                       0.0%
  cpu percent total                 0.0%
  data collected                    Thu, 21 Jul 2016 09:22:59






```
### cp 一個iislog 到 /diskZ/elklog/bpmiis

➜  2016 ls -al /diskZ/elklog/bpmiis
總計 3440
drwxrwxr-x 2 polin polin    4096 2016-07-21 09:25 .
drwxrwxrwx 8 polin polin    4096 2016-07-21 09:12 ..
-rw-rw-r-- 1 polin polin 1848259 2016-07-21 09:25 u_ex160716.log
-rw-rw-r-- 1 polin polin 1660229 2016-07-21 09:12 u_ex160717.log

yellow open   logstash-iis-bpm-2016.07.16                        3   1       7177            0      2.9mb          2.9mb
yellow open   logstash-iis-bpm-2016.07.17                        3   1      12590            0      6.2mb          6.2mb


### cp 多個iislog 到 /diskZ/elklog/bpmiis
➜  2016 cp *1607* /diskZ/elklog/bpmiis







# /opt/logstash/bin/logstash -f turboteam-iislog-witsbpm.conf 
