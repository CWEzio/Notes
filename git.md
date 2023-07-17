# My note of git usage

## Remove file/folder from the git index but keep those files, in other words, want to untrack file or folder
This can be done with 
```
git rm -r --cached <folder>
```
>`-r` is for recursive removal\
>`--cached` is for removing the folder from the index, but keep the files themself untouched.

Refer to [documentation](https://git-scm.com/docs/git-rm) for more information regarding `rm`.

## Use github issues to manage task lists
1. In github issues, you can have a tasks list. The markdown syntax is simple.
    ```markdown
    - [ ] Mercury 
    - [x] Venus
    ```
    > Here `- [ ]` represents an uncompleted task and `- [x]` represents a completed task

    For more details, check this [blog](https://github.blog/2014-04-28-task-lists-in-all-markdown-documents/) and this [official documentation](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-task-lists)

2. You can also mention issues in the tasks list. For more detail, also check the [official documentation](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-task-lists).

3. You can link a commit/(pull request) to a github issue by simply mention the issue number `#xxx` in the commit/(pull request) message.
You can also close an issue with commit/(pull request) by containing keywords like `fix #xxx`/`fixes #xxx`/`close #xxx` in the commit/(pull request) message.
    
    For more details, check this [answer](https://www.edureka.co/community/102139/link-to-the-issue-number-on-github-within-a-commit-message#:~:text=You%20just%20need%20to%20include,(in%20your%20commit%20message).) or this [official documentation](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue)

## Move the git repository one level up in the file hierarchy
Suppose I have a repo
```
<project repo>
    src/
    CMakeLists.txt
    third_party/
    ...
```
By mistake, I have made the `src/` directory a git repo. However, I want to change the git repo to the whole project repo. There is no direct way to do this. Here is a workaround way:
1.  Create a subdirectory `source_files` and move all the files (except the .git) to this subdirectory
     ```
    cd src
    mkdir source_files
    git mv * source_files
    git commit -all -m "moved all existing files to new source_files file"
    ```

2. Move the created subdirectory `source_files` and the `.git` under the `<project repo>`. After that, `src` should contains no file and removing it is safe.
    ```
    cd <project repo>
    mv src/* .
    mv src/.git .
    trash src
    mv source_files src
    ```

3. commit the change 
    ```
    git add .
    git commit -m "include the whole repo's file"
    ```
