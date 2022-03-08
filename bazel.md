# Bazel
My learning notes on Bazel building system.\
ref: https://docs.bazel.build/versions/4.2.2/tutorial/cpp.html

## Basics
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

## Using Bazel with VS code intellisense
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

## Build drake-py with cmake will make the computer froze
This is because that without addtional setting, the bazel will utilize all cpu cores and this will run up all memory. However, since cmake calls bazel indirectly, simply put `-j6` does not help. The solution is to add 
```
build --jobs 6 --local_ram_resources=HOST_RAM*0.5
```
to `.bashrc` file.
For more info, refer to this [github issue](https://github.com/tensorflow/models/issues/195)
