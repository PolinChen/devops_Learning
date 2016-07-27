
### 使用scp 上傳nmon 執行文件到 iPOC的/tmp 中

```
scp ~/Downloads/nmon16d_x86.tar.gz  root@10.1.0.150:/tmp
```

### 在iPOC 中安裝nmon 文件

cd /tmp
mkdir nmon
cd nmon
tar xzf ../nmon16d_x86.tar.gz
./nmon_x86_64_centos6
cp nmon_x86_64_centos6 /usr/bin/nmon
/usr/bin/nmon
