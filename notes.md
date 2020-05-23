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