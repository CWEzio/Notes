# VSCode environment
## Setup
- (MacOS) Install [`MacTex`](https://www.tug.org/mactex/).
- Install `Latex Workshop` extension on VSCode.
- Add the following config to VSCode setting
    ```json
    "latex-workshop.intellisense.package.enabled": true,
    "latex-workshop.latex.outDir": "./build",
    "latex-workshop.latex.recipe.default": "lastUsed",

    "latex-workshop.latex.recipes": [
    {
        "name": "XeLaTeX",
        "tools": [
            "xelatexmk"
        ]
    },
    {
        "name": "PdfLaTeX",
        "tools": [
            "pdflatexmk"
        ]
    }
    ],

    "latex-workshop.latex.tools": [
        {
            "name": "xelatexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-pdfxe",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-outdir=%OUTDIR%",
                "%DOC%"
            ],
            "env": {},
        },
        {

            "name": "pdflatexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-pdf",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-outdir=%OUTDIR%",
                "%DOC%"
            ],
            "env": {},
        }
    ],
    ```
- Above setting use `latexmk` as the default compiler and put the auxiliary files and output pdf into the `build` folder.
- References
    - [latex-vscode-config](https://github.com/shinyypig/latex-vscode-config) by [shinyypig](https://github.com/shinyypig)

## Usage
- (Mac) `cmd + option + J` to jump from current cursor position to pdf location.
- (Mac) `cmd + left click` to jump from pdf location (preview window) to source code.  
- (Mac) `cmd + option + V` to view pdf (command: `Latex Workshop: View LaTeX PDF file`).
- Use command `Latex Workshop: Build with recipe` to build the project. The file to be compiled need to be focused for `Latex Workshop` knowing which file to compile

# Sharp-bits
- New line `\\` shouldn't be used inside `$$`.
- `\\` should be used for newline instead of `\newline` in algorithm or tabular environment.
    - `\\` works well for algorithm and tabular environment.
    - `\newline` creates a paragraph like break.
- Use `~\cite{}` instead of `\cite{}`.
    - `~` is non-breaking space
    - Academic writing format requirement. To prevent the citation breaks with previous word and starts in a new line.

# Usage
- Use `\( ... \)` (`\[ ... \]`)  instead of `$ .. $` (`$$ ... $$`).
  -  Modern LaTeX choice.

## Define command
```latex
\newcommand{\commandname}[num_args][default]{definition}
```
- `default` provides default value for the first argument. `\newcommand` only allows providing default value for the first argument.

## Strike out
- `\sout{}` can only be used to strike out text (inline math is OK). To strike out math equations, we need to manually draw the strike out line.
- One example is given below:
    ```latex
    % import package
    \usepackage{tikz}
    \usetikzlibrary{shapes,arrows,positioning,calc, tikzmark}

    % ---------------------------------------- 
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
- When strike out text include `\cite{}`, enclose it with `\mbox` like `\mbox{\cite{}}` to avoid error.

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
