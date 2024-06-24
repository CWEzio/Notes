# Miscellany

## Initialize class member which is a reference
Check [this answer](https://stackoverflow.com/a/15403837/12825127).
The reference can only be initialized in the *constructor initializer list* (sec7.1, 265) as:
```cpp
Test (int &x) : t(x) {}
```

## C++: “undefined reference to” templated class function
Check [this answer](https://stackoverflow.com/questions/495021/why-can-templates-only-be-implemented-in-the-header-file), [this FAQ](https://stackoverflow.com/questions/495021/why-can-templates-only-be-implemented-in-the-header-file) or [this blog](https://bytefreaks.net/programming-2/c/c-undefined-reference-to-templated-class-function).

## std::map difference between `[]` and `emplace()`
Answer by chatgpt
1. `std::map::operator[]`:
    - If the key does not exist in the map, this will create a new element with that key and default-construct its value, then it will assign the given value to it.
    - If the key already exists, it will assign the given value to the existing element.
    - This method always involves a potentially unnecessary creation of a default-constructed value.
2. `std::map::emplace`:
    - This method constructs a new element from the provided key-value pair arguments and attempts to insert this into the map.
    - If the key already exists in the map, emplace does nothing, and the existing value remains unchanged. It also returns a pair, with an iterator pointing to the existing element and a boolean indicating that the insertion didn't take place.
    - Emplace has the advantage of "in-place" construction which means it constructs the new element directly in the memory where it will be stored within the map. This can be more efficient as it avoids the need to create a temporary object and then copy/move it into the map, which is what happens with the `operator[]`.

Example:
```c++
std::map<string, Binding<Constraint>> map;
map["c"] = prog.AddConstraint(...); // This will return error when compile, since Binding<> object does nit have a constructor taking no input.
map.emplace("c", prog.AddConstraint()); // This will works fine.
```

# clang
## configure `clang-tidy`
`clang-tidy` can be configured with the `.clang-tidy` file.
> The `.clang-tidy` file will overwrite settings of vscode.
Here is an example `.clang-tidy` file used by drake
```yaml
---
# This file is not used by CI and the checks included are not part of the Drake style guide
Checks: >
        clang-analyzer-*,
        clang-diagnostic-*,
        cppcoreguidelines-*,
        google-*,
        performance-*,
        modernize-*,
        -modernize-use-trailing-return-type,
        readability-*,
        -cppcoreguidelines-pro-bounds-array-to-pointer-decay,
        -cppcoreguidelines-pro-bounds-pointer-arithmetic,
        -cppcoreguidelines-pro-type-static-cast-downcast,
        -modernize-use-bool-literals,
        -modernize-use-transparent-functors,
        -modernize-use-using,
        -readability-else-after-return,
        -readability-named-parameter,

CheckOptions:
  - { key: readability-identifier-naming.ClassCase,           value: CamelCase  }
  - { key: readability-identifier-naming.NamespaceCase,       value: lower_case }
  - { key: readability-identifier-naming.PrivateMemberSuffix, value: '_'        }
  - { key: readability-identifier-naming.StructCase,          value: CamelCase  }
  - { key: readability-identifier-naming.VariableCase,        value: lower_case }
...

```
Here are somethings to note:
1. enable a check by adding the check's name like `google-*`
2. disable a check by adding `-` + check's name like `-modernize-use-trailing-return-type`


# link
> Use `ldconfig -p | grep *name*` to check the available version of certain library.
## How to link to `libglapi.so.0`
To compile `DifferentiableCloth`, I find that I met with the `/usr/bin/ld: cannot find -lglapi` issue. I have `libglapi.so.0` in folder `/lib/x86_64-linux-gnu`. To deal with this problem, I have several choices
1. `sudo ln -s /lib/x86_64-linux-gnu/libglapi.so.0 /usr/lib/libglapi.so`

2. Explicitly specify the linked library path like
    ```
    g++ your_code.cpp -o your_program /lib/x86_64-linux-gnu/libglapi.so.0
    ```
    or in a `setup.py` file, using
    ``` python
    ext_modules=[
    CppExtension(
        name='arcsim',
        ...
        extra_link_args=['/lib/x86_64-linux-gnu/libglapi.so.0']  # Directly specify the full path to the library
    )
    ]
    ```


# Eigen
## Use `auto` carefully with Eigen
Check [this answer](https://stackoverflow.com/a/47840292/12825127) and [this documentation page](https://eigen.tuxfamily.org/dox/TopicPitfalls.html) for more details.
In short,
```cpp
auto x = A.colPivHouseHolderQr().solve(b);
Eigen::VectorX<double> vec = x;
```
will return error in runtime.
The reason is that `auto` will make `x` of type `Solve<...>` instead of a vector, storing reference to dead object `tmp_qr`. This object will be solved when the value of `x` is used, i.e. the second line. However, at this time the `qr` object is already dead. Thus leading to error.
> Be careful using `auto` with Eigen object.<br>
> The official documentation suggests that do not use the `auto` keywords with Eigen's expression, unless you are 100% sure what you are doing.<br>
> This should be related with Eigen's lazy evaluation.

## Correct usage of Ref<T>
Check the official [documentation](https://eigen.tuxfamily.org/dox/classEigen_1_1Ref.html), [tutorial](https://eigen.tuxfamily.org/dox/classEigen_1_1Ref.html) and this [answer](https://stackoverflow.com/questions/21132538/correct-usage-of-the-eigenref-class). 
In short,
- Use `Ref<T>` for a writable reference
- Use `const Ref<const T>&` for a const reference

## *Aliasing* problem

Check this [official documentation page](https://eigen.tuxfamily.org/dox/group__TopicAliasing.html) for more details.

- In Eigen, aliasing refers to assignment statement in which the same matrix (or array or vector) appears on the left and on the right of the assignment operators. Statements like `mat = 2 * mat`; or `mat = mat.transpose()`; exhibit aliasing.
- This also relates to `Eigen`'s lazy evaluation mechanism. One possible solution is to evaluate explicitly, using the `eval()` method. For operations like `transpose`, you can also use the inplace version method `transposeInPlace()`.


# GDB usage
Refer to [this article](https://www.cs.cmu.edu/~gilpin/tutorial/) for basic gdb usage.
- `run`: run until the break point or error
- `bt`/`backtrace`: trace the error's call stack 
- `p`/`print` + `var`: print the value of `var` (`var` is the variable name)
- `b`/`break` + `path/to/file:N`: set a break point on the `N`th line of file 
- `b`/`break` + `function name`: set a break point on the beginning of function `function name`

## Debug C++ module in python
GDB can also used to debug the C++ module used by the python program. Example usage is
```
gdb --args python VCD/main.py --gen_data=1 --dataf=./data/vcd --num_variations=1
```
The usage is the same as debugging the pure `C++` program.

## Install drake_gdb (viewing eigen vector)
Use drake_gdb https://github.com/SeanCurtis-TRI/drake_gdb

* git clone https://github.com/SeanCurtis-TRI/drake_gdb

* Create the file ~/.gdbinit

* insert the following lines into .gdbinit
  ```
  python
  import sys
  sys.path.append("path of your drake_gdb")
  import drake_gdb 
  drake_gdb.register_printers()
  end
  ```

One thing to note is that I encounter bug when use the author's install instruction. It seems that gdb use /usr/bin/python which is python2.7. 
```
sys.path.insert(0, "$DRAKE_GDB_ROOT$")
```
given by the authors' instruction seems does not work. import drake_gdb will give
```python
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: No module named drake_gdb
```
Change this line to 
```
sys.path.append("path of your drake_gdb")
```
will solve this problem. Use $DRAKE_GDB_ROOT is not necessary.

> Alternatively, you can also use the [eigengdb](https://github.com/dmillard/eigengdb). Follow its readme for installation details.

## `std::optional`
Read this excellent [article](https://devblogs.microsoft.com/cppblog/stdoptional-how-when-and-why/).


