# OpenTracker

为 [tracker.renfei.net](http://tracker.renfei.net) 提供驱动。tracker服务地址：

* [http://tracker.renfei.net:8080/announce](http://tracker.renfei.net:8080/announce)
* [https://tracker.renfei.net:443/announce](https://tracker.renfei.net:443/announce)

## 说明

针对 CentOS 7 进行的编译说明

```shell
yum -y install unzip wget gcc zlib-devel make
git clone -b renfei https://github.com/renfei/OpenTracker.git
cd OpenTracker
make -C ./libowfat-0.32
make
```

运行

复制编译好的```opentracker```可执行文件和```opentracker.conf```配置文件，我一般建议配置为服务：opentracker.service

service文件存放路径:```/usr/lib/systemd/system/opentracker.service```

```
[Unit]
Description=OpenTracker server
Wants=network-online.target
After=network.target network-online.target

[Service]
ExecStart=/root/opentracker -f /root/opentracker.conf
ExecReload=/bin/kill -s HUP $MAINPID
Restart=always
RestartSec=5
StartLimitInterval=0
StartLimitBurst=5

[Install]
WantedBy=multi-user.target
```

* systemctl daemon-reload
* systemctl status opentracker
* systemctl start opentracker
* systemctl stop opentracker
* systemctl restart opentracker
* systemctl enable opentracker

## 原文

> This is opentracker. An open bittorrent tracker.
>
> You need libowfat (http://www.fefe.de/libowfat/).
>
> Steps to go:
>
> cvs -d :pserver:cvs@cvs.fefe.de:/cvs -z9 co libowfat
> cd libowfat
> make
> cd ..
> cvs -d:pserver:anoncvs@cvs.erdgeist.org:/home/cvsroot co opentracker
> cd opentracker
> make
> ./opentracker
>
> This tracker is open in a sense that everyone announcing a torrent is welcome to do so and will be informed about anyone else announcing the same torrent. Unless
> -DWANT_IP_FROM_QUERY_STRING is enabled (which is meant for debugging purposes only), only source IPs are accepted. The tracker implements a minimal set of
> essential features only but was able respond to far more than 10000 requests per second on a Sun Fire 2200 M2 (thats where we found no more clients able to fire
> more of our testsuite.sh script).
>
> Some tweaks you may want to try under FreeBSD:
>
> sysctl kern.ipc.somaxconn=1024
> sysctl kern.ipc.nmbclusters=32768
> sysctl net.inet.tcp.msl=10000
> sysctl kern.maxfiles=10240
>
> License information:
>
> Although the libowfat library is under GPL, Felix von Leitner agreed that the compiled binary may be distributed under the same beer ware license as the source code for opentracker. However, we like to hear from happy customers.
>