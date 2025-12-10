# Steps to setup a new mac
## ITerm2

## Homebrew

## Git
## Install and configure `git` 

### Install `git`
```bash
brew install git
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
```bash
git config --global core.editor "nvim"
```

## Install `fish`
1. `brew install fish`
2. Set `fish` as the default shell
    - `sudo chsh -s /opt/homebrew/bin/fish $USER`
3. Use my fish config ([Download my dotfiles repo](#download-my-dotfiles-repo) section is the prerequisite):
   ```
   mv ~/.config/fish ~/.config/fish_backup
   ln -s ~/dotfiles/fish ~/.config/fish
   ```
4. Install `fzf`
    ```
    git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
    ~/.fzf/install
    ```
    `y` for all options

## VSCode
- Install VSCode
- Disable the hold key behavior by 
    ```
    defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false
    ```

## Neovim
- Install with `brew install neovim`
- Use my config
  ```
  ln -s <path-to-my-dotfiles>/nvim/ ~/.config/nvim
  ```

## Clash for Windows
- Download clash for windows from [this page](https://github.com/lantongxue/clash_for_windows_pkg/releases).
- `sudo xattr -r -d com.apple.quarantine path-to-clash-for-windows-file` 
    - To remove the quarantine label auto added by Apple.
- Install the app.

## Keyboard setting 
- `System Settings` > `Keyboard` > `Press 🌐 key to` > `Show Emoji & Symbols`
### Remember the last used input source for an APP
- `System Settings` > `Keyboard` > `Edit` in `Input sources` > `Automatically switch to a document's input source`
> Now I prefer to turn it off.
### Remap `caps lock` to `esc`
- Apple > System Settings > Keyboard > Keyboard Shortcuts > Modifier

# Configure appearance
## Catppuccin
### `ITerm2`
- Use `mocha` profile in [this comment](https://github.com/catppuccin/iterm/issues/27#issuecomment-2513558106). 
- Disable `Use different colors for light and dark mode`.

## Font
### `Monaspace`
- Install it with `brew install --cask font-monaspace`
*VSCode*
- Add the following in `settings.json` 
    ```json
    "editor.fontFamily": "'Monaspace Neon Light', monospace",// add Ani (one distinguishable font) to check whether your selected font is at work
    "editor.fontLigatures": "'calt', 'liga', 'ss01', 'ss02', 'ss03', 'ss04', 'ss05', 'ss06', 'ss07', 'ss08', 'ss09'", // to enable ligature and text healing
    ```

*iTerm2*
- `cmd + ,` to open setting
- Go to profile, select text, select `Monaspace Neon` and `Light` under `Font`
- Enable `use ligatures`, click the three dot icon in the right most and enable at least one feature (this will enable text healing).
