---
layout: post
title:  "Common Git Commands"
date:   2018-09-23 16:05:03 +0800
categories: jekyll update
---
## Installation
- Windows: Download from <https://git-scm.com/>
- Linux:

```
$ sudo apt-get update
$ sudo apt-get install git
```
#### Verify
```
$ which git                          # => /mingw64/bin/git
$ git --version                      # => git version 2.18.0.windows.1
```
## Config
#### Edit 
```
$ git config --global <name> <value> # Add a setting
$ git config --global --unset <name> # Remove a setting
$ git config --global -e             # Use Editor to edit
$ git config --global -l             # List all the config settings
```
> The global config is store in `~/.gitconfig`

#### Essential
```
$ git config --global user.name "YOUR NAME"
$ git config --global user.email "YOUR@EMAIL.com"
```
#### Editor
```
$ git config --global core.editor vim
$ git config --global core.editor emacs
```
#### Alias 
```
$ git config --global alias.st status
$ git config --global alias.l "log --all --graph --oneline"
$ git config --global alias.st status
$ git config --global alias.co checkout
$ git config --global alias.br "branch --all --verbose"
```

## Create project
```
$ git clone <repository>             # Clone a project from remote repository
$ git clone <repository> <directory> # with new directory name
$ git init                           # create an empty project in current directory
$ git init <directory>               # create an empty project
```
## Git workflow
1. Working Directory: saved files
2. Staging Area(Index,Cache): preparing files to be commited
3. Repository: commited files in Git history
4. Github or GitLab: remote Git repository


## Git Command
#### Representation
- `A` and `B` are one of `1`,`2`,`3`,`4` represent the four states in git workflow.
- `A` ← `B` means set `A` as `B`, the origin value of A will be covered
- `A` → `B` means append `A` to `B`
- `A`↑ means makes `A` move forward
- `<pathspec>` Most of time it's just a file name. Simply just treat it like using regular expression to select files.
- `<tree-ish>` It's use to specify the version which starts form `Reference` (`HEAD`, `Tag`, `Branch`, `Remote`) or `Hash` and might follow by mutiple `^`, `~`, `@{}`(`Reference` only). There are several examples below.

<table>
<tr><th>notition</th><th>description</th></tr>
<tr><td>head^</td><td>1st parent of HEAD</td></tr>
<tr><td>head^1</td><td>1st parent of HEAD</td></tr>
<tr><td>head^2</td><td>2nd parent of HEAD</td></tr>
<tr><td>head^3</td><td>3rd parent of HEAD</td></tr>
<tr><td>head~</td><td>1st parent of HEAD</td></tr>
<tr><td>head~1</td><td>1st parent of HEAD</td></tr>
<tr><td>head~2</td><td>1st parent of 1st parent of HEAD</td></tr>
<tr><td>head^^</td><td>1st parent of 1st parent of HEAD</td></tr>
<tr><td>head@{0}</td><td>current HEAD</td></tr>
<tr><td>head@{1}</td><td>previous HEAD</td></tr>
<tr><td>head@{2}</td><td>previous of previous HEAD</td></tr>
<tr><td>head@{1}~2^3~1</td><td> 1st parent of 3rd parent of 1st parent of 1st parent of previous HEAD</td></tr>
</table>

> Check [gitglossary](https://git-scm.com/docs/gitglossary) for more information.

#### Add
- `.gitignore`: Not all the file in the project need to be tracked.

>For more `.gitignore` examples, see [here](https://github.com/github/gitignore).

- `.gitkeep` and `.keep`: Git doesn't track empty directories, so some project will add `.gitkeep` or `.keep` file.

```
$ git add --all
$ git add <pathspec>                 # 2 ← 1
$ git add <pathspec> -p              # 2 ← 1, partial add
```
#### Commit
```
$ git commit                      # current-branch↑, 2 → 3
$ git commit -m <message>         # current-branch↑, 2 → 3
$ git commit -a                   # add tracked file, commit
$ git commit <file1> <file2> ...  # current-branch↑, 2 → 3, partial commit
$ git commit --amend              # 3 ← 2
$ git revert <tree-ish>           # add a commit that revert the changes of <tree-ish>
```
#### Inspect
```
$ git status                         # show staged, unstaged, untracked files
$ git reflog                         # Show ref log
$ git show <object>                  # master (Branch)
                                     # master:public/javascript (Tree)
                                     # master:public/javascript/chat.js (Blob)
$ git ls-files                       # Show tracked files
$ git diff                           # diff 1 2
$ git diff --cache                   # diff 2 3
$ git diff <tree-ish> <tree-ish'>    # diff between tree-ish
$ git diff <blob> <blob>             # diff between different version of file
$ git blame <file>                   # Who write this code !?
$ git log
$ git log -<number>
$ git log --oneline
$ git log --graph
$ git log --stat                     # Generate a diffstat
$ git log -p                         # log with patch
$ git log <paths>                    # only commit that modified <paths>
$ git log -S <string>                # e.g. -S "app.use('/index', index);"
$ git log --author=<pattern>         # --author=Andy
                                     # --author="Andy\|Eric"
$ git log --grep=<pattern>           # --grep "bugfix"
$ git log --since=<date>             # --since="Mon Sep 10 21:41:11 2018 +0800">
$ git log --after=<date>             # --after="yesterday"
          --until=<date>             # --until="1 week ago"
          --before=<date>            # --before="1/1/2010"
                                     # --before="10-11-1998"
                                     # --before="1 month 2 days ago"
                                     # --before="six minutes ago"
                                     # --before="last Tuesday"
                                     # --before="5pm"
                                     # --before="12am"
                                     # --before="2017-01"
```
#### Branch
```
$ git branch <new-branch>              # Create new branch
$ git branch <new-branch> <tree-ish>   # Create new branch on start at tree-ish
$ git branch -m <branch>               # Change branch name
$ git branch                           # Show local branch
$ git branch -r                        # Show remote branch
$ git branch -a                        # Show all branch
$ git branch -v                        # Verbose mode, More infomation
             -vv                       # Show upstream
$ git branch -d                        # Delete branch
$ git branch -D                        # Delete not merged branch
```
#### Checkout
```
$ git checkout <pathspec>                   # 1 ← 2
$ git checkout <tree-ish> <pathspec>        # 1 ←(2 ← 3)
$ git checkout <tree-ish>                   # 1 ←(2 ← 3) (modified files are kept)
$ git checkout -b <new-branch>              # Create at here and checkout
$ git checkout -b <new-branch> <tree-ish>   # Create at tree-ish and checkout
```
#### Reset
```
$ git reset <paths>                  # 2 ← 3 (opposite of add)
$ git reset <tree-ish> <paths>       # 2 ← 3
$ git reset <tree-ish> --soft        # current-branch ← tree-ish
$ git reset <tree-ish>               # current-branch ← tree-ish, 2 ← 3
$ git reset <tree-ish> --mixed       # current-branch ← tree-ish, 2 ← 3
$ git reset <tree-ish> --hard        # current-branch ← tree-ish, 1 ←(2 ← 3)
```
#### Remove and Rename
```
$ git clean                          # Remove untracked files (need -f by default)
$ git clean -n                       # Preview what would remove
$ git clean -i                       # Interactive clean
$ git rm <file>                      # rm (1, 2)
$ git rm <file> --cached             # rm 2
$ git mv <file> <file'>              # mv (1, 2)
```
#### Merge
```
$ git merge <tree-ish>               # current↑
$ git rebase <tree-ish>              # current ← tree-ish, current↑...
$ git rebase <tree-ish> -i           # interactive
$ git reset ORIG_HEAD                # cancel merge or rebase
$ git cherry-pick <tree-ish>...      # current↑...
```
#### Stash
```
$ git stash                      # (1, 2) → stash
$ git stash -u                   # (1, 2) → stash, include untracked
$ git stash push                 # (1, 2) → stash            
$ git stash apply                # (1, 2) ← stash
$ git stash drop                 # delete stash
$ git stash pop                  # apply and drop
```
#### Remote
```
$ git remote -v                      # Show remote
$ git remote add <name> <url>        # Add remote
$ git remote remove <name>           # Remove remote
$ git remote update                  # 4 → 3, all remote
$ git fetch --all                    # 4 → 3, all remote
$ git fetch                          # 4 → 3, only update current branch
$ git fetch <remote>                 # 4 → 3
$ git pull                           # fetch and merge
$ git pull --rebase                  # fetch and Rebase
$ git push <remote> <branch>         # 3 → 4
$ git push -u <remote> <branch>      # 3 → 4, set upstream to push without argument
$ git push                           # 3 → 4
$ git push origin :<branch>          # delete remote branch
```
#### Tag
```
$ git tag                            # Show tag
$ git tag <new-tag> <tree-ish>       # Create tag
$ git tag -d <tag>                   # Delete tag
$ git push origin --tags             # Push tags
```
#### Pack
```
git archive -o latest.zip <tree-ish>
```
## References
1. [Git \- Reference](https://git-scm.com/docs)
1. [Git 教學 \- Git 書 \- 為你自己學 Git \| 高見龍](https://gitbook.tw/)
1. [[Git] Reset \- mixed, hard and soft \| 搞搞就懂 \- 點部落](https://dotblogs.com.tw/wasichris/2016/04/29/225157)
1. [github初學者 使用筆記 ~ Return to laughter](https://self.jxtsai.info/2015/11/github.html)
1. [Git 初學筆記 \- 指令操作教學 \| Tsung's Blog](https://blog.longwin.com.tw/2009/05/git-learn-initial-command-2009/)
1. [Git 初學筆記 \- 實作測試 \| Tsung's Blog](https://blog.longwin.com.tw/2009/05/git-learn-test-command-2009/)
1. [5.2 代碼回滾：Reset、Checkout、Revert 的選擇](https://github.com/geeeeeeeeek/git-recipes/wiki/5.2-%E4%BB%A3%E7%A0%81%E5%9B%9E%E6%BB%9A%EF%BC%9AReset%E3%80%81Checkout%E3%80%81Revert-%E7%9A%84%E9%80%89%E6%8B%A9)
1. [Working with dates in Git \- Alex Peattie](https://alexpeattie.com/blog/working-with-dates-in-git)
1. [Git指令整理 \- 柏熒的博客 \| BY Blog](http://qiubaiying.top/2017/02/15/Git%E6%8C%87%E4%BB%A4%E6%95%B4%E7%90%86/)
1. [git archive \- What does tree-ish mean in Git? \- Stack Overflow](https://stackoverflow.com/questions/4044368/what-does-tree-ish-mean-in-git)
