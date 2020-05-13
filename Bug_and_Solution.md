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
