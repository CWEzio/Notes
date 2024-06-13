# Introduction 
My learning notes on Bazel building system.\
ref: https://docs.bazel.build/versions/4.2.2/tutorial/cpp.html

# Basics
*workspace*: A directory containing both the source files and Bazel's building output. 
- The `WORKSPACE` file identifies the directory and the contents as a Bazel workspece. This file should be at the root.
- A directory in the workspace containing the `BUILD` file is a *package*.

`BUILD` file:
- BUILD file in `cpp-tutorial/stage/main`.
  ```bazel
  load("@rules_cc//cc:defs.bzl", "cc_binary")
  cc_binary(
      name = "hello-world",
      srcs = ["hello-world.cc"],
  )
  ```
  This example defines the `hello-world` *target*.
  The first line load the `cc_binary` rule:
  ```
  load("@rules_cc//cc:defs.bzl", "cc_binary")
  ```
- A target is an instance of a build rule.
- By default targets are only visible to other targets in the same `BUILD` file.

The project can be built with
```
bazel build //main:hello-world
```
The output is at `bazel-bin` directory.
- The `//main:` part is the location of the `BUILD` file reltative to the root of the workspace. `hello-world` is the target in the `BUILD` file to be built.
  

# `genrule`
Also see [the official documentation](https://bazel.build/reference/be/general#genrule.outs).

A `genrule` generates one or more files using a user-defined bash command.

See [here](https://shantanugoel.com/2020/05/03/bazel-rule-auto-generate-files-compile-time/) for one tutorial that use `genrule` to build header file which is then used as include file.

## `genrule` usage example 1 
One example of my usage is 
```python
genrule(
    name = "peg_in_hole_station_diagram",
    outs = ["peg_in_hole_station.txt"],
    cmd = "./$(location :simulation_test); mv ./peg_in_hole_station.txt $(location peg_in_hole_station.txt)",
    tools = [":simulation_test"],
)
```

here `:simulation_test` is a `cc_binary` target that will generate the `peg_in_hole_station.txt` file. However, since `bazel run` will run the target in a sandbox,  we need this `genrule` to get the generated file. The `cmd` in this `genrule` basically just run the `cc_binary` target to generate the `peg_in_hole_station.txt` file and then move it to the output location. 
> Use `$(location)` to get the target/out's location. `cmd` follows ["Make" Variable](https://bazel.build/reference/be/make-variables) substitution.<br>
> If the output has only one file, you can also use `$@` to refer to the output file.<br>
> If the input has only one file, you can use `$<` to refer to the input file. <br>
> `genrule` must have output file.<br>

The `peg_in_hole_station.txt` contains a `graph` string. I also define a `sh_binary` target to display it.
```python
sh_binary(
    name = "show_diagram",
    srcs = ["xdot.sh"],
    args = ["$(location :peg_in_hole_station_diagram)"],
    data = [":peg_in_hole_station_diagram"],
)
```

The contents of `xdot.sh` is simply
```
#! /bin/bash
xdot $1
```
`xdot.sh` will take the `graph` string text file as argument and use `xdot` to display it.

The `show_diagram` is simply
```
xdot.sh \path\to\peg_in_hole_station.txt
```

An alternative way to get the location of data file (in this case, `peg_in_hoe_station.txt`) is to use `$TEST_SRCDIR/$TEST_WORKSPACE` in the shell script. In this example, an alternative implementation is
```python
sh_binary(
    name = "show_diagram",
    srcs = ["xdot.sh"],
    data = [":peg_in_hole_station_diagram"],
)
```
`xdot.sh`:
```
#! /bin/bash
xdot  $TEST_SRCDIR/$TEST_WORKSPACE/kuka/peg_in_hole_station_diagram
```

However, the later implementation seems to be more error prone.


# Configurable Build Attributes
Refer to [the official documentation](https://bazel.build/docs/configurable-attributes) and [this tutorial](https://zhuanlan.zhihu.com/p/521591945).

> 
## Usage example in `drake`<br>
In `tools/BUILD.bazel`, they define
```python
config_setting(
    name = "with_mosek",
    values = {"define": "WITH_MOSEK=ON"},
)
```

Then in `tools/install/libdrake/BUILD.bazel`, they use this defined flag to determine whether to build against `Mosek`:
```python
cc_library(
    name = "mosek_deps",
    deps = select({
        "//tools:with_mosek": ["@mosek"],
        "//conditions:default": [],
    }),
)
```
Therefore, if `with_mosek` is defined by passing flag `--define=WITH_MOSEK=ON`, then the mosek dependency will be selected.
> Note that, however, `bazel` recommands to use the custom flag as in [this example](https://github.com/bazelbuild/examples/tree/HEAD/configurations/select_on_build_setting).
    
# Usage 
## Use labels to reference target

Bazel use *labels* to reference targets. The syntax is:
```
//path/to/package:target-name
```
The target can be a *rule target* or *file target*. `//` is the workspace root identifier.

Some special cases
- If the referencing target is at the workspace root, the package path is empty, just use `//:target-name`.
- When referencing target in the same `BUILD` file, you can just use `:target-name`, the `//` can be skipped.
- Use `bazel build //...` to build the entire project and `bazel test //...` to build and test the entire project. The `...` means `everything including the subdirectories`
- The  `bazel run` command is similar to `bazel build` command, except it is used to build and run a single target.

## Working with external Dependencies
> Note that Bazel 5.0 and later introduced a new external dependency system, codenamed "Bzlmod". However, Drake use Bazel 4.2.

## Using Bazel with VS code autocomplete 
I work with Bazel C++ project using VS code. One thing that is useful is to go to declaration position. However, the intellisense do not know the source of the included file. I solve this problem using method suggested https://stackoverflow.com/questions/61015990/how-do-i-enable-c-intellisense-for-a-bazel-project-in-vs-code

The `compile_commands.json` can be generated using [bazel-compilation-database](https://github.com/grailbio/bazel-compilation-database). 

Run
```zsh
bazel-compdb
```
at the project root.

Then `Ctrl+Shift+p`, type `C++` to edit configuration file of VS code C++ plugin. Add 
```json
"compileCommands": "${workspaceFolder}/compile_commands.json"
```
to the `c_cpp_properties.json` file.

> Additional note for Drake. It should be noted that Drake use `C++17` standard. To avoid intellisense error, need to change the `cppStandard` setting in `c_cpp_properties.json` file to `gnu++17`.

*Update*: 2022.07.13, the `bazel-compilation-database` is in maintainance mode now and the author suggest another tool, [`bazel-compile-commands-extractor`](https://github.com/hedronvision/bazel-compile-commands-extractor).
Follow the official detailed guidance to do the setup. Here I give a short version (the steps that I follow):
1. Add the following to the Bazel `WORKSPACE` file.
```python
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")


# Hedron's Compile Commands Extractor for Bazel
# https://github.com/hedronvision/bazel-compile-commands-extractor
http_archive(
    name = "hedron_compile_commands",

    # Replace the commit hash in both places (below) with the latest, rather than using the stale one here.
    # Even better, set up Renovate and let it do the work for you (see "Suggestion: Updates" in the README).
    url = "https://github.com/hedronvision/bazel-compile-commands-extractor/archive/ac78f5f1e2679b9f5b62eeae33055ede355672e6.tar.gz",
    strip_prefix = "bazel-compile-commands-extractor-ac78f5f1e2679b9f5b62eeae33055ede355672e6",
    # When you first run this tool, it'll recommend a sha256 hash to put here with a message like: "DEBUG: Rule 'hedron_compile_commands' indicated that a canonical reproducible form can be obtained by modifying arguments sha256 = ..."
)
load("@hedron_compile_commands//:workspace_setup.bzl", "hedron_compile_commands_setup")
hedron_compile_commands_setup()
```

2. Run 
```zsh
bazel run @hedron_compile_commands//:refresh_all
```
to update the `compile_commands.json`. Note that they will automatic modify your `.gitignore` to help you avoid mistake add `compile_commands.json` to your git repo.

Editor setup

1. install `clangd`
```zsh
code --install-extension llvm-vs-code-extensions.vscode-clangd
# We also need make sure that Microsoft's C++ extension is not involved and interfering.
code --uninstall-extension ms-vscode.cpptools
```
2. Then, open VSCode user settings, so things will be automatically set up for all projects you open.
3. Search for "clangd".
4. Add the following three separate entries to `clangd.arguments`:
```
--header-insertion=never
--compile-commands-dir=${workspaceFolder}/
--query-driver=/**/*
```



## Build drake-py with cmake will make the computer froze
This is because that without addtional setting, the bazel will utilize all cpu cores and this will run up all memory. However, since cmake calls bazel indirectly, simply put `-j6` does not help. The solution is to add 
```
build --jobs 6 --local_ram_resources=HOST_RAM*0.5
```
to `.bazelrc` file.
For more info, refer to this [github issue](https://github.com/tensorflow/models/issues/195)

## Attributes common to all build rules
For details, check [the official documentation](https://bazel.build/reference/be/common-definitions).
> 1. `tags`: list of strings; optional <br>
> `requires-network` keyword allows access to external network. 

## Cleaning built outputs
For details, check [the official documentation](https://bazel.build/reference/be/common-definitions).
Run
```
bazel clean
```

## Easier Debugging of Sandbox Failures
Check [this blog](https://blog.bazel.build/2016/03/18/sandbox-easier-debug.html).
> I fail to enter the sandbox following the instructions in the blog. However, I decide to leave it for now and check in the future.

## Header include path 
Check [stackoverflow question](https://stackoverflow.com/questions/59932849/bazel-cannot-find-dependent-header-file-for-very-simple-case) for an example.

In short, `deps` in `cc_binary` does not add the `cc_library`'s `hdrs` into its include path. However, you can specify the `includes` attribute in `cc_library`. The `cc_binary` that depends on that `cc_library` will add the directory specified in the `includes` attribute to its include path. Or you can use the full include path. See the official documentation on the ['includes' attribute](https://bazel.build/reference/be/c-cpp#cc_library.includes).


## vscode
Use the [`bazel-stack-vscode`](https://marketplace.visualstudio.com/items?itemName=StackBuild.bazel-stack-vscode) extension. It support lint, format and autocomplete.


# Problems
## `python-config` file not found when I try to run jupyter notebook with bazel directly
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

