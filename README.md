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
- [Homebrew](#homebrew)
- [Oh My Zsh] (#install-oh-my-zsh)
- [Updating The Terminal](#update-the-terminal)
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
- Update the Terminal

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


## Homebrew
Homebrew is probably the most popular package manager for MacOs. 
We need to install the Command Line Developer Tools for Xcode first. You do not need Xcode to use Homebrew, however some of the software and components you'll want to install will rely on Xcode's Command Line Tools package
```
xcode-select --install
```
Then install homebrew, you can find the command on Homebrew's webpage or below:
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

You may see a warning:

warning: /opt/homebrew/bin is not in your PATH

That's okay. We address this in the "Update The Terminal" Section. 

Once installation is complete, you can run the following command to make sure everything works:

```
brew doctor
```

This is the list of all packages installed on my Mac, currently:
<img width="1343" alt="image" src="https://user-images.githubusercontent.com/19870859/202926967-91b16bb8-d6b9-4147-bc26-1f4564a49c6d.png">


## Install Oh My Zsh

```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```


## Update The Terminal
Ever since apple switched the default shell to zsh instead of bash, I found it's unnecessary to install iTerm/iTerm2. The terminal naturally defaults to the zsh shell, but if you want to switch to bash, there are plenty of articles on google that show you how. I like zsh, so we'll run with it. 

First things first, we should understand where our paths are. 

```
echo $PATHS
```
you should see what directories your terminal looks through for binaries, links, etc, **in order** of what it looks through first, from left to right. I like to familiarize myself with this list. 

Where does the OS pull this from?
It typically pulls paths from these guys (This is different if your shell is bash ( ~/.bashrc ), fish ( ~/fish/config.fish ), etc):
- ~/.zshrc
- /etc/paths
- /etc/paths.d  
- /etc/profile 


