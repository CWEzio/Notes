# Softgym

## Installation
I am using Ubuntu 20.04, which is not support by default. Therefore, docker is needed.

I follow [this guidance](https://danieltakeshi.github.io/2021/02/20/softgym/) to install softgym.

### Preparation
1. Install conda. Both miniconda or anaconda is fine. 
    ```
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    bash Miniconda3-latest-Linux-x86_64.sh
    ```
    Follow the installation steps. Then, disable auto activate the *base* environment:
    ```
    conda config --set auto_activate_base false
    ```
2. [Install docker and nvidia container toolkit](https://github.com/CWEzio/Notes/blob/master/docker.md)
<<<<<<< HEAD
3. Install dependencies
=======

3. Install prerequistes
>>>>>>> f480b2cd9a08a7645e688f811cdc881f232c0d3e
    ```
    sudo apt-get install build-essential libgl1-mesa-dev freeglut3-dev libglfw3 libgles2-mesa-dev
    ```

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

    > If anaconda is used, run 
    > ```
    >docker run -v /home/chenwang/softgym:/workspace/softgym -v /home/chenwang/anaconda3:/home/chenwang/anaconda3 -v /tmp/.X11-unix:/tmp/.X11-unix --rm --runtime=nvidia --gpus all -e DISPLAY=$DISPLAY  -e QT_X11_NO_MITSHM=1 -it xingyu/softgym:latest bash
    > ```
7. Compile PyFlex (in docker container)
    ```
    cd softgym/
    export PATH="/home/chenwang/miniconda3/bin:$PATH"
    . ./prepare_1.0.sh
    . ./compile_1.0.sh
    ```
    > The first dot in last two commands is shorthand of `source`. In other words, you can also use `source ./prepare_1.0.sh`. If you directly run `prepare_1.0.sh`, this script will be run in a new shell and no change will be made in current shell.

    > If anaconda is used, the exported path should be 
    >```
    >export PATH="/home/chenwang/anaconda3/bin:$PATH"
    >```
8. To use softgym, some path environmental variables need to be set. To ease the usage, I add them in the conda activate scripts. For details, check [this page](https://docs.conda.io/projects/conda/en/latest/dev-guide/deep-dives/activation.html). In short, when activate, scripts in `CONDA_PREFIX/etc/conda/activate.d/` will be run. `CONDA_PREFIX` is the path to the conda environment.
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

## VCD Installation
I also want to get [`VCD`](https://github.com/Xingyu-Lin/VCD) run, which also uses `softgym`. However, `VCD` also uses `pytorch`, making things trickier.
- `pyflex` needs to be compiled with `CUDA 9.2`, which is contained in the provided docker image. However, the python environment might use different `CUDA` version, which might casue `undefined symbol: cudaSetupArgument` problem.
- What's more, too old `pytorch` is also not compatible.
- What's more, `python 3.6` is too old (compatibility issue with pytorch). Need to use `python 3.7`. 
### Installation step
Most installation steps are the same as the previous softgym installation. Here I just give some key steps.
1. clone `VCD`. cd to `VCD`. `cp -r [path to softgym] ./`. Then `cd softgym && git checkout vcd`.
2. Create a custom environment `yml` and use this `yml` to create the `VCD` conda environment.
    ```yml
    name: VCD
    channels:
    - defaults
    dependencies:
    - python=3.7
    - numpy
    - imageio
    - glob2
    - cmake
    - pybind11
    - click
    - matplotlib
    - joblib
    - Pillow
    - plotly
    - pip:
        - gtimer
        - gym==0.14.0
        - moviepy
        - opencv-python==4.1.1.26
        - Shapely==1.6.4.post2
        - pyquaternion==0.9.5 
        - sk-video==1.1.10
    ```
3. Compile pyflex. Need to write a custom `prepare.sh`.
    ```sh
    . activate VCD 
    export PYFLEXROOT=${PWD}/PyFlex
    export PYTHONPATH=${PYFLEXROOT}/bindings/build:$PYTHONPATH
    export LD_LIBRARY_PATH=${PYFLEXROOT}/external/SDL2-2.0.4/lib/x64:$LD_LIBRARY_PATH
    ```
    > Use the softgym under VCD. Remember to change the paths in previous softgym installation section accordingly.

4. Install pytorch with 
    ```
    conda install pytorch==1.12.1 torchvision==0.13.1 torchaudio==0.12.1 cudatoolkit=10.2 -c pytorch
    ```
    > cuda 10.2 is not compatible with nvidia 3090
5. Replace the `python-pcl` dependency with `open3d`. Modify `VCD/utils/utils.py`.

6. Install other requirements.
    ```
    conda install h5py
    conda install pytorch-scatter -c pyg
    conda install pyg -c pyg 
    pip install open3d
    ```

## Notes
1. It seems that `pyflex` has to be compiled with the correct python interpreter using `pybind11`. That is, if I use one python environment to compile the `pyflex` and I want to use another python environemnt to use the compiled `pyflex` lib, I will encounter `no module named pyflex` problem. This seems to be a feature of `pybind11`. Currently, I do not know the reason. On the contrary, `pydrake` seems do not care which python interpreter compile it.

## Bugs & Solutions
### Choose Nvidia graphic card as the default device
In my laptop, I encounter a strange problem. I can successfully compile the pyflex. However, when I try to run the example program, I encounter segmentation errors. The output suggests that some symbols are undefined. This problem is tricky because if you paste the error output and search the internet, you will not get enough useful information. 

The cause of this issue is because that my laptop has two graphic cards, one intel's and another nvidia's. I need to choose the nvidia's graphic card as always use, or runnning softgym will call the intel card and cause errors.

Follow the following steps to choose the nvidia card as the default card:
1. Open the NVIDIA X Server Settings application. You can find it by searching for "NVIDIA" in the app menu.

2. Under the "PRIME Profiles" section, you will see an option to select between the NVIDIA GPU and the integrated Intel GPU. Choose the one you want to use by default.

3. Log out and log back in for the changes to take effect.
