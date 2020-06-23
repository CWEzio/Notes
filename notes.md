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