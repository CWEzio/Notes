# Vim cheet list
Many contents of this note comes from https://missing.csail.mit.edu/2020/editors/
## Movement
- `CR` in `vim` mappings is the carriage return usually the `enter` on the keyboard. 
- Basic movement `hjkl`
- Words: `w` (next word), `b` (beginning of word), `e` (end of word) 
- Lines: `0` (beginning of line), `^` (first non-blank character), `$` (end of line) 
- Screen: `H` (top of screen), `M` (middle of screen), `L` (bottom of screen)
- Scroll: `Ctrl-u` (up), `Ctrl-d` (down)
- File: `gg` (beginning of file), `G` (end of file)
- Line numbers: `:{number}<CR>` or `{number}G` (line{number})
- Misc: `%` (corresponding item)
- Find: `f{character}, t{character}, F{character}, T{character}
    - find/to forward/backward {character} on the current line
    - `,`/`;` for navigating match
- Search: `/{regex}`, `n/N` for navigating matches

## View
- Folding/collapsing a function
    1. Fold a function with `zc`
    2. Unfold a function with `zo`
    3. Close all folds with `zM`
    4. Open all folds with `zR`
    > In VS code, navigate `j` and `k` will make folded code unfold.
    > [This issue](https://github.com/VSCodeVim/Vim/issues/1004) hasn't been fixed.
    > Currently, one workaround is to enable `foldfix` in user setting. See [this question](https://stackoverflow.com/questions/50888475/code-folds-are-automatically-opened-when-cursor-moves-over-them-in-vs-code-vim/74936010#74936010) for reference.



## Screen Positioning 
- Press `z` then `enter` will move the current line to the top of the screen
- `50z` will move the 50th line to the top of the screen. 50 can be other numbers
- `z.` will move the current line to the middle of the screen
- `z-` will move the current line to the bottom of the screen
## Edit
- `i` enter insert mode
- `o`/`O` (small o and captital O) insert line below/above
- `d{motion}` delete {motion}
    - e.g. `dw` is delete word, `d$` is delete to end of line, `d0` is delete to beginning of line
- `c{motion}` change {motion}
    - e.g. `cw` is change word
    - like `d{motion}` followed by `i`
- `x` delete character (equal to `dl`) 
- `s` substitute character (equal to `xi`)
- `v` visual mode and select current character, `V` visual mode and select whole line 
- Visual mode + manipulation
    - select text, `d` to delete it or `c` to change it
- Undo and redo
    1. `u` to undo, `<C-r>` to redo    
    2. `U` (capital u): return the last line which was modified to its original state (reverse all changes in last modified line)
    > `U` is not actually a true "undo" command as it does not actually navigate undo history like `u` and `Ctrl-r`. This means that (somewhat confusingly) `U` is itself undo-able with `u`; it creates a new change to reverse previous changes.
- `y` to copy ("yank", some other command like `d` also copy)
- `p` to paste
- Lots more to learn: e.g. `~` flips the case of a character 
- `>` to indent (after selection), `<` to shift left
- `:8,10 s/**search**/**replace**/g` do search and replece from line 8 to line 10, where **search** is the string to be replaced and **replace** is the string to replace
- ':%s/search/replace' in the whole file 
## Counts
- `3w` moves 3 words forward
- `5j` moves 5 lines down
- `7dw` delete 7 words
## Modifiers
- `ci(` change the contents inside the current pair of parentheses
- `ci[` change the contents inside the current pair of square brackets
- `da'` delete a single-quoted string, including the surroundign single quotes. 

## Surrounding 
> Note that this is supported by `vim-surround` plugin, which is also supported by `vscode`'s vim emulator.
- `y s <motion> <desired>` to add desired surrounding around text defined by `<motion>`
- `S"` to add double quotes (") for visual selections (in visual mode, i.e., something is visually selected)
- In visual mode, `S(` to enclose the selected with `( * )` with space, `S)` to enclose the selected with `(*)` without space.
- `cs"'` to change double quotes (") to single quote (')
- `ds"` to delete double quotes (")

## Changing case 
- After visual selecting the text, 
    - `U` for upper case
    - `u` for lower case
    - `~` for inverting case
> Remember that the text should be visual selected first.
- `gU` + \<motion> will make all characters in \<motion> uppercase, similarly for `~` and `u`. For example:
    - `gUe`
- Capitalize the first letter of each work in a line
    - ```
      :%s/\<./\u&/g
      ```
    - `s` stands for substitute
    - `\<` matching the beginning of a word
    - `.` matches any character
    - `\u` converts the matched character to uppercase
    - `&` represents the matched character
    - `g` makes the command global (apply to all matches in this line)



## Increase or Decrease keyword
> This section is adopted from [`zsh-vi-mode`](https://github.com/jeffreytse/zsh-vi-mode/blob/master/README.md)'s readme.
- In normal mode, use `Ctrl + a` to increase to the next keyword and `Ctrl + x` to decrease to the next keyword <br>
    For example, use `Ctrl + a` to increase
    - `9` => `10`
    - `aa99bb` => `aa100bb`
    - `aa100bc` => `aa101bc`
    - `0xDe` => `0xdf`
    - `0Xdf` => `0Xe0`
    - `0b101` => `0b110`
    - `0B11` => `0B101`
    - `true` => `false`
    - `yes` => `no`
    - `on` => `off`
    - `T` => `F`
    - `Fri` => `Sat`
    - `Oct` => `Nov`
    - `Monday` => `Tuesday`
    - `January` => `February`
    - `+` => `-`
    - `++` => `--`
    - `==` => `!=`
    - `!==` => `===`
    - `&&` => `||`
    - `and` => `or`
    - ...

    use `Ctrl + x` to decrease
    - `100` => `99`
    - `aa100bb` => `aa99bb`
    - `0` => `-1`
    - `0xdE0` => `0xDDF`
    - `0xffFf0` => `0xfffef`
    - `0xfffF0` => `0xFFFEF`
    - `0x0` => `0xffffffffffffffff`
    - `0Xf` => `0Xe`
    - `0b100` => `0b010`
    - `0B100` => `0B011`
    - `True` => `False`
    - `On` => `Off`
    - `Sun` => `Sat`
    - `Jan` => `Dec`
    - `Monday` => `Sunday`
    - `August` => `July`
    - `/` => `*`
    - `++` => `--`
    - `==` => `!=`
    - `!==` => `===`
    - `||` => `&&`
    - `or` => `and`
    - ...

 
## window and tab control
**Window**
- `Ctrl + w` and then press `v` (vertical split) or `s` (horizontal split) to open a new window
- `Ctrl + w` and then use `hjkl` to move to different window
- `Ctrl + w` and then press `o` to close other windows, keep the active window only
- `Ctrl + w` and then press `c` to close current window but keeps the buffer
- For a `split` window: use `Ctrl + w ` and then `+` or `-` to resize the height; For a `vsplit` window: use `Ctrl + w` and then `>` or `<` to resize the width.
- `Ctrl + w`, `=`: Makes all splits equal size

**Tab**
- `:q` close a single tab not a whole window like `Ctrl + w`, `c`. 
    > Note that `Ctrl + w, c` also only close the current tab in `VSCode`.
- `:tabn` to go for next tab in a group; `:tabp` to go for previous tab in a group 
- `gt` to go for next tab
- `1gt` to go for tab 1; `2gt` to go for tab 2; `3gt` to go for tab 3; etc.
- `gT` to go for previous tab 
- `1gT` to go for 1 previous tab; `2gT` to go for 2 previous tab; etc.
- `:tabm -1` moves the current tab 1 position to the left. Adjust the number accordingly for different usage cases.

> In `vs code`
> - `Ctrl + Tab` to change between tabs in a group.
> - `Ctrl + (PgUp / PgDn)` to cycle through tabs in a group
> - `Alt + (1, 2, 3, etc.)` to switch to specific tab
> - Use the command in the command palette, like `close all other editors in group`

## Copy to external 
- `"+y` yank to the clipboard
- `"+p` paste from the clipboard
Similarly, you can use `"*y` and `"*p` to copy and paste from teh primary selection (middle-click copy).
> - `"+` corresponds to the "clipboard" which is what gets used when you Ctrl+C/Ctrl+V in most programs.
> - `"*` corresponds to the "primary" selection, which is what gets used when you select text and middle-click to paste in many Unix-like systems.

> It should be noted, copy to clipboard might not be supported in some `vim` distribution. Actually, the default `vim` installed in ubuntu does not support copy to clip board.
>
> You can find whether your `vim` have clipboard support by 
> ```
> vim --version | grep clipboard
> ```
> If you see `+clipboard` and `+xterm_clipboard`, it means Vim has clipboard support. If you see `-clipboard` and `-xterm_clipboard`, it means Vim was compiled without clipboard support.
> If your vim does not have clipboard support, you can install `vim-gtk` with:
> ```
> sudo apt install vim-gtk
> ```


## IdeaVim
- `gd` to go to the declaration, `Ctrl + o` to go back to where you came from
- `alt + arrow key` to move across tab (pycharm)

## VSCode
Some VSCode's native commands are useful to use together with vim commands:
- use ``Ctrl + ` `` to focus on the terminal
- use `Ctrl + 1` to focus on the editor  
    > when there are multiple editor windows, use, for example, `Ctrl + 2` to refer to other windows. In other words, `Ctrl + index` to focus on the editor
- Navigate between groups using `Ctrl + PageDown` and `Ctrl + PageUp` (also suitable for terminals)

More about VSCodeVim extension:
- You can find all supported vim commands of VSCodeVim in [roadmap](https://github.com/VSCodeVim/Vim/blob/master/ROADMAP.md).

### Multiple cursor mode
- `alt + left click` to add a cursor.
- `gb` to add cursor at next word occurance place. 
  - Use `I` to enter insert mode at the beginning of the word, or `A` for the end.
  - `s` to modify, `d` to delete
- `ctrl + c` or `esc` to exit the multicursor mode. 
  >Note that in jupyter notebook, you should use `ctrl + c` as `esc` exits to cell-level selection.
- `shift + ctrl + ⬆️`️ to add cursor above 
- `shift + ctrl + ↓` to add arrow above
️
## Miscellany
1. Use `:set syntax=python` to force `vim` to syntax-highlight a file as `python`

# Settings
## Caps Lock as an additional Esc
You may want to change the behavior of `Caps Lock` to `Esc` if you are using `Vim`.
1. Open Gnome-tweaks by `windows` + searching with name
2. In `Keyboard & Mouse` tab, click `Additional Layout Options`
3. In `Caps Lock behavior`, select `Make Caps Lock an additional Esc`
4. In vscode, `ctrl + shift + p`, search and open `user settings`
5. Search for `keyboard dispatch` and set it as `keyCode`.