```bash
$ sudo apt install pptpd

$ sudo vim /etc/pptpd.conf
- #localip 192.168.0.234-238,192.168.0.245
- #remoteip 192.168.1.234-238,192.168.1.245
+ localip 10.10.10.1
+ remoteip 10.10.10.10-99

$ sudo vim /etc/ppp/pptpd-options
+ ms-dns 8.8.8.8
+ ms-dns 8.8.4.4

$ sudo vim /etc/ppp/chap-secrects
+ <user> * <password> *

$ sudo systemctl restart pptpd

$ sudo vim /etc/sysctl.conf
- #net.ipv4.ip_forward = 1
+ net.ipv4.ip_forward = 1
$ sudo sysctl -p

$ iptables -A INPUT -p gre -j ACCEPT
$ iptables -A INPUT -p tcp --dport 1723 -j ACCEPT
$ iptables -A INPUT -p tcp --dport 47 -j ACCEPT

$ iptables -t nat -A POSTROUTING -s 10.10.10.1/24 -o eth0 -j MASQUERADE
$ iptables -A FORWARD -p tcp --syn -s 10.10.10.1/24 -j TCPMSS --set-mss 1356
```