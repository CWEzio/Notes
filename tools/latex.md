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

