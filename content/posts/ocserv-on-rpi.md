---
categories : []
date : "2017-06-05T00:38:14+08:00"
description : "安裝OpenConnect VPN Server紀錄"
keywords : []
title : "安裝 OpenConnect VPN Serve(ocserv) 於 raspberry pi"

---

安裝相依套件
```
apt-get install make liblz4-dev libseccomp-dev libreadline-dev libnl-route-3-dev libkrb5-dev libprotobuf-c0-dev libtalloc-dev libgnutls28-dev libwrap0-dev libpam0g-dev libhttp-parser-dev libpcl1-dev libopts25-dev autogen protobuf-c-compiler gperf liblockfile-bin nuttcp texinfo libev4 libev-devel
```

編譯libtasn1
```
wget http://ftp.gnu.org/gnu/libtasn1/libtasn1-4.7.tar.gz
tar zxf libtasn1*.gz && cd libtasn1*
./configure 
make
make install
```


編譯ocserv
```
wget ftp://ftp.infradead.org/pub/ocserv/ocserv-0.11.8.tar.xz
tar xvf ocserv-0.11.8.tar.xz
cd ocserv-0.11.8
./configure --disable-seccomp
make
make install
```

新建ocserv資料夾，並加入config檔案
```
mkdir -p /etc/ocserv
cp doc/ocserv/doc/sample.config /etc/ocserv/ocserv.config
```
參數檔範例：

```
auth = "plain[passwd=/etc/ocserv/ocpasswd]"
tcp-port = 443
udp-port = 443
run-as-user = nobody
run-as-group = daemon
socket-file = /var/run/ocserv-socket
server-cert = /etc/ocserv/certificate.pem
server-key = /etc/ocserv/private.key
isolate-workers = true
max-clients = 16
max-same-clients = 2
keepalive = 32400
dpd = 90
mobile-dpd = 1800
try-mtu-discovery = false
cert-user-oid = 2.5.4.3
tls-priorities = "NORMAL:%SERVER_PRECEDENCE:%COMPAT:-VERS-SSL3.0"
auth-timeout = 240
min-reauth-time = 300
max-ban-score = 50
ban-reset-time = 300
cookie-timeout = 600
deny-roaming = false
rekey-time = 172800
rekey-method = ssl
use-occtl = true
pid-file = /var/run/ocserv.pid
device = vpns
predictable-ips = true
ipv4-network = 192.168.0.200/20
ipv4-netmask = 255.255.255.0
dns = 8.8.8.8
dns = 8.8.4.4
ping-leases = false
#route = 10.10.10.0/255.255.255.0
#route = 192.168.0.0/255.255.255.0
#route = fef4:db8:1000:1001::/64
route = default
cisco-client-compat = true
dtls-legacy = true
```

將ocserv加入service
```bash
sudo nano /etc/systemd/system/ocserv.service
```

```
[Unit]
Description=ocserv-starup
After=network.target

[Service]
Type=oneshot
ExecStart=/usr/local/sbin/ocserv -c /etc/ocserv/ocserv.conf
ExecReload=/usr/bin/killall /usr/local/sbin/ocserv
ExecStop=/usr/bin/killall /usr/local/sbin/ocserv
PrivateTmp=true
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

開啟nat轉發，與區域網路proxy arp 
append to /etc/sysctl.conf file
```
net.ipv4.ip_forward = 1 
net.ipv4.conf.all.proxy_arp = 1
```
- net.ipv4.ip_forward 為轉發ipv4封包
- net.ipv4.conf.all.proxy_arp 使vpn client可以與lan裝置互相連線（[參考資料](http://www.infradead.org/ocserv/recipes-ocserv-pseudo-bridge.html)）


設定開機自動啟動並啟動ocserv
```bash
systemctl enable ocserv
systemctl start ocserv
```

參考資料

- [http://www.infradead.org/ocserv/recipes-ocserv-installation-generic.html](http://www.infradead.org/ocserv/recipes-ocserv-installation-generic.html)
- [http://www.infradead.org/ocserv/recipes-ocserv-pseudo-bridge.html](http://www.infradead.org/ocserv/recipes-ocserv-pseudo-bridge.html)
- [https://holmesian.org/Raspberry-Pi-ARM-install-ocserv](https://holmesian.org/Raspberry-Pi-ARM-install-ocserv)
