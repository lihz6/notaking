### Jobs Control

> `Control+Z`, `jobs`, `bg`, `&`, `fg`, `disown`, `nohup`, `kill`, `Control+C`

命令执行过长时，`Control+Z` 停止，`jobs` 查看，`bg` 后台运行；或者从一开始用 `&` 在后台启动。用 `fg` 将后台进程转到前台执行。

终端关闭时所有后台进程也会关闭，除非终端 `disown -h` 进程，或者从一开始用 `nohup` 启动。

```bash

$ sleep 1000
^Z
[1]+  Stopped                 sleep 1000
$ jobs
[1]+  Stopped                 sleep 1000
$ bg %1
[1]+ sleep 1000 &
$ jobs
[1]+  Running                 sleep 1000 &
$ sleep 1000
^C
$ sleep 1000 &
[2] 12467
$ jobs
[1]-  Running                 sleep 1000 &
[2]+  Running                 sleep 1000 &
$ fg %2
sleep 1000
^Z
[2]+  Stopped                 sleep 1000
$ jobs
[1]-  Running                 sleep 1000 &
[2]+  Stopped                 sleep 1000
$ disown -h %1
$ kill %2
$ jobs
[1]-  Running                 sleep 1000 &
[2]+  Terminated              sleep 1000
$ nohup sleep 1000 &>/dev/null &
[2] 12480
$ jobs
[1]-  Running                 sleep 1000 &
[2]+  Running                 nohup sleep 1000 &> /dev/null &
```
