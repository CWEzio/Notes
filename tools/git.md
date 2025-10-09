# Table of content
- [Table of content](#table-of-content)
- [View](#view)
  - [view commit](#view-commit)
  - [View the change history of a specific file](#view-the-change-history-of-a-specific-file)
- [Commit](#commit)
  - [Squash last `n` commits](#squash-last-n-commits)
  - [Untrack files](#untrack-files)
- [Branch](#branch)
  - [Merge branches](#merge-branches)
  - [Delete a remote branch](#delete-a-remote-branch)
- [Submodule management](#submodule-management)
- [Remote management](#remote-management)
- [Git message](#git-message)
- [Other things](#other-things)
  - [Use github issues to manage task lists](#use-github-issues-to-manage-task-lists)
  - [Move the git repository one level up in the file hierarchy](#move-the-git-repository-one-level-up-in-the-file-hierarchy)
- [Problems](#problems)
  - [`git` overwrites my local file](#git-overwrites-my-local-file)


#  View
## view commit
- `git show` to show the most recent commit
- `git show [commit-hash]` to view a specific commit 
- Additional options for `git show`
    - `--stat`: show the number of changes.
    - `--name-only`: only show the name of the changed files.
    - `--oneline`: show the commit in one line.
    - `--name-status`: show the name of files changed along with the status (added, modified, deleted).
## View the change history of a specific file
- `git log [path-to-file]`
- Specific options:
  - `-p` or `--patch`: show the difference introduced in each commit
  - `--stat`: display the number of changes made to the file along with each commit
  - `--oneline`
  - `--graph`
  - `-n <limit>`: limits the number of commits shown.

# Commit 
- `git commit --amend` amend last commit with new change
## Squash last `n` commits
1. `git rebase -i HEAD~n`, where `n` is the number of last commits that you want to include in the rebase. This will start an interactive rebase.
2. In the text editor opened, you'll see a list of commits. To squash commits, leave the first commit as `pick` and change the word `pick` to `squash` (or just `s` for short) for the commits that you want to squash into the first commit. Save and close the editor. 
3. Git will then prompt you to merge the commit messages. Edit the messages. Save and close the editor.
4. If needed, use `git push origin your-branch-name --force` to also update the origin.

## Untrack files
Remove file/folder from the git index but keep those files, in other words, want to untrack file or folder

This can be done with 
```
git rm -r --cached <folder>
```
- `-r` is for recursive removal\
- `--cached` is for removing the folder from the index, but keep the files themself untouched.

Refer to [documentation](https://git-scm.com/docs/git-rm) for more information regarding `rm`.

# Branch 
- `git branch` list branches.
- `git branch <name>` create a banch with name `<name>`.
- `git branch -m <newname>` rename the current branch.
- `git branch -M <newname>` rename the current branch but in a more forceful way. The brancch will be renamed, even if a branch with name `<newname>` already exists.
- `git fetch origin <branch-name>` fetch the branch `<branch-name>` from the origin.
- Add proper protection rule in github to protect the branches, especially in project with collaboration.
## Merge branches
Suppose you are merging `feature` branch into the `main` branch
1. Check out to the branch you want to merge into
    ```
    git checkout main
    ```
2. Merge the target branch into the current branch
    ```
    get merge feature
    ```

## Delete a remote branch
1. Check whether this branch contains a commit that is not in any other branches
    ```
    git log origin/old-branch-name --not --remotes --exclude=origin/old-branch-name
    ```
2. Back up the branch to be deleted with a tag
    ```
    git tag backup/old-branch-name origin/old-branch-name
    ```
3. Push this tag into remote
    ```
    git push origin backup/old-branch-name
    ```
4. Delete the remote branch
    ```
    git push origin --delete old-branch-name
    ```
> Find which branch contains a certain commit with
> ```
> git branch --contains commit-id
> ```

# Submodule management
- Clone and initialize all the submodules of a repo:
  ```
    git clone --recurse-submodules <repository-url>
  ```
- Initialize all the submodules after clone:
    ```
    git submodule update --init --recursive
    ```
- `git add <path-to-submodule>` to stage changes in the submodule.

# Remote management
- View current origin `git remote -v`, where `-v` stands for verbose.
- Remove current origin `git remote remove origin`
- Add origin `git remote add origin git@github.com:some_repo.git`

# Git message
TODO: finish this section when I have time  (how to write a git message)


# Other things
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

# Problems
## `git` overwrites my local file
My project contains a folder called `exp_data`, which is not tracked by git (adding `exp_data/` in `.gitignore`). A colleague collaborating on this project create a symbolic link `exp_data` that refers to another directory. Because the symbolic link is a file not a directory, it is not ignored. The `exp_data` symbolic link is accidentally staged and pushed to the remote. And after I merged this change, I find that my local `exp_data` folder was overwrote and cannot be restored. I do not use force pull, but there is no warning or error information when I do the merge.

I believe this is a bug of git, because git shouldn't delete untracked files without warning, but there are also some good practices from our side that can prevent this from happening:
1. Do not use symbolic link under a git repo.
2. Use `name` instead of `name/` when adding a folder to `.gitignore`.


