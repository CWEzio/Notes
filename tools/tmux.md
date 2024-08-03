# Usages
Here is two quick reference for tmux: [the missing semester](https://missing.csail.mit.edu/2020/command-line/#Terminal%20Multiplexers) and [this blog](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/).

Commands in tmux are triggered by a `prefix key` followed by a `command key`.
- prefix `C-k` (my customization, `C-k` is the prefix key in vscode)
  > original `C-b`

## session control
- `tmux` to start a new session
- `tmux new -s <name>` to start a session with that name
- `tmux ls` lists the current sessions
- within tmux `C-k d`  to detach the current session
- `tmux a` to attach to the last session. `tmux a -t <name> ` to attach to a specific session
- rename the existing session `tmux rename-session -t <current-name> <new-name>`
- If you are inside the session that you want to rename:
  1. `C-k :` to bring up the command prompt
  2. `rename-session new_session_name`

## pane control
- split windows horizontally `C-k s` (my customization, vim style)
  > original `C-k "`
- split windows vertically `C-k v` (my customization, vim style)
  > original `C-k %`
- select pane left/right/up/down with `M+h`/`M+l`/`M+k`/`M+j` (my customization, vim style)
  > original `C-k <arrow-key>`
- resize pane with `C-k H`/`C-k L`/`C-k J`/`C-k K`. You can repeat resize without re-entering the prefix. (my customization, vim style)
  > original `C-k C-<arrow-key>`
- `exit` or `C-d` to close current pane
- `C-b z` make a pane to full screen, hit it again to shrink it back to its previous size

## window control
- use `C-k c` to create a new window
- use `C-k p` to switch to previous window
- use `C-k n` to switch to next window
- use `C-k <number>` to go to *i*-th window
- use `C-k ,` to rename the current window
### Swap window
Run
```
tmux swap-window -s 3 -t 5
```
in one terminal of the tmux session.  This command will swap the `3` and `5` window.

Alternatively, you can use
```
tmux swap-window -t +1
```
This swap the current window with the window.

Alternatively, you can use `C-k :` to enter the command.


## copy and paste with keyboard
> Note that I have configured the tmux to use vim keybinding 
1. Enter Copy mode with `C-k [`
2. Navigate with `h`, `j`, `k`, `l`
3. press `v` to start selection, use `r` for rectangle selection and `V` to select a whole line
4. yank with `y`, copy's the selection and exit the copy mode
5. paste with `C-k ]`
    > Caution when using zsh-vi-mode. You need to enter insert mode to do the paste or the pasted content will be interpreted as command.
> Can also exit copy mode with `q`.

## copy and paste with mouse (via tmux)
1. Click and drag with the left mouse button to select text.
2. Once the text is selected, release the left mouse button.
3. You can then paste this text within any tmux pane using `prefix + ]`. 

## tmux copy to system's clipboard and enable vi copy mode
1. To enable clip to clipboard. You need to install `xclip`
    ```
    sudo apt install xclip
    ```
2. You need to add the following to the config file
    ```shell
    setw -g mode-keys vi

    # For copy and paste in vim style
    # Bind v to start selection
    bind-key -T copy-mode-vi 'v' send-keys -X begin-selection

    # Bind y to yank (copy) the selection and enable copy to clipboard
    bind-key -T copy-mode-vi 'y' send-keys -X copy-pipe-and-cancel 'xclip -in -selection clipboard'

    # also enable mouse to copy to system's clipboard
    bind-key -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel 'xclip -selection clipboard -i'

    # Bind r for rectangle selection
    bind-key -T copy-mode-vi r send-keys -X rectangle-toggle
    ```
> `copy-pipe-and-cancle`:
> - **copy**: copy the currently selected text to the `tmux` buffer
> - **pipe**: pipes the copied text to an external command specified by the user
> - **cancle**: exit the copy mode

## copy and paste with mouse (via terminal)
1. copy with mouse: `shift + left-click`
2. paste with mouse: `shift + middle-click`
> It should be noted that copy and paste with mouse is the behavior by terminal, not tmux. Therefore, the selected text may span multiple panes.

> **Why using `shift`**?
>
>Some terminal emulators require the use of the `Shift` key for mouse operations when mouse support is enabled within applications like `tmux` or `vim`. This is because when mouse support is enabled, these applications (`tmux`) take over mouse events to provide features like pane resizing, window selection, etc. As a result, the terminal's default mouse behaviors, like text selection and middle-click pasting, can be masked.
>
>Using the `Shift` key in conjunction with mouse operations allows you to bypass the application's mouse event handling and use the terminal emulator's default behaviors. So, when you hold down `Shift` and use the mouse, you're essentially telling the terminal: "Ignore what the application wants to do with this mouse event, and just handle it like you normally would."
>
>So yes, if you're in a `tmux` session with mouse support turned on and you want to use your terminal's default copy/paste behavior, holding `Shift` while selecting text and pasting with the middle button is the correct approach in many terminal emulators.


## Config
You can customizing `tmux` by editing the `~/.tmux.conf` file to make tmux more user friendly. More information can be find in [this blog](https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/). For example customizations, you can find them both in the above blog and [tmux-sensible](https://github.com/tmux-plugins/tmux-sensible).

You can also find my [config file](https://github.com/CWEzio/profile-and-config/tree/main/tmux)

After making changes to your `.tmux.conf`, you should reload the configuration by executing the command:
```
tmux source-file ~/.tmux.conf
```
Or you can do it within `tmux` by `C-k :` to bring up the command prompt, and then entering `source-file ~/.tmux.conf`.

- Useful materials on configuring `tmux`
  - [Ham Vocke's blog article](https://hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/)

### setting basic options
You can set basic options like
```
set -g default-terminal "tmux-256color"
set -g mouse on
set -s escape-time 0
```

### Rebind prefix key
```
set -g prefix C-k
unbind C-b
bind C-k send-prefix
```

### customize keybinding
1. ```
    bind -n M-h select-pane -L
    ```
    - `-n` stands to *no prefix*
    - `M` stands for the `Meta` key, which is the `Alt` key
    - This setting binds `Alt+h` with select the left pane. `bind` is the same as `bind-key`
2. ```
    bind -r H resize-pane -L 2
    ```
    - This setting binds `<Prefix> + H` to resize the pane leftward.
    - `-r` stands for *repeatable*. When a key is bound with -r, after pressing the prefix and the key once, you can keep pressing the key without the prefix, and the bound command will repeat. 

## Bugs and issues

### L-shaped portion of the window filled with dots
When you connect to a `tmux` session from terminals of different sizes, `tmux` will use the size of the smallest terminal. This ensures content is visible in all connected terminals. If there's a larger terminal, `tmux` won't fill it entirely, leading to dots on the side, which indicate unused space. 

Disconnecting the smaller terminal to solve this problem.

You can attach to the session while forcing all other sessions to detach by
```
tmux attach-session -d -t my-session
```
The `-d` flag tells tmux to detach any other clients that are currently attached to the target session.