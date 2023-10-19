## Markdown math
`vscode` use `KaTeX` to render math. Here is a list of [supported functions](https://katex.org/docs/supported.html)

## Select notebook kernel
To select a kernel,  click the `select kernel` button or type `select kernel` in the search palette. 

If you want to select an existing python enviornment, but cannot see it in the list:
1. `ctrl + shift + p`, type `select python interpreter`
2. choose that python environment as the interpreter
3. then you should be able to see that environment in the selection list.

After you select the environment, if the kernel is not already installed in that environment, then vscode will automatically install it. 

## keybinding
### Window control
- use ``Ctrl + ` `` to focus on the terminal
- use `Ctrl + 1` to focus on the editor  
    > when there are multiple editor windows, use, for example, `Ctrl + 2` to refer to other windows. In other words, `Ctrl + index` to focus on the editor
- Navigate between groups using `Ctrl + PageDown` and `Ctrl + PageUp` (also suitable for terminals)
- `Ctrl + Tab` to change between tabs in a group.
- `Ctrl + (PgUp / PgDn)` to cycle through tabs in a group
- use `alt + number` to navigate between different tabs
- Use the command in the command palette, like `close all other editors in group`