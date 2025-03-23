# Steps to setup a new mac
## ITerm2

## Homebrew

## Git

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


## Clash for Windows
- Download clash for windows from [this page](https://github.com/lantongxue/clash_for_windows_pkg/releases).
- `sudo xattr -r -d com.apple.quarantine path-to-clash-for-windows-file` 
    - To remove the quarantine label auto added by Apple.
- Install the app.

## Remap `caps lock` to `esc`
- Apple > System Settings > Keyboard > Keyboard Shortcuts > Modifier

# Configure appearance
## Catppuccin
### `ITerm2`
- Use `mocha` profile in [this comment](https://github.com/catppuccin/iterm/issues/27#issuecomment-2513558106). 
- Disable `Use different colors for light and dark mode`.