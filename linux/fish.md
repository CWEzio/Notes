# Introduction
[`fish`](https://fishshell.com/) is a new shell. Recently I have moved from `zsh` to `fish` because that I find it is more user-friendly and it has better plugin management. (While in `zsh`, plugin like `zsh-vi-mode` and `fd-find` conflict with each other.)

# Basic
[Official Tutorial](https://fishshell.com/docs/current/tutorial.html)

## commands
- `funced <function-name>`: edit function

# Keybinding
- `alt + ←/→` when command line is empty: move backward and forward in the directory history

# fzf-find
Key bindings:
- `ctrl + alt + f` to search for file.
- `ctrl + alt + l` to search for git log.
- `ctrl + alt + s` to search for git status.
- `ctrl + alt + h` to search for history. (My custom keybinding.)
- `ctrl + alt + P` to search for process.
- `ctrl + v` to search for variables.

List default keybinding with:
```fish
fzf_configure_bindings --help
```

# Custom user keybindings
To save custom key bindings, put the bind statements into `config.fish`. Alternatively, fish also automatically executes a function called `fish_user_key_bindings` if it exists.

Personally, I put all my fish keybindings in `config.fish` to keep it simple.

# Sharp bits
- `fish` does not use backtick \`\` for command substitution, unlike `zsh` and `bash`. Instead, it use () with or without $ for command substitution.