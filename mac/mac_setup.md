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

## VSCode
- Install VSCode
- Disable the hold key behavior by 
    ```
    defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false
    ```

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
