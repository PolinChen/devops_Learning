# NMONs 常見問題集
利用大數據提升用戶滿意度
## 可以分析多久的nmon 歷史log</h4>
  >NMONs 大數據分析平台可以支援所有nmon 的歷史log， 包含3-5年以上的歷史資料，從這些歷史log 中，進行數據挖掘、統計、篩選、歸納，以建立系統負載趨勢統計，以當成系統升級改造方案的量化依據，避免設備採購後，改善效果不如預期的風險。   
  >在首次的負載風險評估時，建議最少完整1-3月的log，以便分析評估現有的核心系統中，是否存在隱憂和瓶頸。以當成維運團隊調優的最佳依據，提升異常現象的處理效率。

## 可以同時分析幾台nmon的log
> NMONs 大數據分析平台在一個用戶帳號中，可以同時分析128臺不同server的nmon log，並且依據不同的核心系統進行分類和關聯分析，如不同DB server，AP server 之間進行對比分析

## NMONs+ 可以結合哪些效能log一起進行分析
>目前除了支援各種操作系統的log(AIX\HPUX\SunOS\Linux\Microsoft)外，還可以支持各種不同存儲設備的log(如EMC、HDS、IBM、HP、netapp等)，同時對database log 進行關聯分析（Oracle AWR\Statspack report)

## NMONs 大數據分析平台可以支持哪些nmon的版本
> NMONs 目前可以支持所有的 AIX和Linux 上產生的nmon 格式的log

## 如何上傳系統產生的nmon log</h4>
> 在申請用戶帳號後，即可使用2種上傳的模式，包含通過web 手動上傳nmon 歷史log， 或是經由ftp的方式，設定每天自動上傳昨天收集的nmon log; 發送的格式可以為原始的nmon格式或是經由壓縮的rar/zip/7z/tar.gz 的壓縮格式.  

## nmon log 收集參數設定建議
> 為了更精準的進行大數據分析，在設定nmon 的收集命令時， 建議收集全天24小時、採樣點為1440次，即60秒的收集間隔，并增加Top Process的運行記錄，建議如下：
{% highlight Bash%}
/nmon/bin/nmon12e_aix61 -F server_201503010000.nmon -fT -s 60 -c 1440  
{% endhighlight %}
> nmon log 收集過程占用CPU負載率小於0.5%，不會影響系統的正常運行。

## NMONs 和 NMON analyzer 功能差異如何？
>NMONs 除了兼容NMONanalyzer 的分析圖表外，另外針對JFS、MPIO、TOP PID 的記錄進行可視化分析。同時可以把不同系統多台的主機產生的nmon log 進行關聯解析； 無需人工進行轉換， 更有效率的解讀nmon log 的深層的意義。  


![alt text][logo]

[logo]: https://hackpad-attachments.imgix.net/turboteam.hackpad.com_u3H8jjdgMWx_p.527885_1466593934326_螢幕快照%202016-06-22%20上午10.57.15.png?fit=max&w=882  
