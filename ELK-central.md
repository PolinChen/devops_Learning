
# 日誌集中(Centralized Logging)

Centralized Logging
- [logging tools](http://jasonwilder.com/blog/2012/01/03/centralized-logging/)

日誌是任何系統的重要組成部分，他們給你洞察系統正在做的事情，以及發生了什麼事。事實上，在系統上運行的每個進程生成某種格式的日誌。通常，這些日誌寫入本地磁碟上的文件。當您的系統增長到多台主機，日誌管理和訪問會變得複雜。如果要搜索在數百台服務器在數百個日誌文件的特定錯誤, 沒有好的工具會變得非常的困難。要解決這個問題,是先建立一個集中log的解決方案，使多個日誌可以在一個集中的地方進行匯總。

那麼，什麼是你的選擇？

## 利用Logstash過濾器，以提高資料的集中

Logstash介紹

Logstash是一個專門集中和分析日誌的強大工具，它可以幫助提供和描述你的系統環境，並且可以辨認你伺服器的相關問題。
利用ELK stack（Elasticsearch，Logstash和Kibana）的高效設置,來收集重要的應用系統的log日誌和結構化log日誌，經由部署和過濾讓這些資料變得可讀性提高，和可以查詢的能力，同時通過`grok`的配置，可以將這些log，變成有用的資訊。

## Splunk VS ELK stack - 哪些工具是最適合你？

就像在競選活動中政客所做的承諾，生產環境產生瀰漫在日誌文件的格式文本的無盡行大量的文件。不像選舉期間，他們一年四季都這樣做，每一天產生的非結構化純文本數據的多個綠帶。

在大多數情況下，通過簡單的日誌文件手動去，`grepping`所有的地方的行為，嚴重限制了你可以從中提取價值。有些人甚至說，這是......瘋狂的邊緣。

在最流行的2種日誌管理解決方案：Splunk的和ELK（Elasticsearch-Logstash-Kibana），以幫助指導你通過你的問題“會需要問自己，以便做出正確的選擇。
![][splunk-elk]


1.基礎知識 - 誰是誰在日誌管理？

無論是錯誤和異常log，業務邏輯，或任何其他類型的日誌分析，Splunk的和Elastic stack是當前最大的企業級解決方案領域的方法。 Splunk的是一家上市公司，提供與在其不同的產品有15天的試用完整的商業解決方案。 ELK是ElasticSearch，Logstash和Kibana的合稱縮寫，同時提供免費開源、商業支持、管理的log 分析解決方案


![查看谷歌趨勢報告全文][elk-trend]

針對這兩個產品最大的批評是，`Splunk`是太昂貴了;而`ELK`因為是免費和開源的軟體，擔心服務能力不足。

> 同時`Splunk`和`ELK`正在主導最全面和可定制的log分析解決方案。



2.問題 - 什麼是你想解決問題？

從市場角度看，Splunk瞄準大企業，和負擔得起產品的公司。 而ELK開源解決方案往往是， 隨著彈性的公司，並不斷增長ELK，他們提供優質的支持服務和一套新的支付和開源服務。 已經看到在所有類型的不同的公司採用。
主要的事情要考慮：

- 每日需要和消耗多少log（主要成本考量）
- 這和你想有多少服務連接 用戶，他們是所有開發人員？
- 具體特殊用例
- 預期的變化，以您的儀表盤率

> 如果你正在尋找先進`grepping`功能（其中有很多對自己的價值），並便於可視化，那麼全Splunk的部署可能是矯枉過正。


[splunk-elk]:http://384uqqh5pka2ma24ild282mv.wpengine.netdna-cdn.com/wp-content/uploads/2016/02/splunk-elk.png

[elk-trend]:http://384uqqh5pka2ma24ild282mv.wpengine.netdna-cdn.com/wp-content/uploads/2016/02/splunk-elk-trends.png
