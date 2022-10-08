
## cmake double quoted variables or no quote
Check [this answer](https://stackoverflow.com/questions/35847655/when-should-i-quote-cmake-variables) for more details. In short:
- CMake is a script language and arguments are evaluated after the variables are expanded
- CMake differentiates between normal strings and list variables (strings with semicolon delimiters)
For example:
- `set(_my_text "A B C")` with `message("${_my_text}")` would give `A B C`
- `set(_my_list A B C)` with `message("${_my_list}")` would give `A;B;C`
- `set(_my_list "A" "B" "C")` with `message("${_my_list}")` would give `A;B;C`
- `set(_my_list "A" "B" "C")` with `message(${_my_list})` would give `ABC`

