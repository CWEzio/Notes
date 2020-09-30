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
- ':8,10 s/**search**/**replace**/g' do search and replece from line 8 to line 10, where **search** is the string to be replaced and **replace** is the string to replace
- ':%s/search/replace' in the whole file 
## Counts
- `3w` moves 3 words forward
- `5j` moves 5 lines down
- `7dw` delete 7 words
## Modifiers
- `ci(` change the contents inside the current pair of parentheses
- `ci[` change the contents inside the current pair of square brackets
- `da'` delete a single-quoted string, including the surroundign single quotes. 
## window control
- `Ctrl + w` and then press `v` (vertical split) or `s` (horizontal split) to open a new window
- `Ctrl + w` and then use `hjkl` to move to different window
- `Ctrl + w` and then press `o` to close other windows, keep the active window only
- `Ctrl + w` and then press `c` to close current window but keeps the buffer
- `:q` close a single tab not a whole window like `Ctrl + w`, `c`. (ideaVim)
- `Ctrl + w`, `=`: Makes all splits equal size
- `Alt + s`: split and move right (pycharm my own definition)
## IdeaVim
- `gd` to go to the declaration, `Ctrl + o` to go back to where you came from
- `alt + arrow key` to move across tab (pycharm)
