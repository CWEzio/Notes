# Problems 
## `LazyVim` quit slowly in a directory containing `.git`
I don't know the detailed reason, but after toggling the plugins for a while, I find this problem is caused by the `persistence.nvim` plugin. Disabling it resolves the problem.

## Edit files that require root privilege
1. Set `EDITOR` environmental variable to `/opt/nvim-linux64/bin/nvim`, which is the path to `nvim` binary.
2. Edit the file with `sudoedit <path-to-file>`