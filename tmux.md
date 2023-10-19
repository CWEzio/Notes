# Tmux

## Basic Usage
Here is two quick reference for tmux: [the missing semester](https://missing.csail.mit.edu/2020/command-line/#Terminal%20Multiplexers) and [this blog](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/).

Commands in tmux are triggered by a `prefix key` followed by a `command key`.
- prefix `C-k` (my customization, `C-k` is the prefix key in vscode)
  > original `C-b`

### session control
- `tmux` to start a new session
- `tmux new -s <name>` to start a session with that name
- `tmux ls` lists the current sessions
- within tmux `C-k d`  to detach the current session
- `tmux a` to attach to the last session. `tmux a -t <name> ` to attach to a specific session
- rename the existing session `tmux rename-session -t <current-name> <new-name>`

### pane control
- split windows horizontally `C-k s` (my customization, vim style)
  > original `C-k "`
- split windows vertically `C-k v` (my customization, vim style)
  > original `C-k %`
- select pane left/right/up/down with `M+h`/`M+l`/`M+k`/`M+j` (my customization, vim style)
  > original `C-k <arrow-key>`
- resize pane with `C-k H`/`C-k L`/`C-k J`/`C-k K`. You can repeat resize without re-entering the prefix. (my customization, vim style)
  > original `C-k C-<arrow-key>`
- `exit` or `C-d` to close current pane
- `C-b z` make a pane to full screen, 

### window control
- use `C-k c` to create a new window
- use `C-k p` to switch to previous window
- use `C-k n` to switch to next window
- use `C-k <number>` to go to *i*-th window


### copy and paste with mouse (via tmux)
1. Click and drag with the left mouse button to select text.
2. Once the text is selected, release the left mouse button and immediately press the right mouse button.
3. You can then paste this text within any tmux pane using `prefix + ]`. 

### copy and paste with mouse (via terminal)
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
