
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

