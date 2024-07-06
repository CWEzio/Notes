# The sharp bits
## Leaking variables in loop
In fact, Python formally acknowledges that the names defined as for loop targets (a more formally rigorous name for "index variables") leak into the enclosing function scope. Check [this article](https://eli.thegreenplace.net/2015/the-scope-of-index-variables-in-pythons-for-loops/) for more information. 

## Parsing boolean value with `argparse`
Suppose that I have an argument called `--my_bollean_flag` defined with 
```python
parser.add_argument("--my_boolean_flag", type=bool)
```
Then, I ran this file with
```
python main.py --my_boolean_flag=False
```
However, the parsed `arg.my_boolean_flag` would be True.
This is because
```python
arg.my_boolean_flag = bool('False')
```
and non-empty string would be converted to `True`.




# Conda

## Add conda-forge channel
Some libraries are not contained in the default conda channels. You may choose to add `conda-forge` channel.
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

## Add conda virtual environment to Jupyter notebook

* create a new env
  ```
  conda create -n mr_env python=3.7    
  ```
* install Ipykernel to this env
  ```
  conda install -c anaconda ipykernel
  ```
* add this env to Jupyter notebook
  ```
  python -m ipykernel install --user --name=firstEnv
  ```
* deactivate the env
  ```
  conda deactivate
  ```
Refer to [this tutorial](https://janakiev.com/blog/jupyter-virtual-envs/) for more information.
> Note that when using `vscode` for Jupyter notebook, above steps are not needed. Just select the correct python interpreter and then select the Jupyter environment in the opened Jupyter notebook.

# Python virtual environment

## Use Python Virtual Environment
- Install virtualenv with 
  ```
  pip3 install virtualenv
  ```
- Create a virtualenv with
  ```
  python3 -m venv myenv
  ```
  This command will create a virtual environment called `myenv` in current directory.
- Activate the virtualenv with
  ```
  source path_to_myenv/myenv/bin/activate
  ```
- Deactivate the virtualenv with
  ```
  deactivate
  ```
- To delete the virtual environment, just remove the folder containing the virtual environment.

## Add python virtualenv to Jupyter notebook 
- Make sure the virtualenv is activated
- Install ipykernel with
  ```
  pip install ipykernel
  ```
- Add virtualenv to Jupyter notebook as a kernel with
  ```
  python -m ipykernel install --name=myenv --user
  ```
  where `myenv` is the name of the added kernel.

> Similar as the `conda` case, above steps are not needed when use `VSCode`.

# Jupyter

## Installing Vim-key bindings for Jupyter notebook
* ```bash
  pip install jupyter_contrib_nbextensions

  ```
* ```bash
  jupyter nbextensions_configurator enable --user
  ```
* ```bash
  # You may need the following to create the directoy
  mkdir -p $(jupyter --data-dir)/nbextensions
  # Now clone the repository
  cd $(jupyter --data-dir)/nbextensions
  git clone https://github.com/lambdalisue/jupyter-vim-binding vim_binding
  chmod -R go-w vim_binding
  ```
* Launch a Jupyter notebook session. Then, in a browser, go to <root>/nbextensions/; for example, if the notebook is hosted under localhost:8888, go to http://localhost:8888/nbextensions/. Activate VIM binding from the list of extensions. Check documentation for more details.

## Remove Virtual Environment from Jupyter notebook
- After a virtual environment is deleted, you'll want to remove it from Jupyter.
- First list all available kernels.
  ```
  jupyter kernelspec list
  ```
- Uninstall the kernel with
  ```
  jupyter kernelspec uninstall myenv
  ```

# Package and path management

## Write Python module
Refer to np8's answer
https://stackoverflow.com/questions/714063/importing-modules-from-parent-folder
> One thing to note is that setup.py need to be above the main folder.



# VSCode

## `Autopep8` works too aggressively 
In `vscode`, I use `autopep8` to format my code. However, it works too aggressively and it changes my import order.
I use 
```python
import sys  
sys.path.append('/home/chenwang/catkin_ws/src/turtlebot_tmpc/script')  
```
to add Python path. However, `autopep8` moves this to the end of the import which breaks everything.

The solution is to add `# nopep8` at the end of the line to tell `autopep8` not to format this line.

For example,
```python
import sys  # nopep8
sys.path.append('/home/chenwang/catkin_ws/src/turtlebot_tmpc/script')  # nopep8
```

## Let `flake8` ignore the rules 
Some `flake8` hints are annoying. For example, I want to set the max line length to be longer.
Check [this answer](https://stackoverflow.com/a/50177174/12825127) for details.
In short, 
- `ctrl+shift+p`, open user setting, search for flake8
- add `--max-line-length=120` to Flake8 Args.

# Python virtual environment
## Setup cuda
Add the following to your `activate` file:
```
CUDAVER=cuda-11.2
export PATH=/usr/local/$CUDAVER/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/$CUDAVER/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/usr/local/$CUDAVER/lib64:$LD_LIBRARY_PATH
export CUDA_PATH=/usr/local/$CUDAVER
export CUDA_ROOT=/usr/local/$CUDAVER
export CUDA_HOME=/usr/local/$CUDAVER
export CUDA_HOST_COMPILER=/usr/bin/gcc-10
```
Where `CUDAVER` specifies the cuda version, modify it according to which cuda you use.

# PIP
- Use `pip --force-reinstall --no-cache-dir <package-name>` to install a package and not using previously cached downloads. This is useful when you try different versions of a package to figure out which version can work.

# Problems 
## General python problem
### Import problem 1: `Module not found. *Package Name* is not a package`
Recently, I have been using the `softagent` library to test the `CEM` method. However, when I ran the `run_cem.py`, I encountered the problem stated in the title. The directory structure is:
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
 It turns out that this is the problem of the path. `python` automatically adds the run file's location to the `python`'s path. Therefore, when call
```python
from cem.cem import ...
```
, there will be errors because Python regards `cem.py` as the lib instead of the `cem` directory. Changing `cem.py` to another name resolves this problem.

### Import problem 2: module *module name* has no attribute `attribute name`
When use pydrake, encounter the `AttributeError`:
```python
AttributeError: module 'drake' has no attribute 'experimental_lcmt_deformable_tri'

The above exception was the direct cause of the following exception:
...
```
Solution:

Refer to this [issue](https://github.com/RobotLocomotion/drake/issues/13187), I run 
```python
import drake
print(drake.__file__)
```
to see which drake is used. It prints
```
/home/chenwang/drake/__init__.py
```
Then I run
```python
import sys
print(sys.path)
```
and I got 
```
['', '/opt/openrobots/lib/python2.7/site-packages', '/home/chenwang/underactuated', '/home/chenwang/manipulation', '/home/chenwang/drake-build/install/lib/python3.6/site-packages', '/opt/ros/melodic/lib/python2.7/dist-packages', '/usr/lib/python36.zip', '/usr/lib/python3.6', '/usr/lib/python3.6/lib-dynload', '/home/chenwang/manipulation_env/lib/python3.6/site-packages', '/home/chenwang/manipulation_env/lib/python3.6/site-packages/IPython/extensions']
```
Then I realize I am in home directory, with a directory called `drake`. Then when I import `drake`, this directory is imported. (The reason maybe that the current directory has higher priority in the search sequence.) After `cd` to other directory, I can import successfully.

## Pybind11
### Undefined Symbol when using pybind11
When I use pybind11 to write a Python wrapper of a C++ robot controller, I encounter an undefined symbol issue. At first, I directly compile a module with all cpp files (robot controller source files and binder source files), then importing this module from Python will cause the undefined symbol issue. In another try, I first compile the robot controller into a library, then link it with the pybind11 generated module. This solves the undefined symbol problem. To be honest, I do not know the actual reason, but there are some clues from this helpful website https://stackoverflow.com/questions/12573816/what-is-an-undefined-reference-unresolved-external-symbol-error-and-how-do-i-fix.

Also when looking at the undefined symbol problem, some tools are helpful, such as c++filt, which can be used to demangle the symbol that a linker sees. 

```
nm RobotCtrl_py.cpython-37m-x86_64-linux-gnu.so --demangle -Du

```
This command can be used to see the symbol information in a binary file.
Check this https://medium.com/fcamels-notes/%E8%A7%A3%E6%B1%BA-linux-%E4%B8%8A-c-c-%E7%9A%84-undefined-symbol-%E6%88%96-undefined-reference-a80ee8f85425 website for further information.

## Mujoco_py
### Error: GLEW initialization error: Missing GL version

```
sudo apt-get update -y
sudo apt-get install -y libglew-dev
export LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libGLEW.so
```
See this issue https://github.com/openai/mujoco-py/issues/268

### Box2D env cannot be used 

```
env=gym.make('LunarLander-v2')
AttributeError: module 'gym.envs.box2d' has no attribute 'LunarLander'
```
- Solution
  ```
  pip install Box2D
  ```
- cannot install Box2D with conda

## Jupyter notebook
### No such comm target registered: jupyter.widget.version
`jupyter notebook` return error:
```zsh
[IPKernelApp] ERROR | No such comm target registered: jupyter.widget.version
```

Solution:

(In virtual enviornment)
```zsh
jupyter nbextension install --py widgetsnbextension --user
jupyter nbextension enable --py widgetsnbextension
```
> The above two commands should be run in the right `virtual environment`, i.e., where the kernel returning the error information is in.


## Other
### `pydot` does not find `dot` in path
When using `pydot`, encounter
```python
FileNotFoundError: [Errno 2] "dot" not found in path.
```
Solution:
```zsh
sudo apt install graphviz
```
> It should be noted that, for `pip` installed `pydot`, it is of no use to install `graphviz` via `pip`. I do not know the reason though. I guess this has something to do with the `pydot` search path. Install `graphviz` via `apt` will solve the problem.
