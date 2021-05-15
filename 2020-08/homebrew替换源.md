## 记录一下

对于 homebrew，需要替换的是4个模块的镜像：HomebrewHomebrew CoreHomebrew-bottlesHomebrew Cask而最全的是 USTC（中科大镜像），其他家都缺少第4个，所以换了前三个后，执行 brew update 依然慢的要死; 具体替换方式如下：

# 替换 Homebrew
```
git -C "$(brew --repo)" remote set-url origin https://mirrors.ustc.edu.cn/brew.git
```

# 替换 Homebrew Core
```
git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
```

# 替换 Homebrew Cask
```
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git
```

# 替换 Homebrew-bottles
## 对于 bash 用户：
```
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile
```

## 对于 zsh 用户：

```
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```
