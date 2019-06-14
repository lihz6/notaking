### Jobs Control

> `Control+Z`, `jobs`, `bg`, `&`, `fg`, `disown`, `nohup`

命令执行过长时，`Control+Z` 停止，`jobs` 查看，`bg` 后台运行；或者从一开始用 `&` 在后台启动。用 `fg` 将后台进程转到前台执行。

终端关闭时所有后台进程也会关闭，除非终端 `disown -h` 进程，或者从一开始用 `nohup` 启动。

```bash
$ sleep 100
^Z
[1]+  Stopped                 sleep 100
$ jobs
[1]+  Stopped                 sleep 100
$ bg %1
[1]+ sleep 100 &
$ jobs
[1]+  Running                 sleep 100 &
$ sleep 100 &
[2] 2880
$ jobs
[1]-  Running                 sleep 100 &
[2]+  Running                 sleep 100 &
$ fg %2
sleep 100
^Z
[2]+  Stopped                 sleep 100
$ jobs
[1]-  Running                 sleep 100 &
[2]+  Stopped                 sleep 100
$ disown -h %1
$ nohup sleep 100 1>/dev/null &
[3] 2881
```
