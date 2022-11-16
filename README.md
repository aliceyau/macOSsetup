# macOSsetup
Updated 11/15/2022

This repo will describe how I set up my dev environment on a Mac.
My specs:
 - Macbook Pro 14" 
 - M1 chip, Apple Silicon, Arm-based architecture
 - MacOS Ventura 13.0.1

This is pretty specific for myself, however if it's helpful for you, feel free to follow along. 

- [Automatic Updates](#update-script)
- [System Settings](#system-settings)
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

Inside this script I set up a script that runs through checking for updates for different things. For now, I just need:

```
#!/bin/zsh

echo '~~~~~~~~ Starting update script ~~~~~~~~~~'
echo '1) checking Apple Updates'
/usr/sbin/softwareupdate -ia

exit 0

```
Update your mac if you see that there are updates. 


## System Settings
Obviously we want to customize the system to our liking. Again, these are specific to me. Feel free to ignore this section.

- Notifications -> Remove all notifications except for Mail, Messages, FaceTime, Reminders, Calendar, FindMy, and Wallet
- Trackpad -> Tracking Speed -> Fast
- Trackpad -> Click -> Light
- Trackpad -> Secondary Click -> Bottom Right Corner
- Trackpad -> Tap to click -> Enabled
- Keyboard -> Key Repeat Rate -> fast
- Keyboard -> Delay until repeat -> Short
- Privacy & Security -> File Vault -> Turned on
- AppleID -> FindMy -> Find My mac -> Turned on
- Lock Screen -> Require Password after screen saver begins or when display turns off -> Immediately
