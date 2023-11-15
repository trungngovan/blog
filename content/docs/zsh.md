---
title: "Install zsh & plugins"
date: 2023-10-11T15:51:22+07:00
author: "Ngo Van Trung"
categories: ["tools", "coding"]
draft: true
---
### Install zsh in ubuntu
1. Update the system repository
```shell
sudo apt update
```

2. Install Zsh
```shell
sudo apt install zsh -y
```

3. Check installation
```shell
zsh --version
```

### Install plugins
#### Ohmyzsh & powerlever10k
{{< youtube -s3Yjzu4P4o >}}


```shell
sh-c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

git clone --depth=1 https://github.com/romkatv/powerlevell0k.git ~/powerlevel10k

echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/zshrc
```


#### fzf (fuzzy finder)
https://github.com/junegunn/fzf

#### zsh-autosuggestions
https://github.com/zsh-users/zsh-autosuggestions
