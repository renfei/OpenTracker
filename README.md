# OpenTracker

为 [tracker.renfei.net](tracker.renfei.net) 提供驱动。tracker服务地址：

* http://tracker.renfei.net:8080/announce
* udp://tracker.renfei.net:8080/announce

## 编译

基于Centos

```shell
yum -y install unzip wget gcc zlib-devel make
wget https://github.com/renfei/OpenTracker/archive/master.zip -O ~/OpenTracker.zip
cd ~
unzip OpenTracker.zip
cd ~/OpenTracker-master/libowfat
make
cd ~/OpenTracker-master/opentracker
make
cp ~/OpenTracker-master/opentracker/opentracker ~/opentracker
```

## 启动

运行程序，并且监听tcp和udp端口的8080，并且自动后台工作

```shell
cd ~
nohup ./opentracker -p 8080 -P 8080 > opentracker.log & tail -f opentracker.log
```

查看进程当前并发连接数

```shell
netstat -apn|grep opentracker |wc -l
```

查看系统当前网络情况

```shell
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
```

通过浏览器访问程序的统计功能

* http://ip:8080/stats
* http://ip:8080/stats?mode=everything
* http://ip:8080/stats?mode=top100
* http://ip:8080/scrape

