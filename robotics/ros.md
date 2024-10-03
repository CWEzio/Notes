# Introduction 
My learning notes on ROS.

# Problems 
## No module named `python3-empy`
It turns out that the problem is not that I do not have installed `python3-empy`, it is that I mistakenly run `catkin-make` with my python virtual environment. I need to remove the previous build cache first:
```
cd ~/catkin_ws  
unlink src/CMakeLists.txt
trash build
trash devel
trash .catkin_workspace
```
and then run `catkin_make`.

## Use `ros` with `fish` 
[This article](https://yodahuang.github.io/articles/How-to-let-ROS-play-happily-with-fish/) is a good source.

In short,
1. To bring up functionalities like `roscd` and `rosed`,
  ```
  source /opt/ros/kinetic/share/rosbash/rosfish
  ```
2. To source catkin workspace,
   ```
  bass source devel/setup.bash
   ```

## `rosrun` cannot find my python script
It turns out that rosrun can automatic detect executable files. However, at first I need to make the script executable by `chmod +x` and add 
```python
#!/usr/bin/env python3
```
on the first line of the python script.


## Cannot open a node in a seperate terminal
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

## Gazebo
## how to solve the problem "Error in REST request" in Gazebo in Ubuntu18.04
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