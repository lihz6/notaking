### 环境准备

readline 默认的编辑模式为 `emacs`.

- 查看：`set -o | grep emacs`
- 设置：`set -o emacs`

### 快速清屏

```bash
C-l
```

### 快速注释

```bash
[1]$ sudo apt install
# (M-#) comment the line and add to history
[1]$ #sudo apt install
[2]$
```

### 快速移动

```bash
# move at line level

[1]$ ls -la /etc/wireguard[]
# (C-a) move to the start
[1]$ [l]s -la /etc/wireguard
# (C-e) move to the end
[1]$ ls -la /etc/wireguard[]


# move at word level

# (M-b) move backward a word
[1]$ ls -la /etc/[w]ireguard
# (M-b)
[1]$ ls -la /[e]tc/wireguard
# (M-f) move forward a word
[1]$ ls -la /etc[/]wireguard


# move at char level

# (C-f, →) move forward a char
[1]$ ls -la /etc/[w]ireguard
# (C-b, ←) move backward a char
[1]$ ls -la /etc[/]wireguard


# with digit-argument (M-0, M-1, ..., M--)

# (M-2 M-b) move backward two words
[1]$ ls -[l]a /etc/wireguard
# (M-4 C-f) move forward four chars
[1]$ ls -la /[e]tc/wireguard


# move by searching line for char

# (C-] `r`) move to next `r`
[1]$ ls -la /etc/wi[r]eguard
# (M-C-] ` `) move to prev ` `
[1]$ ls -la[ ]/etc/wireguard
# (M-2 M-C-] `r`) move to next 2 `r`
[1]$ ls -la /etc/wiregua[r]d


# move by searching history containing word

[1]$ ls -la /etc/wireguard
# (C-r `wire` M) search backward history containing `wire`
#                and move to the beginning of `wire`
[2]$ ls -la /etc/[w]ireguard
```

### 快速删除

```bash
# delete at line level

[1]$ echo one two [t]hree four five
# (C-k) delete to the end of the line
[1]$ echo one two []

[1]$ echo one two [t]hree four five
# (C-u) delete to the beginning of the line
[1]$ [t]hree four five


# delete between spaces

[1]$ ls -la /etc/wireguard/wg0.conf[]
# (C-w) delete backward to until a space
[1]$ ls -la []


# delete at word level

[1]$ echo one two [t]hree four five
# (M-d) delete forward a word
[1]$ echo one two [ ]four five
# (M-BACK) delete backward a word
[1]$ echo one [ ]four five

# delete at char level

# (C-d, DELETE) delete forward a char
[1]$ echo one[ ]four five
# (BACK) delete backward a char
[1]$ echo on[ ]four five


[1]$ echo 'a   [ ] b'
# (M-\) delete all spaces and tabs around
[1]$ echo 'ab'


# with digit-argument (M-0, M-1, ..., M--)


# (C-y) yank what have been deleted
```

### 快速编辑

```bash
[1]$ systemctl status nginx
[2]$ sudo systemctl stop []
# (M-.) yank last arg of the prev history
[2]$ sudo systemctl stop nginx[]


# with digit-argument (M-0, M-1, ..., M--)

[1]$ echo 1-4 2-3 3-2 4-1 5
[2]$ echo []
# (M-.) yank 5
[2]$ echo 5[]
# (M-0 M-.) yank echo
[2]$ echo echo[]
# (M-1 M-., M--4 M-.) yank 1-4
[2]$ echo 1-4[]
# (M-2 M-., M--3 M-.) yank 2-3
[2]$ echo 2-3[]
# (M-3 M-., M--2 M-.) yank 3-2
[2]$ echo 3-2[]
# (M-4 M-., M--1 M-.) yank 4-1
[2]$ echo 4-1[]


# (M-t) transpose words
# (C-t) transpose chars
# (M-u) upcase word
# (M-l) downcase word
# (M-c) capitalize word

# (M-TAB) insert a tab character
# (M-?) show possible completions
# (M-*) insert all completions
# (TAB) complete

# (C-p, ↑) prev history
# (C-n, ↓) next history

# (C-r) backward search history
# (M-, C-j) accept
# (ENTER) execute
# (C-g) cancel

# (ENTER, C-j) execute
```
