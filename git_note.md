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