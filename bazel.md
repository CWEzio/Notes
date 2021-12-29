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