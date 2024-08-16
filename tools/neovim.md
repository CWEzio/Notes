# Problems 
## `LazyVim` quit slowly in a directory containing `.git`
I don't know the detailed reason, but after toggling the plugins for a while, I find this problem is caused by the `persistence.nvim` plugin. Disabling it resolves the problem.