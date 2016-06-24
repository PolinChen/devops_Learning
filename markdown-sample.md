# markdown 建議學習路徑

- 參考資料[中文版說明](http://markdown.tw/)
- 原始文件[github 版本連結]( https://github.com/othree/markdown-syntax-zhtw/blob/master/syntax.md)
- github [參考資料](
https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
- 教材1 [bitbucket](https://bitbucket.org/tutorials/markdowndemo)
- 範例一 [good sample](http://www.unexpected-vortices.com/sw/rippledoc/quick-markdown-example.html)
- 範例二)[markitdown](http://www.markitdown.net/markdown)
- 教材[ghost](https://blog.ghost.org/markdown/)

## 標題行的常用格式


利用＃符號，來定義不同的標題行， 共有6階，範例如下

## 標題2
### 標題3
#### 標題4

## 利用-＋* 等符號，來定義不同的列表行
- 列表1-1
- 列表2-1
	- 列表2-1-1
	- 列表2-1-2
		- 列表2-1-2-1
		+ 列表2-2-2-2
- 列表3-1
1. number list
2. number list
3. number list

## 利用[] 和() 符號，來定義相關的連接

- 請踴躍協助翻譯[繁中版](https://www.transifex.com/bitcoinbook/mastering-bitcoin/)
- 普林斯頓在 Coursera 開設的 [比特幣和數字貨幣技術](https://zh-tw.coursera.org/course/bitcointech)
	- 亂亂的 [筆記](http://hashedportfolio.blogspot.tw/)

## 程式碼code 的定義方式
	for filename in *
	do
		rename $filename $filename1
	done


# 分隔線

- 你可以在一行中用三個或以上的星號、減號、底線來建立一個分隔線，行內不能有其他東西。你也可以在星號中間插入空白。下面每種寫法都可以建立分隔線：

----

# 強調

Markdown使用星號（*）和底線（_）作為標記強調字詞的符號，被*或_包圍的字詞會被轉成用<em>標籤包圍，用兩個*或_包起來的話，則會被轉成<strong>，例如：

*single asterisks*

_single underscores_

**double asterisks**

__double underscores__

## 區塊引言

Markdown使用email形式的區塊引言，如果你很熟悉如何在email信件中引言，你就知道怎麼在Markdown文件中建立一個區塊引言，那會看起來像是你強迫斷行，然後在每行的最前面加上>：
> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
> consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
> Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
>
> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
> id sem consectetuer libero luctus adipiscing.

Markdown也允許你只在整個段落的第一行最前面加上>：

> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.

> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
id sem consectetuer libero luctus adipiscing.

區塊引言可以有階層（例如：引言內的引言），只要根據層數加上不同數量的>：

> This is the first level of quoting.
>
> > This is nested blockquote.
>
> Back to the first level.
引言的區塊內也可以使用其他的Markdown語法，包括標題、清單、程式碼區塊等：

> ## This is a header.
>
> 1.   This is the first list item.
> 2.   This is the second list item.
>
> Here's some example code:
>
>     return shell_exec("echo $input | $markdown_script");


# 如何修改文字顏色和大小

因為想要製作反白顯示答案的互動效果，所以需要把文字標記成白色。

<font color="red">紅色的文字<font>
<font color="yellow">黃色的的文字<font>
<font color="white"><font>

Roses are <span style="color:red">red</span>, violets are <span style="color:blue">blue</span>.

Roses are <span style="color:red; font-family:Georgia; font-size:2em;">red.</span>

# 程式碼的範例1
	if [ ! -f $cfgDir/setup.ini ] ; then
      echo "no $cfgDir/setup.ini, Please check again"
      exit
	fi
	for CODE in $(cat $cfgDir/setup.ini); do
      NAME=`echo $CODE | sed 's/=/ /g' | awk '{print $1}'`
      TYPE=`echo $CODE | sed 's/=/ /g' | awk '{print $2}'`
      VARS=`printf "${NAME}=\\${TYPE}\n"`
      eval `printf $VARS`
	done

# 程式碼的範例2
```
		if [ ! -f $cfgDir/setup.ini ] ; then
	      echo "no $cfgDir/setup.ini, Please check again"
	      exit
				fi
		for CODE in $(cat $cfgDir/setup.ini); do
	      NAME=`echo $CODE | sed 's/=/ /g' | awk '{print $1}'`
	      TYPE=`echo $CODE | sed 's/=/ /g' | awk '{print $2}'`
	      VARS=`printf "${NAME}=\\${TYPE}\n"`
	      eval `printf $VARS`
		done
```

# 程式碼內含的內容

如果要標記一小段行內程式碼，你可以用反引號把它包起來（`），例如：

Use the `printf()` function.



＃ 如何修改 Markdown 文字顏色
因為想要製作反白顯示答案的互動效果，所以需要把文字標記成白色。
所以Google了”Markdown 文字顏色”
前八個都說 Markdown 無法修改文字顏色，有一個說要用CSS
我想說不可能
最後在第九個找到答案

<font color="white">要反白的文字<font>

![m'lady]( https://hackpad-attachments.imgix.net/turboteam.hackpad.com_u3H8jjdgMWx_p.527885_1466593934326_螢幕快照%202016-06-22%20上午10.57.15.png?fit=max&w=882)
