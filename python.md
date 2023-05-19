## Let `flake8` ignore rules 
Some `flake8` hints are annoying. For example, I want to set the max line length to be longer.
Check [this answer](https://stackoverflow.com/a/50177174/12825127) for details.
In short, 
- `ctrl+shift+p`, open user setting, search for flake8
- add `--max-line-length=120` to Flake8 Args.


## `Autopep8` works too aggressivly 
In `vscode`, I use `autopep8` to format my code. However, it works too aggressively and it change my import order.
I use 
```python
import sys  
sys.path.append('/home/chenwang/catkin_ws/src/turtlebot_tmpc/script')  
```
to add python path. However, `autopep8` move this to the end of the import which breaks everything.

The solution is to add `# nopep8` at the end of the line to told `autopep8` not to format this line.

For example,
```python
import sys  # nopep8
sys.path.append('/home/chenwang/catkin_ws/src/turtlebot_tmpc/script')  # nopep8
```


# Conda

## Add conda-forge channel
Some libraries are not contained in the default conda channels. You may choose to add conda-forge channel.
1. ```
    vim ~/.condarc
    ```
2. Add/change the following 
    ```
    channels:
        - defaults
        - conda-forge
    ```

> The official documentation recommand use
> ```
> conda config --add channels conda-forge
> conda config --set channel_priority strict
> ```
> However, I find that this will cause the solving environment failed with intial frozen solve. The cause might be the strict channel_priority and put conda-forge channel above defaults channel. You can open `.condarc` to check your setting.
## use proxy
1.  ```
    vim ~/.condarc
    ```
2. Add the following to the file:
    ```
    proxy_servers:
        http: http://127.0.0.1:7890
        https: http://127.0.0.1:7890
    ```

# VSCode
## Set PythonPath
Check [this answer for details](https://stackoverflow.com/questions/53653083/how-to-correctly-set-pythonpath-for-visual-studio-code)
1. Open the `settings.json` file and insert:
    ```
    "terminal.integrated.env.windows": {
    "PYTHONPATH": "${workspaceFolder}/src;${workspaceFolder}/tests"
    },
    "python.envFile": "${workspaceFolder}/.env",
    ```
    > If you only cares about whether the editor can correctly find the package path, only setting `python.envFile` is enough.
2. create `.env` file and put the pythonpath in this file. For example:
    ```
    PYTHONPATH=/home/chenwang/VCD/softgym:${PYTHONPATH}
    PYFLEXROOT="/home/chenwang/VCD/softgym/PyFlex"
    PYTHONPATH="${PYFLEXROOT}/bindings/build:${PYTHONPATH}"
    LD_LIBRARY_PATH=${PYFLEXROOT}/external/SDL2-2.0.4/lib/x64:$LD_LIBRARY_PATH
    ```
    > You can also put other path to this file.

## Debug. Add command line argument
```json
{
    "name": "Python: main_generate_data",
    "type": "python",
    "request": "launch",
    "program": "VCD/main.py",
    "console": "integratedTerminal",
    "justMyCode": true,
    "args": ["--gen_data", "1", "--dataf", "./data/vcd"]
}
```
`args` specifies arguments to pass to the Python program. Each element of the argument string that's separated by a space should be contained within quotes.