# NMONs 常見問題集
## 可以分析多久的nmon 歷史log</h4>

## 可以同時分析幾台nmon的log

- test1
- test2
 - test21
 - test22


```
total 136
drwxr-xr-x  21 polinchen  staff   714  7  1 12:39 .
drwxr-xr-x  10 polinchen  staff   340  6 27 21:28 ..
drwxr-xr-x  14 polinchen  staff   476  7  1 12:39 .git
-rw-r--r--   1 polinchen  staff  3671  6 26 09:08 ELK-central.md
-rw-r--r--   1 polinchen  staff  3647  6 26 08:54 ELK-splunk.md
-rw-r--r--   1 polinchen  staff  1076  6 22 11:10 LICENSE
-rw-r--r--   1 polinchen  staff    47  6 22 11:10 README.md
drwxr-xr-x  13 polinchen  staff   442  6 25 13:37 _site
-rw-r--r--   1 polinchen  staff  1356  6 24 15:23 appinum01.md
-rw-r--r--   1 polinchen  staff  4610  6 25 12:15 boston-img.md
-rw-r--r--   1 polinchen  staff  1561  6 24 10:38 devop_2016.md
-rw-r--r--   1 polinchen  staff  2665  6 22 19:23 faq.md
-rw-r--r--   1 polinchen  staff   874  6 22 14:54 git_start.md
-rw-r--r--   1 polinchen  staff   911  6 29 10:58 iis60.md
drwxr-xr-x   3 polinchen  staff   102  6 29 10:52 img
-rw-r--r--   1 polinchen  staff  5420  6 24 21:56 markdown-sample.md
-rw-r--r--   1 polinchen  staff  1820  6 25 10:29 markdown_advance.md
-rw-r--r--   1 polinchen  staff  2506  6 25 10:12 md-case1.md
-rw-r--r--   1 polinchen  staff  2108  7  1 12:38 md-test.md
-rw-r--r--   1 polinchen  staff  2552  6 23 07:45 top2bi.md
-rw-r--r--   1 polinchen  staff     0  7  1 12:39 xxx
```
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
