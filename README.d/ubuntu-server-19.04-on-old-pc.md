## 准备工作

1. 扣出主板电池重置主板
2. 开机 `DEL` 键进入 BIOS 设置，`F12` 选择启动磁盘
3. 刻录系统 U 盘使用 UltraISO, 写入方式 _RAW_

## 系统配置

### Ubuntu Server 19.04 启用 WIFI

安装 wpasupplicant

```bash
sudo apt install wpasupplicant
```

安装 ifupdown

```bash
sudo apt install ifupdown
```

查看所有可用网卡

```bash
ifconfig -a
```

编辑 `/etc/network/interfaces`

```bash
cat <<EOF | sudo tee /etc/network/interfaces
auto wlx3c46d8648605
iface wlx3c46d8648605 inet dhcp
wpa-essid mywifi
wpa-psk wifipswd
EOF
```

启用网卡

```bash
sudo ifdown wlx3c46d8648605
sudo ifup -v wlx3c46d8648605
```

### 开机耗时问题

编辑 `/etc/netplan/50-cloud-init.yaml`

```
network:
    ethernets:
        enp2s0:
            dhcp4: true
+           optional: true
    version: 2
```

```bash
sudo netplan --debug generate
sudo netplan apply
sudo reboot
```

### 设置时区：

```bash
sudo cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

### VSCode clang-format 问题

> https://github.com/microsoft/vscode-cpptools/issues/3271#issuecomment-473164129

```bash
sudo apt install libtinfo5
```
