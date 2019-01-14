### 动态终端工作目录

> 在 MacOS, 启动终端时自动 `cd` 到 Finder 已打开的目录

在 `~/.bash_profile` 加入：

```bash
if [ "$(pwd)" = "$HOME" ]; then
  cd $(osascript -e 'tell application "Finder"' -e "${1-1} <= (count Finder windows)" -e "get POSIX path of (target of window ${1-1} as alias)" -e 'end tell' 2>/dev/null || echo ~/Documents)
fi
```