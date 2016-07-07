# linux nmon 和 top 的log 收集方案

> 利用IBM linux 的nmon 命令和linux 自帶的top 命令收集performance log， 並且利用ftp定時發送log 到 log server 中。

## linux log server 的系統結構

![設定的內容](/img/linux_ftp.png)

## 收集nmon&top log 的shell script: 
```
$ cat /opt/nmons/nmons-collect.sh
#!/bin/bash 
nmonDir="/opt/nmon"
nmonTool=/opt/nmon/nmon_linux_x86 
#nmonDir=`cat /etc/passwd | grep ^nsadmin | awk -F ':' '{ print $(NF -1) }'` 
#nmonTool=$adpath/nmon/nmon_x86_64_ubuntu1104 

echo $nmonDir 
host=`hostname | awk -F"." '{print $1}'` 
today=`date +%y%m%d` 
count=1440
top -d 60 -n $count -b -i > $nmonDir/$host\_$today.top & 
$nmonTool -f -N -t -s 60 -c $count -m $nmonDir

```

## nmon&top log 傳檔程式 


```
$cat /opt/nmons/nmons-xfer.sh
#!/bin/bash
################################################
nmonDir="/opt/nmons"
today=`date +%y%m%d`
yesterday=`date -d "yesterday" +%y%m%d`
day30ago=`date -d " - 30 days " +%y%m%d` 
HOST="XXX.YYY.AAA.BBB"
USER="nmons"
PASS="poc.nmons"
################################################################
### ftp /opt/nmon/*.nmon  & *.top 
################################################################
cd $nmonDir
ls -al *.nmon > ftplists
ls -al *.top >> ftplists
for uploadfile in $(awk '{if($5 >0) {print $NF}}' ftplists);
do
ftp -in $HOST << ABC
user $USER $PASS
pass
binary
put $uploadfile
bye
ABC
done

if [ ! -d $nmonDir/sent ] ; then
	mkdir $nmonDir/sent
fi
if [ -f $nmonDir/*_$yesterday*.nmon ] ; then
        echo "mv $nmonDir/*_$yesterday*.nmon $nmonDir/sent "
        mv $nmonDir/*_$yesterday*.nmon $nmonDir/sent 
        mv $nmonDir/*_$yesterday*.top $nmonDir/sent 
fi
if [ -f $nmonDir/*_$day30ago*.nmon ] ; then
        rm $nmonDir/sent/*_$day30ago*.nmon 
        rm $nmonDir/sent/*_$day30ago*.top
fi

exit 
```

## 定時任務crontab 的設定方式
```
$ crontab -e
0 0 * * * /opt/nmons/nmons-collect.sh		### 每天0點0分啟動收集
9,19,29,39,49,59 * * * * /opt/nmons/nmons-xfer.sh	### 每小時發送6次
```
