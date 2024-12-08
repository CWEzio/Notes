# Table of content
- [Table of content](#table-of-content)
- [Environment setup](#environment-setup)
  - [setup `ros` environment in `fish`](#setup-ros-environment-in-fish)
  - [Install dependencies with `rosdep`](#install-dependencies-with-rosdep)
- [Problems](#problems)
  - [Unsorted](#unsorted)
    - [No module named `python3-empy`](#no-module-named-python3-empy)
    - [`rosrun` cannot find my python script](#rosrun-cannot-find-my-python-script)
    - [Cannot open a node in a seperate terminal](#cannot-open-a-node-in-a-seperate-terminal)
    - [Missing Ros Packages](#missing-ros-packages)
    - [No module named 'rospkg'](#no-module-named-rospkg)
  - [`fish`](#fish)
    - [Autocomplete error in fish](#autocomplete-error-in-fish)
  - [Gazebo](#gazebo)
    - [how to solve the problem "Error in REST request" in Gazebo in Ubuntu18.04](#how-to-solve-the-problem-error-in-rest-request-in-gazebo-in-ubuntu1804)
    - [Symbolic lookup error](#symbolic-lookup-error)
  - [using python virtual environment](#using-python-virtual-environment)
    - [`symbol not found` issue](#symbol-not-found-issue)

# Environment setup

## setup `ros` environment in `fish` 
[This article](https://yodahuang.github.io/articles/How-to-let-ROS-play-happily-with-fish/) is a good source.

In short,
1. To bring up functionalities like `roscd` and `rosed`,
  ```
    source /opt/ros/noetic/share/rosbash/rosfish
  ```
2. Setup ros env
   ```
    bass source /opt/ros/noetic/setup.bash
   ```
3. Setup catkin workspace env, source its `setup.bash` 
   ```
    bass source devel/setup.bash
   ```

## Install dependencies with `rosdep`
1. Initialize `rosdep` if you haven't done so:
    ```
    sudo rosdep init
    rosdep update
    ```
2. Navigate to your ros workspace:
   ```
   cd <path-to-your-workspace>
   ``` 
3. Install dependencies
    ```
    rosdep install --from-paths src --ignore-src -r -y
    ```
    - `--from-paths src`: specifies the `src` directory of your workspace, where the packages are located. 
    - `--ignore-src`: prevent reinstalling dependencies that has been already installed.
    - `-r`: recursively resolve dependencies
    - `-y`: confirm installation


# Problems 
## Unsorted
### No module named `python3-empy`
It turns out that the problem is not that I do not have installed `python3-empy`, it is that I mistakenly run `catkin-make` with my python virtual environment. I need to remove the previous build cache first:
```
cd ~/catkin_ws  
unlink src/CMakeLists.txt
trash build
trash devel
trash .catkin_workspace
```
and then run `catkin_make`.


### `rosrun` cannot find my python script
It turns out that rosrun can automatic detect executable files. However, at first I need to make the script executable by `chmod +x` and add 
```python
#!/usr/bin/env python3
```
on the first line of the python script.


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

**Solution**
It turns out that the error message is misleading. I got this error because I haven't have `xterm` installed. After installing `xterm`, everything works fine again.


### Missing Ros Packages
**Problem description**
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
**Solution**
```
sudo apt-get install ros-melodic-rviz-visual-tools
```
Check this https://answers.ros.org/question/335514/ros-graph-messages-build/ for more information.

### No module named 'rospkg'
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

## `fish`
### Autocomplete error in fish
When autocomplete the ros command in `fish`, I encountered this problem:
```
string split: -type f -perm /111: unknown option

/opt/ros/noetic/share/rosbash/rosfish (line 1):
string split --max 1 --right / $argv[1]
^
in command substitution
        called on line 578 of file /opt/ros/noetic/share/rosbash/rosfish
in function '_roscomplete_search_dir' with arguments '-type\ f\ -perm\ /111'
        called on line 648 of file /opt/ros/noetic/share/rosbash/rosfish
in function '_roscomplete_rosrun'
in command substitution
```

It turns out that the issue is caused by a bug in the `rosfish` file. To fix it, modify:
```fish
set path (string split --max 1 --right / $argv[1])[1]
```
in line 578 to
```
set path (string split --max 1 --right / $arg)[1]
```

## Gazebo
### how to solve the problem "Error in REST request" in Gazebo in Ubuntu18.04
change ~/.ignition/fuel/config.yaml from
      url: <https://api.ignitionfuel.org>
  to
      url: <https://api.ignitionrobotics.org>

### Symbolic lookup error
Detail:
```
gazebo: symbol lookup error: /usr/lib/x86_64-linux-gnu/libsdformat.so.6: undefined symbol: _ZTIN8ignition4math2v45ColorE
```
Solution:
```Bash
sudo apt upgrade
```


## using python virtual environment

### `symbol not found` issue
- Ref: [this csdn notes](https://blog.csdn.net/qq_38606680/article/details/129118491)

I encounter the following issue when using `cv_bridge` in a `python3.10` conda environment.
```
ImportError: /lib/x86_64-linux-gnu/libp11-kit.so.0: undefined symbol: ffi_type_pointer, version LIBFFI_BASE_7.0
```

The cause of this issue is that I am using a new python version and it installs new packages. However, the ros package is old, which cause version conflicts.

In this case, it is caused by the `libffi` python package, which create a `libffi.7.so` to `libffi.8.so`.

The solution is:

1. 
    ```
    cd ~/miniconda3/envs/<env-name>/lib
    ```
2. Check the lib with error
    ```
    ls -l | grep ffi
    ```
    You would see `libffi.7.so` is linked to version 8 (or alike).

3. Delete `libffi.7.so libffi.so.7`  

4. Create new link
    ```
    sudo ln -s /lib/x86_64-linux-gnu/libffi.so.7 libffi.7.so
    sudo ln -s /lib/x86_64-linux-gnu/libffi.so.7 libffi.so.7
    ```


