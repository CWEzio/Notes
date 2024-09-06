# clangd
## Use different `compile_commands.json` files for different subprojects.
### Specify the `compile_commands.json`
- Create the `.clangd` file at the project's root directory.
- Inside the `.clangd` file, include:
    ```yaml
    If:
    PathMatch: cgal_mesh/.*
    CompileFlags:
    CompilationDatabase: cgal_mesh
    ---
    If:
    PathMatch: softgym/.*
    CompileFlags:
    CompilationDatabase: softgym/compile_database   
    ```
- In above example, there are two subprojects, `cgal_mesh` and `softgym`. The `compile_commands.json` files are `cgal_mesh/compile_commands.json` and `softgym/compile_database/compile_commands.json`.
- `cgal_mesh/` is the relative path from the root directory. Also note that relative path like `./cgal_mesh` does not work.
- For some reason I don't know, absolute path also does not work.

### Not specify the `compile_commands.json`
As in [this comments](https://github.com/clangd/clangd/discussions/1504#discussioncomment-4987407), `clangd` search for the `compile_commands.json` file from the closest ancestor directory for each file. Therefore, a project with the following structure does not require manual settings:
```
  project_root
      compile_commands.json      // contains entries for files not specific to a target
      target1
          compile_commands.json  // will be used for files in target1 subdirectory
          ...
      target2
          compile_commands.json  // will be used for files in target2 subdirectory
          ...
```
- Note that `clangd` also search for `build` subdirectories. For more details, check [doc](https://clangd.llvm.org/installation#compile_commandsjson).

# clang-format
## `clang-format` cause include order changes
Recently, I am working with `Pyflex`. I find that the automatic formatting will change the include order, which will introduce bugs. To close this feature, I can create the `.clang-format` and include the following lines in it:
```yaml
BasedOnStyle: Google
IndentWidth: 4
SortIncludes: false  # Disables sorting includes
IncludeBlocks: Preserve  # Prevents reordering of include blocks
```