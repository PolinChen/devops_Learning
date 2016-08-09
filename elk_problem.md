## elk elasticsearch error status

### 問題描述
IISlog 執行完畢後 ， elasticsearch 還在持續運行， logstash 已經正常結束了, elasticsearch  持續耗盡1個CPU 100%

```
# sudo service elasticsearch status
# top
# sudo service elasticsearch stop (大概有3-5分鐘， 才會正常結束)
# sudo service elasticsearch status
# sudo service elasticsearch start
```

此時檢查top 發現elasticsearch 在重新initial ， 大約需要20-30分鐘, 重新進入kibana 才會發揮正常，同時從top 發現 CPU 的loading 已經恢復到2-3% 的範圍中

從iPOC 看到的圖形: ![ipoc-elk](/img/pocelk-problem1.png)

從kibana 看到的內容: ![ipoc-elk1](/img/elk-problem1.png)

