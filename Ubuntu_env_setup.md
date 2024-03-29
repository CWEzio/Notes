> Original version of this tutorial is from Huangjian. Gradually add modification as I use Ubuntu.

# Guide to reinstall Ubuntu20.04
> This guide should also apply to `Ubuntu18.04` and `Ubuntu 22.04`<br>

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

## install vscode
```bash
sudo apt install snap
```
Open Ubuntu Software, search for vscode and install.

## install vim
```bash
sudo apt install vim-gtk
```
> You can also install vim, but the default vim does not support copy to clipboard. Therefore, I install `vim-gtk`.

## install curl
```bash
sudo apt install curl
```

## Install git 
```bash
sudo apt install git 
```

## Setup github SSH key
```bash
ssh-keygen -t rsa -C "chenwang0234@gmail.com"
```
Open github, account setting. Choose SSH and GPG keys.
```bash
vim ~/.ssh/id_rsa.pub
```
Copy the content of `id_rsa.pub` to add SSH key.

## Setup git user email and name
```bash
git config --global user.email "chenwang0234@gmail.com"
git config --global user.name "chenwang"
```

## Change git default editor
```
git config --global core.editor "vim"
```

## Change Terminal
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


>* 修改透明度为10%
>* 命令行自动提示：没多大用，用Tab即可


## Tmux
1. Install tmux with:
    ```
    sudo apt install tmux
    ```
2. Install xclip with (to enable clip to system's clipboard with tmux):
    ```
    sudo apt install xclip 
    ```
3. Download [config](https://github.com/CWEzio/profile-and-config/blob/main/tmux/.tmux.conf) and place it in the home directory.

## Install python virtual environment
```bash
sudo apt install python3-venv
```

## Installing Vim-key bindings for jupyter notebook
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

## Install trash
```
sudo apt install trash-cli
```
`Trash` is a useful commandline tool which is used to move files to trash, empty trash folder and things like that. `Trash` is much safer than `rm`.

## Install TLDR
```zsh
sudo apt install tldr
```

## Install [fd-find](https://github.com/sharkdp/fd)
`fd` is a simple, fast and user-friendly alternative to `find`.
Install by
```
sudo apt install fd-find
```

## Install Times New Roman Font
1. Install the Microsoft TrueType core fonts 
> ```
> sudo apt install ttf-mscorefonts-installer
> ```
2. Scan the font directories to build fresh font information cache files
>```
>sudo fc-cache -vr
>```
For more details, refer to this [website](https://linuxconfig.org/install-microsoft-fonts-on-ubuntu-20-04-focal-fossa-desktop)

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
## Mouse and Touchpad

* [x] nautural scrolling

## install pycharm 

## Mendeley    
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

## 清理不必要的软件

```bash
sudo apt update
sudo apt upgrade
sudo apt remove firefox libreoffice-common unity-webapps-common
sudo apt remove thunderbird totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot
sudo apt remove gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku  landscape-client-ui-install
sudo apt remove onboard deja-dup
```

## VScode

* [ ] minimap
* [x] auto save  # 0.1s
  
>* 好的插件:C/C++, C++ Intellisense, CMake, CMake Tools, cmake-format, Code Runner, Markdown All in One, markdownlint, Python, ROS, XML, Remote SSH

## OpenCV

推荐安装opencv3.4.8. 若在ubuntu14.04,则安装opencv2.4.14

>* 安装opencv前的准备

  参考[此教程](https://github.com/HuangJianxjtu/opencv_learning.git)



## 其他

  >* ubuntu下好用的视频播放器：smplayer
  >* PDF工具：mendeley, foxit reader(unrecommended)
  >* VPN工具：easyConnect


# After Installation
## Purge Nvidia Driver
Find [this answer](https://askubuntu.com/a/206289/1004561) quiet useful. In short,
```
sudo apt-get remove --purge '^nvidia-.*'
sudo apt-get install ubuntu-desktop
sudo rm /etc/X11/xorg.conf
echo 'nouveau' | sudo tee -a /etc/modules
```
Not all above commands are needed, but it does not hurt to run them all.


# WSL2 Ubuntu

## Proxy

Add the following to `.zshrc` file
```zsh
export hostip=$(cat /etc/resolv.conf | grep "nameserver" | cut -f 2 -d " ")

set_proxy(){
    export https_proxy=http://$hostip:7078
    export http_proxy=http://$hostip:7078
    export telnet_proxy=http://$hostip:7078
    export ftp_proxy=http://$hostip:7078
}
unset_proxy(){
    unset https_proxy
    unset http_proxy
    unset telnet_proxy
    unset ftp_proxy
}
```
> Remember to turn on `allow LAN` in the clash/monocloud client.

# Obsolete

## install Qv2ray
  >* get latest release from https://github.com/Qv2ray/Qv2ray/releases/tag/v2.6.3
  >* follow the guide https://qv2ray.net/getting-started/ to configure Qv2ray

## install expressvpn
  >* just follow the official guide

> Just do not use Chinese input in Ubuntu

## 搜狗中文输入法

  >* download and install;
  >* `im-config -n fcitx` change from ibus to fcitx;
  >* `sudo reboot`
  >* (4)点击右上角输入法小图标,选择config，去掉勾，点击左下角小加号，找到Sogou Pinyin添加即可
  >* 若在菜单栏的右上角出现两个输入法图标，则：`sudo apt remove fcitx-ui-qimpanel`;再重启
  >* 搜狗拼音的简体、繁体相互转换：Ctrl+Shift+F

## Chinese input for Ubuntu18.04

  >* 在setting里面点"install/Remove Languages", 点击安装Chinese
  >* 重启
  >* 添加"Chinese(Intelligent Pinyin). win+空格切换输入法


## system monitor

  >* `sudo add-apt-repository ppa:fossfreedom/indicator-sysmonitor`
  >* `sudo apt update`
  >* `sudo apt install indicator-sysmonitor`

启动，右击设置"Run on startup"

## Shadowsocks(optional)

  >* `sudo add-apt-repository ppa:hzwhuang/ss-qt5`
  >* `sudo apt update`
  >* `sudo apt install shadowsocks-qt5`
  >* 在Startup Application里面设置shadowsocks开机自动启动


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

## 安装WPS

  >* 直接从官网下载安装包安装
  >* 解决WPS缺少字体问题：

  将WPS缺少的字体复制到 /usr/share/fonts/wps-office 目录下（字体文件已备份，共6个文件）

生成字体的索引信息,再更新字体缓存：

  ```bash
  sudo mkfontscale
  sudo mkfontdir
  sudo fc-cache -f -v
  ```

  >* 安装Microsoft开源字体

  ```bash
  sudo apt update
  sudo apt install ttf-mscorefonts-installer
  ```

  >* 将Windows的字体搬到ubuntu
  
  在 /usr/share/fonts/ 中新建文件夹 fonts_from_windows . 再把567个字体文件copy到此文件夹下。重复上一步的字体更新过程。重启电脑。这样就能再ubuntu下完成80%左右的办公任务了！！！

  BUG:WPS中不能正常显示出新增的windows字体名

## ROS-kinetic

  >* 先设置国内的镜像源:

```bash
sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
wget https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -O - | sudo apt-key add -
```

  >* 然后继续按官网的安装教程继续安装。
  >* 再测试turtlesim和Gazebo是否能正常工作。
  >* 解决gazebo打不开world的解决方法：

  ```bash
  cd ~/.gazebo/
  mkdir -p models
  cd ~/.gazebo/models/
  wget http://file.ncnynl.com/ros/gazebo_models.txt
  wget -i gazebo_models.txt #下载这些模型大概需要1小时！为了节省时间，已经备份模型了
  ls model.tar.g* | xargs -n1 tar xzvf
  ```