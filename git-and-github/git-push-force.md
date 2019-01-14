### 强制推送本地仓库

> 本地修改过已推送的提交时，用 `git push -f` 将更新推送到远程

```bash
$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean

$ git add .
$ git commit --amend

$ git push
failed

$ git push -f
sucess
```
