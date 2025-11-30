The `forest switch` command allows you to change your current branch by selecting one among the existing branches in your Development Environment:

```
$ forest switch --help
Switch to another branch in your local development environment.

USAGE
  $ forest switch [BRANCH_NAME]

ARGUMENTS
  BRANCH_NAME  The name of the local branch to set as current.

OPTIONS
  --help  Display usage information.
```

#### Branch selection

You can switch directly to a specific branch by using `forest branch <my_branch_name>`.

Omitting the branch name will allow you to easily select one:

```
$ forest switch
[? Select the branch you want to set-current:
feature/add-new-smart-view-with-information
hotfix/fix-dropdown-issue
feature/implement-refund-smart-action
```
