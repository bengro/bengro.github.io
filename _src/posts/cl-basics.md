# Command line basics

This document contains commands and other random command line one-liners, which I collected during my first year at Pivotal Labs. Some of the notes may be lacking context or may be obvious. They were once not obvious to me. They might serve as a starting point for further investigation for the curious reader.

## Git
* `git diff --staged`
* `git diff SHA1 SHA1 <file>` to see the diff of two commits
* `git log --graph --all --decorate --oneline` nicely show commits
* `git log --follow <filename>` follows file renames
* `git stash` (stash files and revert files)
* `git stash show` (show details of stash)
* `git stash list` (list all stashes), git stash apply (get it back)
* `git show HEAD` (show explicit changes)
* `git show -p` (list files and show line changes)
* `git show sha:file` shows the file of a given commit
* `.git/config` specifies repository wide git configuration
* `~/.gitconfig` specifies system wide git configuration
* Define git aliases in `~/.gitconfig`
* `git reset --soft <sha>` goes back to commit and stages new and modified files
* `git reset --mixed <sha>` goes back to commit and does not stage new and modified files
* `git reset --hard <sha>` goes back to commit and deletes the previous changes.
* .gitignore with * content, then use `git add -f` is a restrictive repository pattern
* `git reflog`: any commit can be found, keeps track of local git history
* `git merge --ff-only` (merges only if there is no merge conflict, aborts if so)
* `git pull` is a shortcut for `git fetch && git merge`
* "git ref" (for reference) can be a branch, sha or tag
* git change author: `git commit --amend --author / --reset-author`
* Change order of commits, squash commits together: `git rebase -i HEAD~3`
* `git revert SHA` (put commit with SHA on top of branch)
* git caret vs tilde: similar but ~ refers to commits first parent. a commit can have two parents.
* `git clean -fxd` (cleans all files not tracked by git)
* `git clean --dry-run` to tell you what files git would remove when running git clean
* `man git-add` gives information about git add
* `git --amend -C HEAD` does not prompt user for refining git message
* `git blame <file>` lists all the lines and shows in which commit they were modified
* "fast-forward is a pull without merge"
* "rebase is a stash and a pull"
