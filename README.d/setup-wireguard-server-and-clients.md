## Server: Ubuntu 18.04

### Installation

```bash
$ sudo add-apt-repository ppa:wireguard/wireguard
$ sudo apt update
$ sudo apt install wireguard

$ lsmod | grep wireguard # oops
$ sudo modprobe wireguard # activate wireguard
$ lsmod | grep wireguard # okay
wireguard             208896  0
ip6_udp_tunnel         16384  1 wireguard
udp_tunnel             16384  1 wireguard
```

### Generating Keys

```bash
cd /etc/wireguard
umask 077
wg genkey | sudo tee privatekey | wg pubkey | sudo tee publickey
```

### Configuration

```bash
$ # pick a local IPv4 subnet that is not used by other networks
$ IPv4=10.10.0.1/24
$ # pick a local IPv6 subnet that is not used by other networks
$ IPv6=fd86:ea04:1111::1/64
$ # list network interfaces
$ ifconfig # or `ip addr`
$ NINT=eth0
$ cat <<EOF | sudo tee wg0.conf
[Interface]
PrivateKey = $(sudo cat privatekey)
Address = $IPv4
Address = $IPv6
SaveConfig = true
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o $NINT -j MASQUERADE; ip6tables -A FORWARD -i wg0 -j ACCEPT; ip6tables -t nat -A POSTROUTING -o $NINT -j MASQUERADE
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o $NINT -j MASQUERADE; ip6tables -D FORWARD -i wg0 -j ACCEPT; ip6tables -t nat -D POSTROUTING -o $NINT -j MASQUERADE
ListenPort = 51820
DNS = 8.8.8.8
EOF

cat <<EOF | sudo tee -a /etc/sysctl.conf
net.ipv4.ip_forward=1
net.ipv6.conf.all.forwarding=1
EOF

sudo sysctl -p
```

### Starting Server

```bash
$ wg-quick up wg0
$ sudo wg
interface: wg0
  public key: wYHnSnbRraJNooN5gjNtH5SLS1tw/0xehAVbU9bgIxA=
  private key: (hidden)
  listening port: 51820
```

### Using systemd

```bash
$ wg-quick down wg0
$ sudo systemctl enable wg-quick@wg0
$ sudo systemctl start wg-quick@wg0
```

## Client: Ubuntu 18.04

- Installation
- Generating Keys

### Configuration

```bash
$ cd /etc/wireguard
$ ADDR4=10.10.0.5/32 # same local IP subnet with server
$ ADDR6=fd86:ea04:1111::5/128 # same local IP subnet with server
$ SRVPUB_IP=47.2.123.145
$ SRVPUBKEY=$(ssh root@$SRVPUB_IP 'cat /etc/wireguard/publickey')
$ cat <<EOF | sudo tee wg0.conf
[Interface]
Address = $ADDR4
Address = $ADDR6
PrivateKey = $(sudo cat privatekey)
DNS = 8.8.8.8

[Peer]
PublicKey = $SRVPUBKEY
Endpoint = $SRVPUB_IP:51820
AllowedIPs = 0.0.0.0/0, ::/0
EOF
```

### Add Client to Server

```bash
$ ssh root@$SRVPUB_IP "wg set wg0 peer $(sudo cat publickey) allowed-ips $ADDR4,$ADDR6"
```

### Starting Client

```bash
$ wg-quick up wg0
```

### Shutdown Client

```bash
$ wg-quick down wg0
```

### Using Alias

`$HOME/.bash_profile`

```bash
_wg0() {
  if ifconfig wg0 &>/dev/null; then
    export PS1="*$PS1"
    alias wg0='if wg-quick down wg0 &>/dev/null; then export PS1=${PS1:1}; _wg0; fi'
  else
    alias wg0='if wg-quick up wg0 &>/dev/null; then _wg0; fi'
  fi
}

_wg0
```

- Using systemd

## Client: MacOS 10.14

### Installation

```bash
brew install wireguard-tools
```

- Generating Keys
- Configuration
- Add Client to Server
- Starting Client

### Using Alias

`$HOME/.bash_profile`

```bash
_wg0() {
  if ifconfig utun1 &>/dev/null; then
    export PS1="*$PS1"
    alias wg0='if wg-quick down wg0 &>/dev/null; then export PS1=${PS1:1}; _wg0; fi'
  else
    alias wg0='if wg-quick up wg0 &>/dev/null; then _wg0; fi'
  fi
}

_wg0
```

## Note

```bash
# 端口转发，服务器中转
$ iptables -t nat -A PREROUTING -p udp --dport 51xxx -j DNAT --to-destination 149.xxx.xxx.xxx:51xxx

# 经典网络用公网 IP, 专有网络用私网 IP
$ iptables -t nat -A POSTROUTING -p udp -d 149.xxx.xxx.xxx --dport 51xxx -j SNAT --to-source 39.xxx.xxx.xxx # 公网 IP
$ iptables -t nat -A POSTROUTING -p udp -d 149.xxx.xxx.xxx --dport 51xxx -j SNAT --to-source 10.xxx.xxx.xxx # 私网 IP

# 保存规则
$ sudo apt-get install iptables-persistent
$ sudo netfilter-persistent save
$ sudo netfilter-persistent reload
```

## Trouble Shooting

##### 1. When `wg-quick up wg0`, `/usr/bin/wg-quick: line 31: resolvconf: command not found`

- `sudo apt install resolvconf`

##### 2. `RTNETLINK answers: Permission denied`

- edit `/etc/sysctl.conf`
- set `*.disable_ipv6` to `0`
- `sudo sysctl -p`
