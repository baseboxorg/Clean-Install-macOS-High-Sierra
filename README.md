# Clean Install macOS High Sierra

If you like to follow these instructions keep in mind that you need to use your own information (like username and folder locations).


---

## Create Bottable USB Disk

- [Download macOS High Sierra](https://itunes.apple.com/tr/app/macos-high-sierra/id1246284741?mt=12)
- Format USB (min 8GB):
    - Open **Disk Utility**
    - Click **Erase**
    - Select **OS X Extended (Journaled)**
    - Rename USB to **Untitled**
- To Create a OS X bootable USB disk enter this command to your terminal:

```bash
sudo /Applications/Install\ macOS\ High\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/Untitled
```

### Backup
Backup Your files to Dropbox.

*don't forget your your ssh keys:*

```bash
cp -R ~/.ssh ~/Dropbox/
cp -R ~/Dotfiles/private ~/Dropbox/private
```

---

## Start Clean Install

***YOU ARE GOING TO DELETE EVERYTHING ON YOUR MAC IN THE NEXT STEP !!!***

Write down the next 4 steps. You won't be able to to open your Mac until OS is installed.

1. Restart & hold down the Option **⌥ alt** key.
2. Choose **Install OS X High Sierra**.
3. Select **Disk Utility** from the menu and **erase** you main HDD.
4. Go back to the main menu; select **Install OS X** and choose your HDD when prompted.

Continue installation with your credentials.


### Restore your files

- Download [Dropbox](https://www.dropbox.com/downloading) and install it. 

- Login to your Acoount. 

- The application will create a Dropbox folder and begin downloading files from your account. Stop the download immediately by clicking the Dropbox icon in your menu bar, clicking the gear icon, and selecting ***Pause Syncing*** from the menu.

- Copy the files from your portable drive into the Dropbox folder. The default location of the folder on your Mac is /Users/yourUserName/Dropbox (such as /Users/Robert/Dropbox).

- Resume syncing by clicking the Dropbox icon in your menu bar, clicking the gear icon, and selecting ***Resume Syncing*** from the menu.

	*[Source](https://www.dropbox.com/en/help/1941)*

### Install Brew

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Install Brew Packages (instal.sh)

```bash
brew install bash bash-completion coreutils curl fabric gettext git git-extras grep guetzli jpeg libffi libpng libyaml mysql node openssl openssl@1.1 pcre peco readline ssh-copy-id stormssh stormssh-completion tree wget xz youtube-dl

#Restart Terminal
```

### Restore `.ssh` files 

```bash
cd
mkdir ~/.ssh
cd Dropbox/.ssh 
cp id_rsa* ~/.ssh/
ln -s ~/Dropbox/Dotfiles/configs/ssh_config ~/.ssh/config
```

Set Permissions 

```bash
chmod g-r,o-r id_rsa 
```


### Install [Dotfiles](https://github.com/vigo/dotfiles-light)
 

```bash
git clone https://github.com/vigo/dotfiles-light.git $HOME/Dotfiles
bash $HOME/Dotfiles/scripts/install.bash
#Restart Terminal

cd Dotfiles/
git remote rename origin upstream 
git remote add origin git@github.com:tarikkavaz/dotfiles-light.git
git push -u origin master
```

Dotfiles Private settings

```bash
cd
cd Dotfiles/
mkdir_cd private
touch env
```

Write this to the `env` file

```bash
export PS1="${PS1_OSX_ADVANCED}"
export VIRTUAL_ENV_DISABLE_PROMPT=1
```
Copy Dotfiles Colors

```bash
cp ${HOME}/Dotfiles/startup-sequence/sample-ps1-colors ${HOME}/Dotfiles/private/my-colors
```

git aliases & settings

```bash
ln -s ~/Dropbox/Dotfiles/configs/gitconfig ~/.gitconfig
ln -s ~/Dropbox/Dotfiles/configs/gitignore_global ~/.gitignore_global
```

### Install [Ruby](https://github.com/rbenv/rbenv)

```bash
git clone https://github.com/rbenv/rbenv.git ~/.rbenv

#Restart Terminal

git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build

git clone https://github.com/rbenv/rbenv-default-gems.git $(rbenv root)/plugins/rbenv-default-gems

nano $(rbenv root)/default-gems

    # add these to the default-gems file
    bundler
    pry

RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl) --with-readline-dir=$(brew --prefix readline) --with-libyaml-dir=$(brew --prefix libyaml)" rbenv install 2.4.0

rbenv global 2.4.0 

#Restart Terminal
```


### Install [Python](https://github.com/pyenv/pyenv)
```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv 
#Restart Terminal

git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv

git clone https://github.com/pyenv/pyenv-virtualenvwrapper.git $(pyenv root)/plugins/pyenv-virtualenvwrapper

CFLAGS="-I$(brew --prefix openssl)/include" LDFLAGS="-L$(brew --prefix openssl)/lib" CONFIGURE_OPTS="--with-readline-dir=$(brew --prefix readline)" pyenv install 3.6.0

#Restart Terminal

CFLAGS="-I$(brew --prefix openssl)/include" LDFLAGS="-L$(brew --prefix openssl)/lib" CONFIGURE_OPTS="--with-readline-dir=$(brew --prefix readline) --with-openssl-dir=$(brew --prefix)" PYTHON_CONFIGURE_OPTS=--enable-unicode=ucs2 pyenv install 2.7.12

pyenv global 2.7.12

#Restart Terminal
```

### Install nvm

```bash
nvm install 6.3
nvm alias default 6.3
```

### Install Bower & Gulp

```bash
npm install -g bower
npm install -g gulp-cli
```

 

### Install Apps

Change the default Gatekeeper behavior to run apps downloaded from anywhere.

```bash
sudo spctl --master-disable
```

#### From Brew Cask

##### Install Homebrew Cask
```bash
brew tap caskroom/cask
```
This Should be in your `.bash_profile` or Private Dotfile.

```bash
export HOMEBREW_CASK_OPTS="--appdir=/Applications --fontdir=/Library/Fonts"
```

##### Install Cask Apps

```bash
brew cask install atom appcleaner bartender google-chrome codekit commander-one goofy iterm2 liya macdown moom padbury-clock phoneclean qlImageSize qlmarkdown screens-connect skype spotify sequel-pro slack textmate teamviewer tripmode the-unarchiver vlc virtualbox waltr whatsapp zenmate-vpn
```

- [Atom](https://atom.io/download/mac)
- [Appcleaner](https://freemacsoft.net/appcleaner/)
- [Bartender 2](https://www.macbartender.com/) *
- [Chrome](https://www.google.com/chrome/browser/desktop/)
- [CodeKit](https://incident57.com/codekit/) *
- [Commander One](http://mac.eltima.com/file-manager.html) *
- [Dropbox](https://www.dropbox.com/downloading?os=mac)
- [Goofy](http://www.goofyapp.com/)
- [iTerm2](https://www.iterm2.com/downloads.html)
- [Liya](https://cutedgesystems.com/software/liya/)
- [MacDown](http://macdown.uranusjr.com/)
- [Moom](https://manytricks.com/moom/) *
- [Padbury Clock](http://padbury.me/clock/)
- [PhoneClean](http://www.imobie.com/phoneclean/)
- [qlImageSize](https://github.com/Nyx0uf/qlImageSize)
- [QuickLook Markdown](https://github.com/toland/qlmarkdown/)
- [Screens Connect](http://edovia.com/screensconnect/download/)
- [Skype](http://www.skype.com/en/download-skype/skype-for-mac/downloading/)
- [Spotify](https://www.spotify.com/us/download/)
- [Sequel-Pro](https://www.sequelpro.com/)
- [Slack](https://slack.com/)
- [TextMate](https://macromates.com/download) *
- [TeamViewer](https://www.teamviewer.com/en/download/mac/)
- [TripMode](https://www.tripmode.ch) *
- [The Unarchiver](http://unarchiver.c3.cx/unarchiver)
- [VLC](http://www.videolan.org/vlc/index.html)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [WALTR](https://softorino.com/waltr) *
- [WhatsApp Messenger](https://www.whatsapp.com/download/)
- [ZenMate](https://secure.zenmate.com/)



#### From App Store
- [1Password](https://1password.com/)
- [Airmail 3](http://airmailapp.com/)
- [Feedly](http://feedly.com/)
- [Keynote](http://www.apple.com/mac/keynote/)
- [Numbers](http://www.apple.com/mac/numbers/)
- [Pages](http://www.apple.com/mac/pages/)
- [Parcel](https://web.parcelapp.net/)
- [Pixelmator](http://www.pixelmator.com/mac/)
- [Screens](http://edovia.com/screens/)
- [Sip](http://sipapp.io/)
- [Telegram](https://telegram.org/)
- [TweetDeck](https://tweetdeck.twitter.com/)
- [Unclutter](http://unclutterapp.com/)

#### From Web
- [Adobe Creative Cloud](http://www.adobe.com/creativecloud/desktop-app.html) *
- [Transmit](https://panic.com/transmit/) *
- [Virtual Machines](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/mac/)

#### Themes

- [Solarized color palette](http://ethanschoonover.com/solarized)
- [iTerm themes](http://iterm2colorschemes.com/)


`*` = needs Registration

