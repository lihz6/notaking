### MacOS `brew`

```bash
cd $(brew --repo)
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

cd $(brew --repo)/Library/Taps/homebrew/homebrew-core
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

cd $(brew --repo)/Library/Taps/homebrew/homebrew-cask
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git

echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile

. ~/.bash_profile
brew update
```

### `npm`

临时使用

```bash
npm --registry https://registry.npm.taobao.org install express
```

持久使用

```bash
npm config set registry https://registry.npm.taobao.org
npm config get registry
```

### Proxy

`~/.bashrc`

```bash
export http_proxy=http://<username>:<password>@<domain>:<port>
# same as http_proxy
export https_proxy=http://<username>:<password>@<domain>:<port>
export no_proxy=$(printf %s, {0..9}).local,localhost
```
