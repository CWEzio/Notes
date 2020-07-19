## Add conda virtual enviornment to jupyter notebook

* create a new env
  ```
  conda create -n mr_env python=3.7    
  ```
* install ipykernel to this env
  ```
  conda install -c anaconda ipykernel
  ```
* add this env to jupyter notebook
  ```
  python -m ipykernel install --user --name=firstEnv
  ```
* deactivate the env
  ```
  conda deactivate
  ```
Or simply follow this tutorial
https://janakiev.com/blog/jupyter-virtual-envs/
It contains more information.

## Write python module

Refer to np8's answer
https://stackoverflow.com/questions/714063/importing-modules-from-parent-folder
One thing to note is that setup.py need to be above the main folder.

## View Eigen Value when using clion debug(gdb)

Use drake_gdb https://github.com/SeanCurtis-TRI/drake_gdb

* git clone https://github.com/SeanCurtis-TRI/drake_gdb

* Create the file ~/.gdbinit

* insert the following lines into .gdbinit
  ```
  python
  import sys
  sys.path.append("path of your drake_gdb")
  import drake_gdb 
  drake_gdb.register_printers()
  end
  ```

One thing to note is that I encounter bug when use the author's install instruction. It seems that gdb use /usr/bin/python which is python2.7. 
```
sys.path.insert(0, "$DRAKE_GDB_ROOT$")
```
given by the authors' instruction seems does not work. import drake_gdb will give
```python
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: No module named drake_gdb
```
Change this line to 
```
sys.path.append("path of your drake_gdb")
```
will solve this problem. Also use $DRAKE_GDB_ROOT is not necessary.