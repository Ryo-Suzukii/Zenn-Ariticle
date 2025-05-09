---
title: "Macbookの初期設定備忘録"
emoji: "🗒️"
type: "idea"
topics:
  - "mac"
  - "備忘録"
published: true
published_at: "2025-05-04 15:56"
---

これやったらとりあえず一緒

## brewのinstall
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo >> /Users/$USER/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/$USER/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

## brewを使ったinstall
```
brew install cfn-lint awscli aws-sam-cli docker arc 1password wget ghostty visual-studio-code pyenv notion poetry mas bettertouchtool
```
## masを使ったinstall
```
mas install 1429033973 539883307
```

# oh my zshの設定
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-autosuggestions
git clone https://github.com/MichaelAquilina/zsh-you-should-use.git $ZSH_CUSTOM/plugins/you-should-use
```

あとは今度やる時ちゃんとメモする
