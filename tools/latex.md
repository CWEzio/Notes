# Sharp-bits
- New line `\\` shouldn't be used inside `$$`.
- `\\` should be used for newline instead of `\newline` in algorithm or tabular environment.
    - `\\` works well for algorithm and tabular environment.
    - `\newline` creates a paragraph like break.

# `Algorithm2e`
## comment
- `\tcp{}`
  - Add `\\` style comment (single-line comment, though it can be multiple line).
  - Places the comment on its own line.
- `\tcp*[r]` 
    - variant of `\tcp{}`
    - Puts the comment on the same line (in algorithm sense, the rendered content can be multiple lines) as preceding code.
    - Finish the line automatically. Do not add line break after it.
    - Leave no space between the code and `\tcp*[r]`, or the semicolon `;` would have additional space before it.
    - Example
        ```latex
        x \leftarrow 0\tcp*[r]{Initialize x to zero}
        % no space between 0 and \tcp*[r]
        % no semicolon `;` at the end of code line
        % No need to add `//` after \tcp*[r]{}
        ```
- `\tcc`
    - Add `/* ... */` style comment (multi-line comment)
    - Automatically start next line (in algorithm sense)
    - Example
        ```latex
        x \leftarrow 0\;
        \tcc{Initialize x to zero}
        % No need to add `//` after \tcc{}
        ```
- `\tcc*[h]`
    - Similar display as `\tcc`, but does not start the next line automatically. Need to add `//` at the end to start the next line. 
- Typical usage
    - Use `\tcp*[r]` for inline comment.
    - Use `\tcc` for multiple line comment

# Memo
- Define command
    ```latex
    \newcommand{\commandname}[num_args][default]{definition}
    ```
    - `default` provides default value for the first argument. `\newcommand` only allows providing default value for the first argument.
- Strike out math.
    - `\sout{}` can only be used to strike out text. To strike out math equations, we need to manually draw the strike out line.
    - One example is given below:
        ```latex
        \usepackage{tikz}
        \usetikzlibrary{shapes,arrows,positioning,calc, tikzmark}
        \begin{align*}
        \tikzmarknode{A}{\dot{\bfx}_s^i}  &= f_\text{SI} (\bfx_s^i , \bfu^i_s) =  \dot{\bfp}^i  = \tikzmarknode{B}{\bfu^i_s}  \\ 
        \tikzmarknode{C}{\dot{\bfx}_d^i}  &= f_\text{DI}(\bfx_d^i,\bfu^i_d) = \bmat{\dot{\bfp}^i  \\ \dot{\bfv}^i } =
                    \bmat{\bfzero & \bfI \\ \bfzero & \bfzero} \bfx^i  + \bmat{\bfzero \\ \bfI} \tikzmarknode{D}{\bfu^i_d} 
        \end{align*}

        \begin{tikzpicture}[remember picture, overlay]
        \draw ([shift={(-0.5,0)}]A.mid west) -- ([shift={(0.5,0)}]B.mid east);
        \draw ([shift={(-0.5,0)}]C.mid west) -- ([shift={(0.5,0)}]D.mid east);
        \end{tikzpicture}
        ```
    - The key is to use `\tikzmarknode` to add anchor points and then draw the line using `tikzpicture`. Details can be found in the example above.
