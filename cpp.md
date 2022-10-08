## Initialize class member which is a reference
Check [this answer](https://stackoverflow.com/a/15403837/12825127).
The reference can only be initialized in the *constructor initializer list* (sec7.1, 265) as:
```cpp
Test (int &x) : t(x) {}
```

## C++: “undefined reference to” templated class function
Check [this answer](https://stackoverflow.com/questions/495021/why-can-templates-only-be-implemented-in-the-header-file), [this FAQ](https://stackoverflow.com/questions/495021/why-can-templates-only-be-implemented-in-the-header-file) or [this blog](https://bytefreaks.net/programming-2/c/c-undefined-reference-to-templated-class-function).

## GDB usage
`run`: run until the break point or error
`bt`/`backtrace`: trace the error's call stack 
`p`/`print` + `var`: print the value of `var` (`var` is the variable name)
`b`/`break` + `path/to/file`:`N`: set a break point on the `N`th line of file 
`b`/`break` + `function name`: set a break point on the beginning of function `function name`

### Install drake_gdb (viewing eigen vector)
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
will solve this problem. Also use $DRAKE_GDB_ROOT is not necessary.

> alternatively, you can also use the [eigengdb](https://github.com/dmillard/eigengdb). Follow its readme for installation details.

## `std::optional`
Read this excellent [article](https://devblogs.microsoft.com/cppblog/stdoptional-how-when-and-why/).


