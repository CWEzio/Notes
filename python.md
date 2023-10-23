
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

## Let `flake8` ignore rules 
Some `flake8` hints are annoying. For example, I want to set the max line length to be longer.
Check [this answer](https://stackoverflow.com/a/50177174/12825127) for details.
In short, 
- `ctrl+shift+p`, open user setting, search for flake8
- add `--max-line-length=120` to Flake8 Args.

# Bugs
## `Module not found. *Package Name* is not a package`
Recently I am using the `softagent` library to test the `CEM` method. However, when I ran the `run_cem.py`, I encounter the problem stated in the title. The directory structure is:
```
- cem
    - cem.py
        .
        .
        .
    - run_cem.py
```
when I run
```
python cem/run_cem.py
```
 It turns out that this is the problem of the path. `python` automatically add the run file's location to the `python`'s path. Therefore, when call
```python
from cem.cem import ...
```
, there will be errors because python regards `cem.py` as the lib instead of the `cem` directory. Changing `cem.py` to other name resolves this problem.
