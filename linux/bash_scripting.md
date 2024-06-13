## How to properly export a path
An example is
```zsh
# setup drake
export PATH="/opt/drake/bin${PATH:+:${PATH}}"
export PYTHONPATH="/opt/drake/lib/python$(python3 -c 'import sys; print("{0}.{1}".format(*sys.version_info))')/site-packages${PYTHONPATH:+:${PYTHONPATH}}"
```
Note that in the end `${PYTHONPATH:+:${PYTHONPATH}}` is used. Here `:+` is a conditional parameter expansion operator. The syntax is as follows:
```zsh
${parameter:+word}
```
- If `parameter` is set and not null (i.e., non-empty), the result of the expansion is the value of `word`.
- If `parameter` is unset or null, the result of the expansion is an empty string.
This comes handy because `PATH` or `PYTHONPATH` might not have been set.