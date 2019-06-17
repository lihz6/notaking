### 基本用法

```bash
$ ssh <user>@<host>
```

### 使用别名

配置 `~/.ssh/config`

```
Host lihz
  HostName 192.168.0.125
  User lihz
  Port 22

Host ngolin
  HostName www.ngolin.com
  User root
  Port 22
```

```bash
$ ssh lihz # ssh lihz@192.168.0.125
$ ssh ngolin # ssh root@www.ngolin.com
```

### 免密登录

```bash
# if `~/.ssh/id_rsa` exists, don't overwrite
$ ssh-keygen

# set `id_rsa` and `id_rsa.pub` read only
# to avoid accidential overwrited
$ chmod -w ~/.ssh/id_rsa*

$ ssh-copy-id lihz # ssh-copy-id lihz@192.168.0.125
```

### 防止掉线

配置 `/etc/ssh/ssh_config`

```
Host *
  # SendEnv LANG LC_*
  ConnectTimeout 12
  ServerAliveCountMax 1
  ServerAliveInterval 12
```
