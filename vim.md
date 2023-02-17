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
- `u` to undo, `<C-r>` to redo    
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
- `S"` to add double quotes (") for visual selections (in visual mode, i.e., something is visual selected)
- `cs"'` to change double quotes (") to single quote (')
- `ds"` to delete double quotes (")

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

 


## window control
- `Ctrl + w` and then press `v` (vertical split) or `s` (horizontal split) to open a new window
- `Ctrl + w` and then use `hjkl` to move to different window
- `Ctrl + w` and then press `o` to close other windows, keep the active window only
- `Ctrl + w` and then press `c` to close current window but keeps the buffer
- For a `split` window: use `Ctrl + w ` and then `+` or `-` to resize the height; For a `vsplit` window: use `Ctrl + w` and then `>` or `<` to resize the width.
- `:q` close a single tab not a whole window like `Ctrl + w`, `c`. (ideaVim)
- `Ctrl + w`, `=`: Makes all splits equal size
- `:tabn` to go for next tab in a group; `:tabp` to go for previous tab in a group 
- `gt` to go for next tab
- `1gt` to go for tab 1; `2gt` to go for tab 2; `3gt` to go for tab 3; etc.
- `gT` to go for previous tab 
- `1gT` to go for 1 previous tab; `2gT` to go for 2 previous tab; etc.

> In `vs code`
> - `Ctrl + Tab` to change between tabs in a group.
> - `Ctrl + (PgUp / PgDn)` to cycle through tabs in a group
> - `Alt + (1, 2, 3, etc.)` to switch to specific tab
> - Use the command in the command palette, like `close all other editors in group`
## IdeaVim
- `gd` to go to the declaration, `Ctrl + o` to go back to where you came from
- `alt + arrow key` to move across tab (pycharm)

## vscode
- use ``Ctrl + ` `` to focus on the terminal
- use `Ctrl + 1` to focus on the editor  
    > when there are multiple editor windows, use, for example, `Ctrl + 2` to refer to other windows. In other words, `Ctrl + index` to focus on the editor
- Navigate between groups using `Ctrl + PageDown` and `Ctrl + PageUp`
