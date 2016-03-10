---
layout: post
title: "Customize your terminal"
date: 2015-12-05 22:46:11 +0800
comments: true
categories: programming
author: Eric Zhang
---

## 1. install `zsh`:
```
# install zsh-5.0.7
# prerequisite: gcc ncurses-devel readline-devel pcre-devel zlib-devel
curl -L http://jaist.dl.sourceforge.net/project/zsh/zsh/5.0.7/zsh-5.0.7.tar.bz2 | tar jx
cd zsh-5.0.7/
./configure --prefix=/usr/local/zsh/5.0.7 --enable-cap --enable-pcre --enable-multibyte --disable-gdbm
make
sudo make install
sudo alternatives --install /usr/local/bin/zsh zsh /usr/local/zsh/5.0.7/bin/zsh 50000
```


## 2. set the default shell to `zsh`:
### 2.1 update the `/etc/shells`, append `zsh` path there.
```
/bin/sh
/bin/bash
/sbin/nologin
/usr/bin/sh
/usr/bin/bash
/usr/sbin/nologin
/bin/tcsh
/bin/csh
/usr/local/bin/zsh
/usr/local/zsh
```

### 2.2 change the default shell to `zsh`
```
sudo chsh -s `which zsh`
sudo shutdown -r 0
```

## 3. Install the `oh-my-zsh`
https://github.com/robbyrussell/oh-my-zsh
```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
## 4. cutomize the theme of terminal
http://www.if-not-true-then-false.com/2012/solarized-linux/

```
git clone https://github.com/sigurdga/gnome-terminal-colors-solarized.git
gnome-terminal-colors-solarized/install.sh
```

-- EOF --


