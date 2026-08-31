# Ubuntu development environment setup

This is the short setup checklist for a fresh Ubuntu installation. It consolidates the development-tool sections from [Ubuntu_env_setup.md](Ubuntu_env_setup.md) and uses the quicker dotfile-based setup from [mac_setup.md](../mac/mac_setup.md).

## Install the base packages

```bash
sudo apt update
sudo apt upgrade
sudo apt install -y curl git fish tmux xclip bat fd-find gpg wget
```

Ubuntu names the `bat` and `fd` executables `batcat` and `fdfind`. Add the names expected by the fish configuration:

```bash
mkdir -p ~/.local/bin
[ -e ~/.local/bin/bat ] || ln -s /usr/bin/batcat ~/.local/bin/bat
[ -e ~/.local/bin/fd ] || ln -s /usr/bin/fdfind ~/.local/bin/fd
```

After starting fish, make sure the directory is on `PATH`:

```fish
fish_add_path ~/.local/bin
```

## Configure Git and GitHub SSH

```bash
git config --global user.name "chenwang"
git config --global user.email "chenwang0234@gmail.com"
git config --global core.editor "nvim"
git config --global init.defaultBranch main
```

Create an SSH key:

```bash
ssh-keygen -t ed25519 -C "chenwang0234@gmail.com"
cat ~/.ssh/id_ed25519.pub
```

Add the printed public key in GitHub under **Settings > SSH and GPG keys**, then test it:

```bash
ssh -T git@github.com
```

## Clone the dotfiles

The fish, tmux, and Neovim sections below assume the repository is at `~/dotfiles`.

```bash
git clone --recurse-submodules git@github.com:CWEzio/dotfiles.git ~/dotfiles
```

Back up any existing configuration before creating the symbolic links below.

## Fish

Set fish as the login shell:

```bash
chsh -s "$(command -v fish)"
```

Log out and back in after changing the login shell. Link the fish configuration:

```bash
mkdir -p ~/.config
ln -s ~/dotfiles/fish ~/.config/fish
```

Install the current `fzf` release from its repository:

```bash
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

Answer `y` to the installer prompts. The dotfiles already declare the fish plugins. If Fisher itself is missing, run the following inside fish:

```fish
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source
fisher install jorgebucaran/fisher
fisher update
```

The `fzf.fish` previews also depend on `eza`, `bat`, and `fd`.

## eza

`eza` is a modern replacement for `ls`. First try Ubuntu's package:

```bash
sudo apt install -y eza
```

If the package is unavailable on the installed Ubuntu release, use the [official Debian/Ubuntu repository](https://github.com/eza-community/eza/blob/main/INSTALL.md#debian-and-ubuntu):

```bash
sudo mkdir -p /etc/apt/keyrings
wget -qO- https://raw.githubusercontent.com/eza-community/eza/main/deb.asc | sudo gpg --dearmor -o /etc/apt/keyrings/gierens.gpg
echo "deb [signed-by=/etc/apt/keyrings/gierens.gpg] http://deb.gierens.de stable main" | sudo tee /etc/apt/sources.list.d/gierens.list
sudo chmod 644 /etc/apt/keyrings/gierens.gpg /etc/apt/sources.list.d/gierens.list
sudo apt update
sudo apt install -y eza
```

Verify the three fish preview dependencies:

```bash
eza --version
bat --version
fd --version
```

## tmux

`tmux` and `xclip` were installed with the base packages. `xclip` lets tmux copy text to the Linux desktop clipboard.

Link the tmux configuration:

```bash
ln -s ~/dotfiles/tmux/.tmux ~/.tmux
ln -s ~/dotfiles/tmux/.tmux.conf ~/.tmux.conf
```

Start tmux and install the plugins with `prefix + I` (capital `I`). Then reload the configuration:

```bash
tmux source-file ~/.tmux.conf
```

If the Ubuntu package is too old for a configured plugin or theme, follow the [official tmux installation guide](https://github.com/tmux/tmux/wiki/Installing) to install a newer release.

## Neovim

Ubuntu's `apt` version can lag behind Neovim releases. Install the latest stable x86-64 archive using the [official Linux instructions](https://github.com/neovim/neovim/blob/master/INSTALL.md#linux):

```bash
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.tar.gz
sudo tar -C /opt -xzf nvim-linux-x86_64.tar.gz
```

For an ARM64 machine, replace `x86_64` with `arm64` in both commands and paths. Add Neovim to fish's `PATH`:

```fish
fish_add_path /opt/nvim-linux-x86_64/bin
```

Link the Neovim configuration:

```bash
mkdir -p ~/.config
ln -s ~/dotfiles/nvim ~/.config/nvim
```

Start Neovim, wait for its plugins to finish installing, and check the result:

```bash
nvim
```

Inside Neovim, run:

```vim
:checkhealth
```

Prefer `sudoedit` when editing root-owned files instead of maintaining a second Neovim configuration under `/root`.

## Final check

Open a new terminal, then verify the tools:

```bash
fish --version
tmux -V
git --version
eza --version
bat --version
fd --version
nvim --version
```
