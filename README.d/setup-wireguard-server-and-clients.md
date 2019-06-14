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

- Starting Client
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

## Trouble Shooting

##### 1. When `wg-quick up wg0`, `/usr/bin/wg-quick: line 31: resolvconf: command not found`

- `sudo apt install resolvconf`

##### 2. `RTNETLINK answers: Permission denied`

- edit `/etc/sysctl.conf`
- set `*.disable_ipv6` to `0`
- `sudo sysctl -p`
