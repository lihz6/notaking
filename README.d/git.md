## `.gitignore`

- Files already tracked by Git are not affected.
- A line starting with `#` serves as a comment.
- `/<pattern>` align to where `.gitignore` exists.
- `<pattern1>/<pattern2>` equals `/<pattern1>/<pattern2>`
- `<pattern>` matches both files and dirs, `<pattern>/` only dirs.
- `*` matches any zero or more characters except `/`.
- `<pattern>/**/` matches every dirs inside.
- `<pattern>/**` matches every files and dirs inside.
- `<pattern1>/**/<pattern2>` matches zero or more dirs.
- `!<pattern>` negates any matching file if its parent not ignored.

> Note: There is usage of `?` and `[a-zA-Z]`. See `man gitignore`.

## Working with Git

### 创建并提交

```bash
# Step 1
$ git clone [--depth=1] path.to.<repo>.git
$ cd <repo>

$ mkdir <repo> && cd <repo>
$ git init

# Step 2
$ git add .

# Step 3
$ export EDITOR=vim
$ git commit [-m <message>]
```

### 创建并切换分支

```bash
$ git branch <new_branch>
$ git checkout <new_branch>

$ git checkout -b <new_branch>

$ git checkout --orphan <new_branch_with_empty_history>
```

### 分支合流

```bash
$ git merge <branch>
```

### 分支拼接

```bash
$ git rebase <branch>
```

### 移动 HEAD

```bash
$ git checkout <branch>|<tag>|<commit_hash[:6]>

$ git checkout <branch>^[m]{n}|<tag>^[m]{n}|HEAD^[m]{n}|<commit_hash[:6]>^[m]{n}

$ git checkout <branch>~n|<tag>~n|HEAD~n|<commit_hash[:6]>~n
```

### 移动分支

```bash
$ git branch -f <branch> <commit_position>
```

### 回退分支

```bash
$ git checkout <branch>
$ git reset <commit_position>
```

### 抵消分支

```bash
$ git checkout <branch>
$ git revert <commit_position>
```

### 将<commit>依次复制提交

```bash
$ git cherry-pick <commit_position1> <commit_position2>...
```

### 交互式 rebase

```bash
$ git rebase -i <to_commit_position>
```

### 修改 commit

```bash
$ git commit --amend [-m <message>]
```

### 打 TAG

```bash
$ git tag <new_tag> <commit_position>
```

### 更新远程分支

```bash
$ git fetch [--all]
```

### 更新远程分支并 merge

```bash
$ git pull
```

### 更新远程分支并 rebase

```bash
$ git pull --rebase
```

### 推送远程分支

```bash
$ git push [--force]
```

### 远程追踪

```bash
$ git checkout -b <branch> [<remote>/<branch>]
$ git checkout -b <new_branch> <remote>/<branch>

$ git branch -u <remote>/<branch> [<branch>]

$ git remote add <remote> path.to.repo.git
$ git push -u <remote> <branch>
```

### 同步 `--force`

```bash
$ git push --force
```

```
$ git fetch --all
$ git branch -a
$ git reset --hard <remote>/<branch>
```

## Teamwork with Git

```bash
$ git clone --depth=1 https://github.com/git/git.git
# Fork https://github.com/git/git.git
# into https://github.com/$USER/git.git
$ git remote add $USER https://github.com/$USER/git.git
$ git config remote.pushDefault $USER

$ git checkout master
$ git checkout -b <new_feature>
$ git branch --edit-description
$ git push -u

$ git stash -u
$ git stash pop

$ git checkout master
$ git pull # from origin/master

$ git checkout master
$ git push # to $USER/master
```

```bash
alias git-branch='git branch | while read line; do
    desc=$(git config branch.$(echo "$line" | sed "s/\* //g").description)
    printf "%-8s\t\t$desc\n" "$line"
done'
```
