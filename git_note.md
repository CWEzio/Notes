# My note of git usage

## Remove file/folder from the git index but keep those files, in other words, want to untrack file or folder
This can be done with 
```
git rm -r --cached <folder>
```
>`-r` is for recursive removal\
>`--cached` is for removing the folder from the index, but keep the files themself untouched.

Refer to [documentation](https://git-scm.com/docs/git-rm) for more information regarding `rm`.