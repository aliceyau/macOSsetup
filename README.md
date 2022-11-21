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
- [Text Editor](#text-editor)
- [Oh My Zsh](#install-oh-my-zsh)
- [Updating The Terminal](#update-the-terminal)
- [Git](#git)
- [Visual Studio Code](#vs-code)
- [Python](#python)
- [Pip](#pip)
- [Node.js](#nodejs)

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

I add the following to my updateMe.sh script
```
echo '2) HOMEBREW '
brew update
brew upgrade
brew cleanup
```

## Text Editor
If you like to use vim, feel free to skip this section. 
Vim, to me, is a great text editor for quick edits. It's low level, efficient, has a low-memory footprint, and highly configurable. I could go on and on. However, sometimes I want something more intuitive, and tangible visually. So I like to use Sublime. 
We can download Sublime straight from the website. It will install as an application. To use it:

```
/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl
```

Obviously we don't want to type all that out just to use Sublime, so we can make a softlink to that location, inside one of the directories that 'echo $PATH' spit out. I'm going to add the link to my /usr/local/bin

```
cd /usr/local/bin
ln -sf /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl subl   
```
now we can just type

```
subl
```
or 
```
subl <file>
```
to open up Sublime. 

## Install Oh My Zsh
Oh my zsh is an open source framework to manage our Zsh shell configuration - it has some nice plug ins and themes. If you are using any other shell (bash/fish/etc) please skip this. 
```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
I add the following to my updater script:

```
echo '3) ZSH '
source $ZSH/oh-my-zsh.sh
omz update
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
- /etc/zprofile 

Let's add homebrew to our path.
There are different arguments that tell you where to put your PATH, and you'll see arguments for and against all of them. These are the most common locations you'll see: 
- .zshenv
  - .zshenv is always sourced first. It often contains exported variables that should be available to other programs. For example, $PATH, $EDITOR, and $PAGER are often set in .zshenv. Also, you can set $ZDOTDIR in .zshenv to specify an alternative location for the rest of your zsh configuration. Be aware when setting $PATH in .zshenv, various other files all are sourced after this file that will override this value
- .zprofile
  - .zprofile is for login shells. It is basically the same as .zlogin except that it's sourced before .zshrc whereas .zlogin is sourced after .zshrc. According to the zsh documentation, ".zprofile is meant as an alternative to .zlogin for ksh fans; the two are not intended to be used together, although this could certainly be done if desired."
- .zshrc
  - .zshrc is for interactive shells. You set options for the interactive shell there with the setopt and unsetopt commands. You can also load shell modules, set your history options, change your prompt, set up zle and completion, et cetera. You also set any variables that are only used in the interactive shell (e.g. $LS_COLORS)
- .zlogin
  - .zlogin is for login shells. It is sourced on the start of a login shell but after .zshrc, if the shell is also interactive. This file is often used to start X using startx. Some systems start X on boot, so this file is not always very useful.
- .zlogout (not used for path, but adding here for knowledge purposes)
  - .zlogout is sometimes used to clear and reset the terminal. It is called when exiting, not when opening.   

If you want to see all the arguments, feel free to google. For myself, I'm just putting the path into my .zshrc file. 
To add it to your ~/.zshrc file

```
subl ~/.zshrc
```
and add " export PATH=/opt/homebrew/bin:$PATH " to the section that mentions your path (typically the first section) of the zshrc file. 

<img width="448" alt="image" src="https://user-images.githubusercontent.com/19870859/203116233-218d5012-16f1-4124-bd80-d2d42bcdfb78.png">


### Themes
You can look online for Oh My Zsh themes. Personally, I really like the 'Jonathan', which I customize myself. 
It's simple and clean. Another good theme is the agnoster theme.

<img width="856" alt="image" src="https://user-images.githubusercontent.com/19870859/203117976-ff7e638d-3222-480b-afad-f90f6978d356.png">

To customize a theme go to your Oh My Zsh installation location, find the theme you want to customize, and make a copy:
```
cd ~/.oh-my-zsh/themes
cp jonathan.zsh-theme jonathan_custom.zsh-theme
subl jonathan_custom.zsh-theme
```
Go crazy. 

and remember to add the theme to your .zshrc file
<img width="482" alt="image" src="https://user-images.githubusercontent.com/19870859/203126091-8724abc4-1389-4569-9d0c-35f47e6b9d79.png">

### Auto Suggestions
I really like auto-suggestions. It's like how google gives me suggestions as I type.
```
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions
subl ~/.zshrc
```
add 
```
source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh
```
somewhere in your ~/.zshrc file. 
Then: 
```
source ~/.zshrc
```

### Peco
"peco can be a great tool to filter stuff like logs, process stats, find files, because unlike grep, you can type as you think and look through the current results." - [Peco Github Description](https://github.com/peco/peco)
To install, use brew:
```
brew install peco
```

To use it, use your terminal commands, and pipe it to peco:
```
history | peco
```

### Silver Surfer
"A code searching tool similar to ack, with a focus on speed." - [ilver Surfer Github](https://github.com/ggreer/the_silver_searcher)
To install, use brew:
```
brew install the_silver_searcher
```

To use it, it follows the pattern:
```
ag [FILE TYPE] [OPTIONS] Pattern [PATH]
```

### Exa
"exa is a modern replacement for the venerable file-listing command-line program ls that ships with Unix and Linux operating systems, giving it more features and better defaults. It uses colours to distinguish file types and metadata. It knows about symlinks, extended attributes, and Git. And it’s small, fast, and just one single binary." - [Exa Github](https://github.com/ogham/exa)
To install, use brew:
```
brew install exa
```

To use it, it follows the pattern 
```
exa [OPTIONS]
```
To see all the options, go to their website, or the github read me.


### Aliasing
Now for some fun things. This is entirely personalizable. Add aliases to your ~/.zshrc file, make them something easy to remember! Auto suggestions can help you out too, if you ever forget your alias. You can also just come back to you ~/.zshrc file to check.

Some aliases that I have: 
```
alias fixZsh="subl ~/.zshrc"
alias doneFixZsh="source ~/.zshrc"
alias searchHistory="history | peco"
alias searchProcess="ps -aux | peco"
alias searchFile="ag -l | peco"
alias see="exa -Glah --icons"
alias seeR="exa -GlahR --icons"
```

## Git
The Mac OS comes with its own preinstalled version of Git, but it's typically recommended to install our own so that we can upgrade it and not interfere with the system version. 
```
brew install git
```
which, when you do
```
which git
```
it should output
<img width="194" alt="image" src="https://user-images.githubusercontent.com/19870859/203130792-46eb0e9d-7f2f-4dd7-bdb0-326b8f0def56.png">

if not, try 
```
source ~/.zshrc
```
and check again. 

Now you can configure your git settings in a gitconfig file
```
subl ~/.gitconfig
```
Here you can add aliases, color schemes, define the user, exclude files (like .gitingore for example).
Tip: On a Mac, it is important to remember to add .DS_Store (a hidden macOS system file that's put in folders) to your project .gitignore files. You also set up a global .gitignore file, located for instance in your home directory (but you'll want to make sure any collaborators also do it)



## VS Code
Visual Studio Code is a good option for IDE. You can download it from the website, and drag it into the applications folder. 
I keep this guy in my doc, as it's my go-to IDE. 

To modify some preferences, go to Code -> Preferences -> Settings -> Open JSON Settings
<img width="106" alt="image" src="https://user-images.githubusercontent.com/19870859/203132239-30702d26-201a-436b-94b9-5c5e92fc4e7b.png">

This is where you can define your default Python interpreter, extension settings, and editor settings (tab size, enable previews, etc)



## Python
The Mac OS comes with its own preinstalled version of Python, but it's typically recommended to install our own so that we can upgrade it and not interfere with the system version, as some system tools rely on it. It's also recommended to install our own version of Python using a virtual environment, that way we can handle and manage different versions of Python with different projects more gracefully. 
Let's first install the dependencies:

```
brew install openssl readline sqlite3 xz zlib
```
And install pyenv

```
brew install pyenv
```
To see what python versions are available to us, we can run

```
pyenv install --list
```

To install a python version (for example, 3.11.0), run

```
pyenv install 3.11.0
```

To see what verions you have locally installed:

```
pyenv versions
```

To see what you are running

```
python --version
```
Now at this moment, the python version that is running may still be your system python version (Mac OS, located in /usr/bin). The system itself, does not know that pyenv has other versions either. 

What we want to do right now is add the pyenv shims folder into our path, which I will do in my .zshrc file. You can add it elsewhere, wherever you feel comfortable (see Updating the Terminal section for more information). 

```
subl ~/.zshrc
```
and add 
$(pyenv root)/shims
to your path. I have it before my homebrew path:
<img width="430" alt="image" src="https://user-images.githubusercontent.com/19870859/203137894-da5f4f58-dbe3-4e6b-ba4c-09bee7883b40.png">

make sure to source ~/.zshrc to see the changes take effect.


Usage: 
In a project directory, you can use
```
pyenv local 3.X.X
```
to save that project's python version to a .python-version file. When you enter that project's directory from a terminal, pyenv will load that version for you. 

If you want to set up a global default python version, you can do 
```
pyenv global 3.X.X
```
## Pip
Pip3 should have been installed by pyenv. This is the package manager for Python.

I aliased pip='pip3' for ease in my ~/.zshrc

## Node.js
We should install Node.js with nvm (Node Version Manager), so that we can manage different versions of Node.js for different projects. 

To install/update we can run an [install cURL command](https://github.com/nvm-sh/nvm#install--update-script)

Once that's done, we can check the version that was installed: 
```
nvm -v
```

We can view available versions of Node with:
```
nvm ls-remote --lts
```
and install the latest Node version with
```
nvm install node
```
You can see which versions you have by
```
nvm ls
```
Then you can change versions by doing:
```
nvm use XX
```

### npm
Installing Node also installs its package manager, npm

