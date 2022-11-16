# macOSsetup
Updated 11/15/2022

This repo will describe how I set up my dev environment on a Mac.
My specs:
 - Macbook Pro 14" 
 - M1 chip, Apple Silicon, Arm-based architecture

This is pretty specific for myself, however if it's helpful for you, feel free to follow along. 

- [System update](#system-update)
- [System preferences](#system-preferences)
- [Security](#security)
- [iTerm2](#iterm2)
- [Homebrew](#homebrew)
- [Git](#git)
- [Visual Studio Code](#visual-studio-code)
- [Vim](#vim)
- [Python](#python)
- [Node.js](#nodejs)
- [Ruby](#ruby)
- [Heroku](#heroku)
- [PostgreSQL](#postgresql)
- [Redis](#redis)
- [Elasticsearch](#elasticsearch)
- [Projects folder](#projects-folder)
- [Apps](#apps)

## Update Script
First thing I do is automate any updating via a bash/zsh script, that I call "updateMe.sh"
Note: Ever since MacOS 10.15 Catalina, the default shell is zsh, not bash. 
I create a folder 'myScripts' somewhere to place these. 
```
cd ~
mkdir myScripts && cd myScripts
vim updateMe.sh
```

Inside this script I set up a script that runs through checking for updates for different things. For now, just need:

```
#!/bin/zsh

echo '~~~~~~~~ Starting update script ~~~~~~~~~~'
echo '1) checking Apple Updates'
/usr/sbin/softwareupdate -ia

exit 0

```

