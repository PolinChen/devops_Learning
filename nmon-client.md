# nmon client operation 


## install nmon in your system
- sudo cp nmon-bianry to /usr/bin/nmon 
- mkdir /opt/nmons
- creaate nmons log collect scripts (v30-nmons-collect.sh)
- send nmons log to ftp server (v30-nmons-xfer.sh)
- create crontab for nmon (crontab -e)
- check nmon log in client
- check nmon log in ftp server


### creaate nmons log collect scripts

```
$ cat /opt/nmons/v30-nmons-collect.sh
#!/bin/bash
nmonDir="/opt/nmons"
host=`hostname | awk -F"." '{print $1}'`
DAY=`date +%Y%m%d`
#/usr/bin/nmon -f -t -s 60 -c 1440
/usr/bin/nmon -f -t -s 60 -c 1440 -m $nmonDir
exit
```

### send nmons log to ftp server

```
$ cat /opt/nmons/v30-nmons-xfer.sh
#!/bin/bash
################################################
### cp /nmon/today.nmon to 10.5.160.225 /home/ttpoc
################################################
nmonDir="/opt/nmons"
TODAY=`date +%Y%m%d`
today=`date +%y%m%d`
YESTERDAY=`date -d "yesterday" +%Y%m%d`
yesterday=`date -d "yesterday" +%y%m%d`
HOST="00.000.000.000"
USER="nmons"
PASS="xxx.nmons"
################################################################
### ftp /opt/nmons/*.nmon to iPOC_ftp /home/ttpoc
################################################################
cd $nmonDir
ls -al *.nmon > nmonlist
for dfile in $(awk '{if($5 >0) {print $NF}}' nmonlist);
do
printf "."
ftp -in $HOST << ABC
user $USER $PASS
pass
binary
put $dfile
bye
ABC
done

if [ ! -d $nmonDir/backup ] ; then
    mkdir $nmonDir/backup
fi

if [ -f $nmonDir/*_$yesterday*.nmon ] ; then
    tar czf $nmonDir/backup/nmon_$yesterday.tar.gz *_$yesterday*.nmon
    mv  $nmonDir/*_$nmondate*.nmon $nmonDir/backup
    rm  $nmonDir/backup/*_$nmondate*.nmon
fi

```


### create crontab for nmon (crontab -e)

```
## crontab -l
0 0 * * * /opt/nmons/v30-nmons-collect.sh
9,19,29,39,49,59 * * * * /opt/nmons/v30-nmons-xfer.sh
```

### check the nmon log in client

### check the nmon log in ftp-server
