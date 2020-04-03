# Build MIT Cheetah
## Install Dependencies
* Qt 5.10
* LCM
* Eigen
* mesa-common-dev
* freeglut3-dev
* coinor-libipopt-dev
* libblas-dev liblapack-dev

## Install LCM
* download `LCM-1.4.0` from https://github.com/lcm-proj/lcm/releases
* Must install java jdk before building LCM
  ```
  sudo apt install openjdk-8-jdk
  ```
  Or LCM-java will not be built and installed. Will cause problems when build cheetah.
  It seems that `openjdk-11-jdk` does not work.

## After installing Cheetah
```
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
```