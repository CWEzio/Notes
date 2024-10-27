# Personal Experience
- `conda` is convenient for managing python interpreters. I can easily have multiple interpreters with different version. It also install `mkl` version `numpy` and other libs by default. However, its *intelligent* package management usually does a lot of dumb things and I really don't like. Therefore, I like to use `conda` only for managing the interpreters and a limited set of libs like `numpy` and use `pip` to manage all the rest libs.


# How to do
## Set path (environment variable)
For details, check [this page](https://docs.conda.io/projects/conda/en/latest/dev-guide/deep-dives/activation.html). In short, when activate, scripts in `CONDA_PREFIX/etc/conda/activate.d/` will be run. `CONDA_PREFIX` is the path to the conda environment.

- create a script file called `set_path.sh`
    ```
    mkdir -p ~/miniconda3/envs/softgym/etc/conda/activate.d/
    vim ~/miniconda3/envs/softgym/etc/conda/activate.d/set_path.sh
    ```
    > If anaconda is used, run 
    > ```
    >  mkdir -p ~/anaconda3/envs/softgym/etc/conda/activate.d/
    >  vim ~/anaconda3/envs/softgym/etc/conda/activate.d/set_path.sh
    > ```

    > It should be noted that in `fish`, `*.sh` won't run. `*.fish` should be created instead. 
- Then copy the following to the created script file
    ```
    export PYTHONPATH="/home/chenwang/softgym:${PYTHONPATH}"
    export PYFLEXROOT="/home/chenwang/softgym/PyFlex"
    export PYTHONPATH="${PYFLEXROOT}/bindings/build:${PYTHONPATH}"
    export LD_LIBRARY_PATH=${PYFLEXROOT}/external/SDL2-2.0.4/lib/x64:$LD_LIBRARY_PATH
    ```

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