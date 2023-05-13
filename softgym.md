# Softgym

## Installation
I am using Ubuntu 20.04, which is not support by default. Therefore, docker is needed.

I follow [this guidance](https://danieltakeshi.github.io/2021/02/20/softgym/) to install softgym.

### Preparation
1. Install conda (I use miniconda)
    ```
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    bash Miniconda3-latest-Linux-x86_64.sh
    ```
    Follow the installation steps. Then, disable auto activate the *base* environment:
    ```
    conda config --set auto_activate_base false
    ```
2. [Install docker](https://docs.docker.com/engine/install/ubuntu/)
3. [Install nvidia container toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker)

### Installation steps
> It should be noted that *docker* is only needed for compiling *Pyflex*. After compiling *Pyflex* library in the docker container, *softgym* can be used directly from the normal terminal.
1. Clone the repo
    ```
    git clone git@github.com:Xingyu-Lin/softgym.git
    ```
2. `cd` to the *softgym* directory
3. create the `softgym` conda environment:
    ```
    conda env create -f environment.yml
    ```
    > Note that environment.yml already contains the name of the environment to be created.
4. install *pybind11*
    ```
    conda activate softgym
    conda install pybind11
    ```
5. pull the docker image
    ```
    docker pull xingyu/softgym
    ```
6. enter the container
    ```
    docker run -v /home/chenwang/softgym:/workspace/softgym -v /home/chenwang/miniconda3:/home/chenwang/miniconda3 -v /tmp/.X11-unix:/tmp/.X11-unix --rm --runtime=nvidia --gpus all -e DISPLAY=$DISPLAY  -e QT_X11_NO_MITSHM=1 -it xingyu/softgym:latest bash
    ```
    > One trap in this step is that you need to make sure that the mounted path of *miniconda3* is the same in the docker container as in the local machine. (Here `/home/chenwang/miniconda3:/home/chenwang/miniconda3`).
7. Compile PyFlex (in docker container)
    ```
    cd softgym/
    export PATH="/home/chenwang/miniconda3/bin:$PATH"
    . ./prepare_1.0.sh
    . ./compile_1.0.sh
    ```
    > The first dot in last two commands is shorthand of `source`. In other words, you can also use `source ./prepare_1.0.sh`. If you directly run `prepare_1.0.sh`, this script will be run in a new shell and no change will be made in current shell.
8. To use softgym, some path environmental variables need to be set. To ease the usage, I add them in the conda activate scripts. For details, check [this page](https://docs.conda.io/projects/conda/en/latest/dev-guide/deep-dives/activation.html). In short, when activate, scripts in `CONDA_PREFIX/etc/conda/activate.d/` will be run. `CONDA_PREFIX` is the path to the conda environment.
    - create a script file called `set_path.sh`
        ```
        mkdir -p ~/miniconda3/envs/softgym/etc/conda/activate.d/
        vim ~/miniconda3/envs/softgym/etc/conda/activate.d/set_path.sh
        ```
    - Then copy the following to the created script file
        ```
        export PYTHONPATH="/home/chenwang/softgym:${PYTHONPATH}"
        export PYFLEXROOT="/home/chenwang/softgym/PyFlex"
        export PYTHONPATH="${PYFLEXROOT}/bindings/build:${PYTHONPATH}"
        export LD_LIBRARY_PATH=${PYFLEXROOT}/external/SDL2-2.0.4/lib/x64:$LD_LIBRARY_PATH
        ```

## Usage
### Run examples
0. `cd` to softgym directory
1. Activate the environment
    ```
    conda activate softgym
    ```
2.  
    ```
    python examples/random_env.py --env_name ClothFlatten
    ```

