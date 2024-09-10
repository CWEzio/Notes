TODO: Write a script to automatic complete most of the setup steps.

# Guide to configure a freshly installed Ubuntu 

## Update & Upgrade
1. `sudo apt update`
2. `sudo apt upgrade`

## install graphics driver
Recently, I bought a new computer with RTX 3070, I need to install the driver manually.
 1. First add the ubuntu Graphics Drivers PPA. More information can be found on https://launchpad.net/~graphics-drivers/+archive/ubuntu/ppa/+index?batch=75&direction=backwards&memo=75

 2. Open Software&Updates, open *other software* tab, check the two graphics drivers ppa. Then update the ppa information.

 3. `sudo apt install nvidia-driver-455`

 > Now, since the official source has been updated, you can just install the driver with the *3rd* step.

## Google Chrome
  >* install Chrome using package;
  >* sign in google account, chrome will automatically restore all the things 

## install clash_for_windows
1. Download clash_for_windows from the [release page](https://github.com/Fndroid/clash_for_windows_pkg/releases)
2. Go to [monocloud](https://www.monocloud.me/knowledgebase/16) and follow the knowledge base.

## terminal proxy
 * add 
    ```
    set_proxy(){
        export https_proxy=http://127.0.0.1:8889
        export http_proxy=http://127.0.0.1:8889
        export telnet_proxy=http://127.0.0.1:8889
        export ftp_proxy=http://127.0.0.1:8889
    }
   unset_proxy(){
        unset https_proxy
        unset http_proxy
        unset telnet_proxy
        unset ftp_proxy
    }

    ``` 
    to file `~/.zshrc`. (Set the port the same as v2ray's port)

## `apt` proxy 
Add the file `proxy.conf` containing
```
Acquire {
  HTTP::proxy "http://127.0.0.1:2340";
  HTTPS::proxy "http://127.0.0.1:2340";
}
```
in `/etc/apt/apt.conf.d` directory.

## `ssh` proxy
Add 
```
ProxyCommand /usr/bin/nc -X 5 -x 127.0.0.1:7777 %h %p
```
as the first line to `~/.ssh/config`. If `config` does not exist, create a new one.
> It should be noted that `ssh` is used by `git`. Without setting `ssh` proxy may cause `git push` fail since GFW has banned `github`.

## Install Applications

### install vscode
```bash
sudo apt install snap
```
Open Ubuntu Software, search for vscode and install.

### install vim
```bash
sudo apt install vim-gtk
```
> You can also install vim, but the default vim does not support copy to clipboard. Therefore, I install `vim-gtk`.

### install curl
```bash
sudo apt install curl
```

## Install and configure `git` 

### Install `git`
```bash
sudo apt install git 
```

### Setup github SSH key
```bash
ssh-keygen -t rsa -C "chenwang0234@gmail.com"
```
Open github, account setting. Choose SSH and GPG keys.
```bash
vim ~/.ssh/id_rsa.pub
```
Copy the content of `id_rsa.pub` to add SSH key.

### Setup git user email and name
```bash
git config --global user.email "chenwang0234@gmail.com"
git config --global user.name "chenwang"
```

### Change git default editor
```
git config --global core.editor "vim"
```

## Modify `CapsLock` behavior 
1. Install `gnome-tweak-tool`:
  ```
  sudo apt-get install gnome-tweak-tool
  ```
2. Start `gnome-tweak-tool`:
  ```
  gnome-tweaks
  ```
3. Change `CapsLock`'s in `Keyboard & Mouse` > `Additional Layout Options` > `Caps Lock Behavior`.

> In order for the change to work in `VSCode`, modify vscode setting:
> ```json
> "keyboard.dispatch": "keyCode"
> ```

## Install fzf


## Change shell(fish)
I now prefer to use the `fish` terminal.
1. Install fish
  ```
  sudo apt-add-repository ppa:fish-shell/release-3
  sudo apt update
  sudo apt install fish
  ```

2. Set fish as the default terminal.
    ```
    sudo chsh -s /usr/bin/fish $USER
    ```

3. Install `fzf`
    ```
    git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
    ~/.fzf/install
    ```

4. Download my fish config files and create a softlink to it.
   ```
   cd ~
   git clone git@github.com:CWEzio/dotfiles.git  
   ln -s ~/dotfiles/fish ~/.config/fish
   ```

Optionally,
- Bring `zsh` history to `fish` with [`zsh-history-to-fish`](https://github.com/rsalmei/zsh-history-to-fish).

### Install plugins
Note that the following steps can be omitted because my config files already contain the plugins.
1. Use [`fisher`](https://github.com/jorgebucaran/fisher) to manage fish's plugins.
  ```
  curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source && fisher install jorgebucaran/fisher
  ```

2. Install [`pure`](https://github.com/pure-fish/pure) theme
  ```
  fisher install pure-fish/pure
  ```

> VSCode terminal has an [issue](https://github.com/pure-fish/pure/issues/341) with `pure-fish`, as the prompt will insert a newline. To solve this, add the following line to the vscode setting:</br>
> ```json
> "terminal.integrated.shellIntegration.enabled": false,
> ```


3. Install other fish plugins
  ```
  fisher install patrickf1/fzf.fish
  fisher install decors/fish-colored-man
  ```
- `fzf-fish` requires `fzf` installed. 
- `fzf-fish`'s previewing search directory requires `eza` and `bat`. Check the following sections on how to install them.

## eza
[`eza`](https://github.com/eza-community/eza) is a modern alternative to `ls` that looks nice.
Follow [this instruction](https://github.com/eza-community/eza/blob/main/INSTALL.md) to install eza.

## bat
[`bat`](https://github.com/sharkdp/bat) is a modern alternative to `cat` that looks nice. Install it by:
```
sudo apt install bat
```

## Tmux
1. Install tmux with:
    ```
    sudo apt install tmux
    ```
> Ubuntu 20.04 does not provide the latest version `Tmux`. Build it from source follow [the official guide](https://github.com/tmux/tmux/wiki/Installing).

2. Install xclip with (to enable clip to system's clipboard with tmux):
    ```
    sudo apt install xclip 
    ```
3. Download [config](https://github.com/CWEzio/profile-and-config/blob/main/tmux/.tmux.conf) and place it in the home directory.

4. Use my config. 
  ```
  ln -s <path-to-dotfile-repo>/tmux/.tmux ~/.tmux
  ln -s <path-to-dotfile-repo>/tmux/.tmux.conf ~/.tmux.conf
  ```

## Install additional Fonts
### `MesloLGS Nerd`
From [Nerd Font](https://www.nerdfonts.com/).
- Download the font files
```
curl -OL https://github.com/ryanoasis/nerd-fonts/releases/latest/download/Meslo.tar.xz
```
- Click the following files to install:
  - `MesloLGSNerdFont-Regular.tff`
  - `MesloLGSNerdFont-Italic.tff`
  - `MesloLGSNerdFont-Bold.tff`
  - `MesloLGSNerdFont-BoldItalic.tff`

### Install Times New Roman Font
1. Install the Microsoft TrueType core fonts 
> ```
> sudo apt install ttf-mscorefonts-installer
> ```
2. Scan the font directories to build fresh font information cache files
>```
>sudo fc-cache -vr
>```
For more details, refer to this [website](https://linuxconfig.org/install-microsoft-fonts-on-ubuntu-20-04-focal-fossa-desktop)


## Install python virtual environment
```bash
sudo apt install python3-venv
```


## Solving ubuntu windows time conflict
Check time date setting
```
timedatectl
```
Set RTC in local TZ to yes
```
timedatectl set-local-rtc 1 --adjust-system-clock
```
You may see warning, this is fine.


## Install trash
```
sudo apt install trash-cli
```
`Trash` is a useful commandline tool which is used to move files to trash, empty trash folder and things like that. `Trash` is much safer than `rm`.

## Install TLDR
```zsh
sudo apt install tldr
```

## Install VSCode


## Install Mendeley    
> *Note on Ubuntu22.04* <br>
> Since Ubuntu 22.04 does not support `python2`, `mendeley` has denpendency issue in installing. However, we can turn to `flatpak` to install mendely. `flatpak` provides an encapsulation of an app and its dependency and allows for depolyment on different version of Linux system.<br>
> 1. Follow [this guidance](https://flatpak.org/setup/Ubuntu) to setup flatpak.
> 2. Install `mendeley` by
>```
>flatpak install flathub com.elsevier.MendeleyDesktop
>```
> 3. After installation, run `mendeley` with
>```
>flatpak run com.elsevier.MendeleyDesktop
>```



## Make workspace span displays
> A workspace in Ubuntu is a virtual desktop, allowing users to organize and separate open applications. Think of it as having multiple computer desktops on a single monitor. Users can swiftly switch between these workspaces to access different sets of open windows, reducing clutter and improving task organization.

By default, the workspace does not span multiple displays and navigating between workspaces only switch the workspace in the main display.

Check [this article](https://www.thegygers.com/2020/07/30/ubuntu-20-04-workspaces-and-multiple-monitors/) for details.
In short, first install `gnome-tweaks`
```
sudo add-apt-repository universe
sudo apt install gnome-tweaks
```
Then in the left tabs, select `Workspaces` and then on the right click `Workspaces span displays`.

> Note that this is the default behavior in Ubuntu 22.04. 


# Better theme (Optional)
## `Catppuccin`
[`Catppuccin`](https://catppuccin.com/) is a pastel theme that looks nice. It can be configured for various applications.
### [`fish`](https://github.com/catppuccin/fish)
Install with
  ```
  fisher install catppuccin/fish
  fish_config theme save "Catppuccin Mocha"
  ```

### [`gnome-terminal`](https://github.com/catppuccin/gnome-terminal)
- Make sure that `gsettings` and `python3` are available.
- Install with
  ```
  curl -L https://raw.githubusercontent.com/catppuccin/gnome-terminal/v0.3.0/install.py | python3 -
  ```
- In `gnome-terminal`, open `Edit->Preferences`, and enable the profile for the theme you want.

### `Tmux` 
> - `Catppuccin` does not work on `tmux-3.0`. As I tested, it works in `tmux-3.4`
> - `Catppuccin` requires `nerd font` in order to work correctly.
- With my `tmux` dotfile in place, press `Prefix + I` (captial I) to install it with `TPM`.
- Reload tmux with `tmux source-file ~/.tmux.conf`.

### `VSCode`
Install[ `Catppuccin for VSCode`](https://marketplace.visualstudio.com/items?itemName=Catppuccin.catppuccin-vsc) and [`Catppuccin Icons for VSCode`](https://marketplace.visualstudio.com/items?itemName=Catppuccin.catppuccin-vsc-icons) in the extension market.

Add the following lines to the user `settings.json`:
```json
// For Catppuccin Theme
// we try to make semantic highlighting look good
"editor.semanticHighlighting.enabled": true,
// prevent VSCode from modifying the terminal colors
"terminal.integrated.minimumContrastRatio": 1,
// make the window's titlebar use the workbench colors
"window.titleBarStyle": "custom",
// applicable if you use Go, this is an opt-in flag!
"gopls": {
    "ui.semanticTokens": true,
},
```

### `Bat`
Catppuccin also has a theme for `bat`, [`catppuccin/bat`](https://github.com/catppuccin/bat).

Follow the steps in `readme` to install it.


# Obsolete

## Installing Vim-key bindings for jupyter notebook (web browser)
> Currently, I mainly use `VSCode` to edit jupyter notebook. Therefore, this section is no longer needed.
* ```bash
  pip install jupyter_contrib_nbextensions

  ```
* ```bash
  jupyter nbextensions_configurator enable --user
  ```
* ```bash
  # You may need the following to create the directoy
  mkdir -p $(jupyter --data-dir)/nbextensions
  # Now clone the repository
  cd $(jupyter --data-dir)/nbextensions
  git clone https://github.com/lambdalisue/jupyter-vim-binding vim_binding
  chmod -R go-w vim_binding
  ```
* Launch a Jupyter notebook session. Then, in a browser, go to <root>/nbextensions/; for example, if the notebook is hosted under localhost:8888, go to http://localhost:8888/nbextensions/. Activate VIM binding from the list of extensions. Check documentation for more details.

## Install [fd-find](https://github.com/sharkdp/fd)
`fd` is a simple, fast and user-friendly alternative to `find`.
Install by
```
sudo apt install fd-find
```

## Fcitx disable extra trigger key (shift)
```
code ~/.config/fcitx/config
```
change SwitchKey=SHIFT Both to SwitchKey=Disable
After saving
```
chmod 400 config
```
To make this setting persist after restart.

## Change to zsh shell 
Use `zsh` as the terminal and `oh-my-zsh` to manage the extensions.
1. install `zsh` with
   ```zsh
   sudo apt install zsh
   ```

2. install `oh-my-zsh`
    ```bash
    sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
    ```

3. Install `zsh-syntax-highlighting`
    ```bash
    cd /usr/share/
    sudo git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
    echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
    source ~/.zshrc
    ```

4. Install `zsh-autosuggestions`
    ```bash
    git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
    echo "source $ZSH_CUSTOM/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh" >> ~/.zshrc
    source ~/.zshrc
    ```

5. Install [zsh-vi-mode](https://github.com/jeffreytse/zsh-vi-mode)
    * Clone the project
      ```zsh
      git clone https://github.com/jeffreytse/zsh-vi-mode \
      $ZSH_CUSTOM/plugins/zsh-vi-mode
      ```
    * Add the following to `.zshrc`
      ```zsh
      plugins+=(zsh-vi-mode)
      ```
      > Keep in mind that plugins need to be added before `oh-my-zsh.sh` is sourced.
6. Increase the history size
  Add the following
  ```zsh
  # Better history
  export SAVEHIST=1000000000
  export HISTSIZE=1000000000
  ```
  to `.zshrc`

7. reboot

## system monitor

  >* `sudo add-apt-repository ppa:fossfreedom/indicator-sysmonitor`
  >* `sudo apt update`
  >* `sudo apt install indicator-sysmonitor`

启动，右击设置"Run on startup"



## 好看的主题(optional)

  >* 先安装unity-tweak-tool

`sudo apt install unity-tweak-tool`

  >* 安装Flatabulous主题

```bash
sudo add-apt-repository ppa:noobslab/themes
sudo apt update
sudo apt install flatabulous-theme
```

  >* 安装扁平化的图标

```bash
sudo add-apt-repository ppa:noobslab/icons
sudo apt update
sudo apt install ultra-flat-icons
```

  >* 安装完成后，打开unity-tweak-tool软件.修改theme为Flatabulous,修改icon为Ultra-flat

## 安装docky(optional)

  >* `sudo apt install docky`
  >* 卸载：`sudo apt remove docky`
  >* [参考博客](https://www.jianshu.com/p/4bd2d9b1af41)
  >* 小技巧：如果想要删除docky上的图标(除了docky本身)，直接用鼠标将其拖出即可；此外，如果要将一个图标添加到docky,只需要打开这个软件，再在docky上右击，选择"pin to dock"
  >* 也可以使用cairo-dock，会玩的话, 足以媲美macOS自带的dock.缺点是不能正常显示一些软件的图标，如VSCode,需要手动设置
