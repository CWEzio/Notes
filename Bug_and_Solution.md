# Bug and Solution

## Cannot open terminal with ctrl+alt+t
At first I think the problem is locale configuration is wrong. However, after reconfigure locale with every methods I have found from internet, this problem persists.

I note https://askubuntu.com/questions/1040566/cant-open-terminal-in-ubuntu-18-04-after-upgrade-from-17-10.

It turns out that the issue is because I have set the default python3 to be python3.7. 

The solution is
```console
code /usr/bin/gnome-terminal
```
 The first line is 
 ```
#! /usr/bin/python3
 ```
 Change it to 
 ```
 #! /usr/bin/python3.6 
 ```
Works like magic!

## Missing Ros Packages
Problem description
```
-- Using CATKIN_DEVEL_PREFIX: /home/chenwang/Documents/towr_icra17/build/devel
-- Using CMAKE_PREFIX_PATH: /opt/ros/melodic
-- This workspace overlays: /opt/ros/melodic
-- Found PythonInterp: /usr/bin/python2 (found suitable version "2.7.17", minimum required is "2") 
-- Using PYTHON_EXECUTABLE: /usr/bin/python2
-- Using Debian Python package layout
-- Using empy: /usr/bin/empy
-- Using CATKIN_ENABLE_TESTING: ON
-- Call enable_testing()
-- Using CATKIN_TEST_RESULTS_DIR: /home/chenwang/Documents/towr_icra17/build/test_results
-- Found gtest sources under '/usr/src/googletest': gtests will be built
-- Found gmock sources under '/usr/src/googletest': gmock will be built
-- Found PythonInterp: /usr/bin/python2 (found version "2.7.17") 
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Looking for pthread_create
-- Looking for pthread_create - not found
-- Looking for pthread_create in pthreads
-- Looking for pthread_create in pthreads - not found
-- Looking for pthread_create in pthread
-- Looking for pthread_create in pthread - found
-- Found Threads: TRUE  
-- Using Python nosetests: /usr/bin/nosetests-2.7
-- catkin 0.7.23
-- BUILD_SHARED_LIBS is on
-- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
-- Could NOT find rviz_visual_tools (missing: rviz_visual_tools_DIR)
-- Could not find the required component 'rviz_visual_tools'. The following CMake error indicates that you either need to install the package with the same name or change your environment so that it can be found.
CMake Error at /opt/ros/melodic/share/catkin/cmake/catkinConfig.cmake:83 (find_package):
  Could not find a package configuration file provided by "rviz_visual_tools"
  with any of the following names:

    rviz_visual_toolsConfig.cmake
    rviz_visual_tools-config.cmake

  Add the installation prefix of "rviz_visual_tools" to CMAKE_PREFIX_PATH or
  set "rviz_visual_tools_DIR" to a directory containing one of the above
  files.  If "rviz_visual_tools" provides a separate development package or
  SDK, be sure it has been installed.
Call Stack (most recent call first):
  CMakeLists.txt:5 (find_package)


-- Configuring incomplete, errors occurred!
See also "/home/chenwang/Documents/towr_icra17/build/CMakeFiles/CMakeOutput.log".
See also "/home/chenwang/Documents/towr_icra17/build/CMakeFiles/CMakeError.log".
```
Solution
```
sudo apt-get install ros-melodic-rviz-visual-tools
```
Check this https://answers.ros.org/question/335514/ros-graph-messages-build/ for more information.

## No module named 'rospkg'
Problem description
```
... logging to /home/chenwang/.ros/log/e247848c-94f8-11ea-8e01-04d4c448372e/roslaunch-chenwang-System-Product-Name-12456.log
Checking log directory for disk usage. This may take a while.
Press Ctrl-C to interrupt
Done checking log file disk usage. Usage is <1GB.

xacro: in-order processing became default in ROS Melodic. You can drop the option.
xacro: in-order processing became default in ROS Melodic. You can drop the option.
xacro: in-order processing became default in ROS Melodic. You can drop the option.
substitution args not supported:  No module named 'rospkg'
when processing file: /opt/ros/melodic/share/xpp_hyq/urdf/hyq.urdf.xacro
RLException: while processing /opt/ros/melodic/share/xpp_hyq/launch/all.launch:
while processing /opt/ros/melodic/share/xpp_hyq/launch/hyq.launch:
Invalid <param> tag: Cannot load command parameter [hyq_rviz_urdf_robot_description]: command [['/opt/ros/melodic/lib/xacro/xacro', '--inorder', '/opt/ros/melodic/share/xpp_hyq/urdf/hyq.urdf.xacro']] returned with code [2]. 

Param xml is <param command="$(find xacro)/xacro --inorder '$(find xpp_hyq)/urdf/hyq.urdf.xacro'" name="hyq_rviz_urdf_robot_description"/>
The traceback for the exception was written to the log file
```

Solution
```
conda install -c conda-forge rospkg
```
Check this https://answers.ros.org/question/331455/xacro-substitution-args-not-supported-no-module-named-rospkg/ for more information.


## Bug when using tensorboard with pytorch
* Problem detail
```python
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-13-39c22274501e> in <module>
     19 writer.add_embedding(features,
     20                     metadata=class_labels,
---> 21                     label_img=images.unsqueeze(1))
     22 writer.close()

~/anaconda3/envs/legged_env/lib/python3.7/site-packages/torch/utils/tensorboard/writer.py in add_embedding(self, mat, metadata, label_img, global_step, tag, metadata_header)
    779         save_path = os.path.join(self._get_file_writer().get_logdir(), subdir)
    780 
--> 781         fs = tf.io.gfile.get_filesystem(save_path)
    782         if fs.exists(save_path):
    783             if fs.isdir(save_path):

~/.local/lib/python3.7/site-packages/tensorflow_core/python/util/module_wrapper.py in __getattr__(self, name)

AttributeError: module 'tensorflow._api.v1.io.gfile' has no attribute 'get_filesystem'

```
* solution
  
  It seems that pytorch and tensorflow conflict with each other. Deleting pytorch, tensorflow, tensorboard then reinstalling pytorch and tensorboard fix this bug.

See this issue https://github.com/pytorch/pytorch/issues/30966

## CMAKE error, cannot find catkin package
### error log
```
By not providing "Findcatkin.cmake" in CMAKE_MODULE_PATH this project has
asked CMake to find a package configuration file provided by "catkin", but
CMake did not find one.

Could not find a package configuration file provided by "catkin" with any
of the following names:

catkinConfig.cmake
catkin-config.cmake

Add the installation prefix of "catkin" to CMAKE_PREFIX_PATH or set
"catkin_DIR" to a directory containing one of the above files. If "catkin"
provides a separate development package or SDK, be sure it has been
installed.
```
### Solution
I have already install ros. It turns out the problem is caused by not sourcing setup.bash. Since I use clion, I need to at first source the setup.bash file at terminal and then use that terminal to open clion. 
```console
source /opt/ros/melodic/setup.zsh 
```

## Undefined Symbol when using pybind11
When I use pybind11 to write a python wrapper of a c++ robot controller, I encounter a undefined symbol issue. At first I directly compile a module with all cpp files (robot controller source files and binder souce files), then import this module from python will cause undefined symbol issue. Then I at first compile the robot controller into a library, then link it with the pybind11 generated module. This solves the undefined symbol problem. To be honest I do not know the actual reason, but there are some clues from this helpful website https://stackoverflow.com/questions/12573816/what-is-an-undefined-reference-unresolved-external-symbol-error-and-how-do-i-fix.

Also when look at the undefined symbol problem, some tools are helpful. Such as c++filt, this can use to demangle the symbol that a linker sees. 

```
nm RobotCtrl_py.cpython-37m-x86_64-linux-gnu.so --demangle -Du

```
This command can be used to see the symbol information in a binary file.
Check this https://medium.com/fcamels-notes/%E8%A7%A3%E6%B1%BA-linux-%E4%B8%8A-c-c-%E7%9A%84-undefined-symbol-%E6%88%96-undefined-reference-a80ee8f85425 website for further information.


## Gazebo
### Symbolic lookup error
Detail:
```
gazebo: symbol lookup error: /usr/lib/x86_64-linux-gnu/libsdformat.so.6: undefined symbol: _ZTIN8ignition4math2v45ColorE
```
Solution:
```Bash
sudo apt upgrade
```

## Python
### Missing dependencies for SOCKS support
Currently I am using electron-ssr. However, this gives me an exception 
```
requests.exceptions.InvalidSchema: Missing dependencies for SOCKS support.      

```
I solve this by 
```console
export all_proxy="http://127.0.0.1:12333"
```
Here http://127.0.0.1:12333 is my ssr port address.
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
## Docker 
### Tensorflow docker image
Need to include flag `runtime=nvidia`


## Drake
### Problem description
When I try to [run jupyter notebook with bazel](https://github.com/RobotLocomotion/drake/blob/master/tools/jupyter/README.md#running-notebooks) using virtual environment, I encounter a problem that it says python-config file is not found. 
```
bazel run //tools/jupyter:example
```

```zsh
INFO: Repository python instantiated at:
  /home/chenwang/drake/WORKSPACE:10:22: in <toplevel>
  /home/chenwang/drake/tools/workspace/default.bzl:317:29: in add_default_workspace
  /home/chenwang/drake/tools/workspace/default.bzl:234:26: in add_default_repositories
Repository rule python_repository defined at:
  /home/chenwang/drake/tools/workspace/python/repository.bzl:283:36: in <toplevel>
ERROR: An error occurred during the fetch of repository 'python':
   Traceback (most recent call last):
	File "/home/chenwang/drake/tools/workspace/python/repository.bzl", line 136, column 37, in _impl
		py_info = repository_python_info(
	File "/home/chenwang/drake/tools/workspace/python/repository.bzl", line 111, column 13, in repository_python_info
		fail((
Error in fail: Cannot find corresponding config executable: /home/chenwang/manipulation_env/bin/python3-config
  From interpreter: /home/chenwang/manipulation_env/bin/python3
ERROR: Error fetching repository: Traceback (most recent call last):
	File "/home/chenwang/drake/tools/workspace/python/repository.bzl", line 136, column 37, in _impl
		py_info = repository_python_info(
	File "/home/chenwang/drake/tools/workspace/python/repository.bzl", line 111, column 13, in repository_python_info
		fail((
Error in fail: Cannot find corresponding config executable: /home/chenwang/manipulation_env/bin/python3-config
  From interpreter: /home/chenwang/manipulation_env/bin/python3
ERROR: /home/chenwang/drake/tools/jupyter/BUILD.bazel:31:24: //tools/jupyter:example depends on //:module_py in repository @ which failed to fetch. no such package '@python//': Cannot find corresponding config executable: /home/chenwang/manipulation_env/bin/python3-config
  From interpreter: /home/chenwang/manipulation_env/bin/python3
ERROR: Analysis of target '//tools/jupyter:example' failed; build aborted: Analysis failed
INFO: Elapsed time: 0.087s
INFO: 0 processes.
FAILED: Build did NOT complete successfully (0 packages loaded, 0 targets conf\
FAILED: Build did NOT complete successfully (0 packages loaded, 0 targets conf\
igured)
    currently loading: 
```
### Solution
It turns out that this problem is due to the *virtualenv* do not create `python-config` file. I have to create a symlink to refer to it explicitly. In this issue, the virtual environment is at 
`/home/chenwang/manipulation_env`. What I do is 
```zsh 
cd /home/chenwang/manipulation_env
ln -s /usr/bin/python3.6-config python3-config
```
Then 
```
bazel run //tools/jupyter:example
```
works fine.
> Later I decide not to use virtualenv with bazel becasue it causes bug (`Python.h` not found).

### Problem description
Encounter "RuntimeError: The operating system's C++ standard library is not installed correctly" when build drake from source using bazel. 
I ask a [question](https://stackoverflow.com/questions/70604791/encounter-runtimeerror-the-operating-systems-c-standard-library-is-not-inst/70615466#70615466) in stackoverlow, which contains the detail.

### Solution
I solve this problem follows @jwnimmer-tri's answer in my question. In specific, I run
```
sudo apt remove cpp-9 g++-9 gcc-9 libasan6 libgcc-9-dev libstdc++-9-dev
sudo apt remove cpp-8 g++-8 gcc-8 libasan6 libgcc-8-dev libstdc++-8-dev
```
to remove GCC 8 and GCC 9 from my system. I must have accidentally installed GCC 8 and GCC 9, but not the corresponding G++. Drake use GCC 7 in `Ubuntu 18.04` and the `install_prereqs.sh` install GCC 7 fully (with G++). Clang seems to look for GCC standard C++ library and for higher version GCC. Therefore, when Clang looks for GCC standard C++ library in my system, it looks for GCC 9 and GCC 8. However, I do not have the corresponding G++ installed. Therefore, the Clang will fails to find the include file.

Another possible way to solve this problem is to have the higher version GCC fully installed.

### Problem Description
When using `pydot`, encounter
```python
FileNotFoundError: [Errno 2] "dot" not found in path.
```
### Solution
```zsh
sudo apt install graphviz
```
> It should be noted that, for `pip` installed `pydot`, it is of no use to install `graphviz` via `pip`. I do not know the reason though. I guess this has something to do with the `pydot` search path. Install `graphviz` via `apt` will solve the problem.

### Problem Description
`jupyter notebook` return error:
```zsh
[IPKernelApp] ERROR | No such comm target registered: jupyter.widget.version
```

### Solution
(In virtual enviornment)
```zsh
jupyter nbextension install --py widgetsnbextension --user
jupyter nbextension enable --py widgetsnbextension
```
> The above two commands should be run in the right `virtual environment`, i.e., where the kernel returning the error information is in.

### Problem Description
Encounter
```
Executing: /tmp/apt-key-gpghome.PpjCyyBi8S/gpg.1.sh -q --fetch-keys https://bazel.build/bazel-release.pub.gpg
gpg: WARNING: unable to fetch URI https://bazel.build/bazel-release.pub.gpg: Connection timed out
```
when install the `manipulation` course supplement's prerequiste using the provided shell script `install_prereqs.sh`.
### Solution
This is because of the GFW. The apt-key cannot get the key. Need to first manully download the key from https://bazel.build/bazel-release.pub.gpg. Then run 
```zsh
sudo apt-key add key.gpg
```
suppose the key file is named `key.gpg`.

Or one can use a single command to do the trick:
```zsh
curl -sSL \
'https://bazel.build/bazel-release.pub.gpg' \
| sudo apt-key add -
```

### Problem Description
When use pydrake, encounter the `AttributeError`:
```python
AttributeError: module 'drake' has no attribute 'experimental_lcmt_deformable_tri'

The above exception was the direct cause of the following exception:
...
```
### Solution
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


## ROS
### Cannot open a node in a seperate terminal
I would like to open a node in a seperate terminal and thus I use 
```xml
  <node name="teleop" pkg="turtlebot3_teleop" type="turtlebot3_teleop_key" output="screen" launch-prefix="xterm -e" />
```
However, I encounter

```
RLException: Roslaunch got a 'No such file or directory' error while attempting to run:

xterm -e /opt/ros/noetic/lib/turtlebot3_teleop/turtlebot3_teleop_key __name:=teleop __log:=/home/chenwang/.ros/log/75148da4-dfee-11ec-896f-1372cd24759d/evader-teleop-6.log

Please make sure that all the executables in this command exist and have
executable permission. This is often caused by a bad launch-prefix.
The traceback for the exception was written to the log file
```

### Solution
It turns out that the error message is misleading. I got this error because I haven't have `xterm` installed. After installing `xterm`, everything works fine again.

