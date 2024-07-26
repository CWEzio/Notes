# Flags
- `-DCMAKE_BUILD_TYPE=Debug` sets the build to debug mode.


# Usage
## cmake double quoted variables or no quote
Check [this answer](https://stackoverflow.com/questions/35847655/when-should-i-quote-cmake-variables) for more details. In short:
- CMake is a script language and arguments are evaluated after the variables are expanded
- CMake differentiates between normal strings and list variables (strings with semicolon delimiters)
For example:
- `set(_my_text "A B C")` with `message("${_my_text}")` would give `A B C`
- `set(_my_list A B C)` with `message("${_my_list}")` would give `A;B;C`
- `set(_my_list "A" "B" "C")` with `message("${_my_list}")` would give `A;B;C`
- `set(_my_list "A" "B" "C")` with `message(${_my_list})` would give `ABC`

## Choose generator
- Use `cmake --help` to see a list of avaliable generator
- Use `cmake -G "Ninja" /path/to/source` to choose `Ninja` as the build system
- Use `cmake -G "Unix Makefiles" /path/to/source` to choose `make` as the build system
> Usually `Ninja` is faster, especially in incremental build. So why not choose `Ninja`
- Use `cmake --build /path/to/source` to automatically choose the correct build system and build the program.


## custom target that copy certain files in src to build
```c
add_custom_target(move_and_log ALL COMMAND ${CMAKE_COMMAND} -E copy
                                           ${CMAKE_CURRENT_SOURCE_DIR}/move_and_log.sh
                                           ${CMAKE_CURRENT_BINARY_DIR}/move_and_log.sh)

```
- This snippet define a target `move_and_log` and its associated command.
- `ALL`: Indicate taht this target should be added to the default build target so that it will be run every time.


## generate `compile_commands.json`
`vscode` requires `compile_commands.json` to understand the c++ project and do intellisense. In order to generate the `compile_commands.json` in a `cmake` project, run the following command:
```
cmake .. -DCMAKE_EXPORT_COMPILE_COMMANDS=ON && mv compile_commands.json ..
```
This command will generate a `compile_commands.json` file which will then be moved to the upper-level folder.

# Problems 
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

## The compiled code does not have debug symbols
It seems that just setting the `CMAKE_CXX_FLAGS` to have ` -g -Og` flags is not enough. The build type can overide those settings. I need to use `-DCMAKE_BUILD_TYPE=Debug`. 
