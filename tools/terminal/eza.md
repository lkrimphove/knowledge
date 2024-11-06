---
title: eza
draft: true
tags:
  - terminal
---
# eza

- modern, maintained replacement for the venerable file-listing command-line program `ls`

![[eza.png]]
## Install
[[homebrew]]:
```bash
brew install eza
```

apt:
```
sudo apt install -y gpg

sudo mkdir -p /etc/apt/keyrings
wget -qO- https://raw.githubusercontent.com/eza-community/eza/main/deb.asc | sudo gpg --dearmor -o /etc/apt/keyrings/gierens.gpg
echo "deb [signed-by=/etc/apt/keyrings/gierens.gpg] http://deb.gierens.de stable main" | sudo tee /etc/apt/sources.list.d/gierens.list
sudo chmod 644 /etc/apt/keyrings/gierens.gpg /etc/apt/sources.list.d/gierens.list
sudo apt update
sudo apt install -y eza
```

## Alias
> [!important]
> You have to install and use a font with icon support for the icons to work. Use any NerdFont.
> ```
> brew install --cask font-meslo-for-powerlevel10k
> ```

Add to `.zshrc` or `.bashrc`:
```
alias ls='eza --color=always --group-directories-first --icons'
alias ll='eza -la --icons --octal-permissions --group-directories-first'
alias l='eza -bGF --header --git --color=always --group-directories-first --icons'
alias llm='eza -lbGd --header --git --sort=modified --color=always --group-directories-first --icons' 
alias la='eza --long --all --group --group-directories-first'
alias lx='eza -lbhHigUmuSa@ --time-style=long-iso --git --color-scale --color=always --group-directories-first --icons'

alias lS='eza -1 --color=always --group-directories-first --icons'
alias lt='eza --tree --level=2 --color=always --group-directories-first --icons'
alias l.="eza -a | grep -E '^\.'"
```

---

## Resources
- https://github.com/eza-community/eza
- https://gist.github.com/AppleBoiy/04a249b6f64fd0fe1744aff759a0563b