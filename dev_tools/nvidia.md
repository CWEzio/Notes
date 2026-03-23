# cuda
## Installation 
[Official doc](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#download-the-nvidia-cuda-toolkit) on cuda installation.

### Network repo setup
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
```
- Modify `ubuntu2004/x86_64` accordingly.

### Install `libcudnn`
```
sudo apt install libcudnn8
```
- Need to complete the [Network repo setup](#network-repo-setup).

### Install `cuda-toolkit` with runfile
- Go to the [website](https://developer.nvidia.com/cuda-12-1-0-download-archive)
- Select platform, and select `runfile (local)`.
- Follow the guidance in the website to run the runfile.
- In the installation interface, select off the driver. Then select installation.

### Setup the path
- Bash
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
- Fish
    ```fish
    # setup cuda-toolkit path
    set CUDAVER cuda-12.1
    set -gx PATH $PATH /usr/local/$CUDAVER/bin
    set -gx LD_LIBRARY_PATH $LD_LIBRARY_PATH /usr/local/$CUDAVER/lib
    set -gx LD_LIBRARY_PATH $LD_LIBRARY_PATH /usr/local/$CUDAVER/lib64
    set -gx CUDA_PATH /usr/local/$CUDAVER
    set -gx CUDA_ROOT /usr/local/$CUDAVER
    set -gx CUDA_HOME /usr/local/$CUDAVER
    set -gx CUDA_HOST_COMPILER /usr/bin/gcc-10
    ```
> Change the CUDAVER accordingly.


TODO: complete it


## `nvcc` and `nvidia-smi` can have different versions
Refer to [this answer](https://stackoverflow.com/questions/53422407/different-cuda-versions-shown-by-nvcc-and-nvidia-smi) for more detail. In general, it is fine to have the driver's CUDA version (`nvidia-smi`) larger than the CUDA runtime version (`nvcc`).