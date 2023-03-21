# GIT

### Commands:
```bash
$ git diff 
$ git diff <file>
# compare un-staged changes with staged

$ git diff HEAD
$ git diff HEAD <file>
# compare un-staged changes with local repo

$ git diff --staged HEAD
$ git diff --staged HEAD <file>
# compare staged changes with local repo

$ git diff <commit1_id> <commit2_id>
# compare two commits

$ git diff HEAD HEAD^
# compare last two commits

$ git log
$ git log --oneline --graph --decorate
$ git log --since="2 days ago"
# see commits

$ git log -- <file>
# see commits made to a particular file

$ git show <commit_id>
# get details of a commit

$ git rm <file>
# delete file

$ git reset HEAD <file>
# un-stage a file (before commit)

$ git checkout -- <file>
# revert (un-staged) changes

$ git reset HEAD~
# revert last commit

$ git mv <file> <new_file>
# re-name a file

$ git config --global alias.<short_form> "<full_command>"
# alias = create short forms of git commands
# don't include the word 'git' while specifying full command

$ nano ~/.gitconfig
# view global git config

$ git branch -d <branch_name>
# delete a branch
```

### Rebase:  
Similar to merge but diferent.  
Rebase does not make an explicit merge commit.  
Rather, it combines the history of 2 branches.  
All commits on feature branches are put at the tip of the master branch, upon rebase.  
This results in a much cleaner and linear project.   

```bash
$ git checkout <target_branch eg: feature branch>
$ git rebase <source_branch eg: master>

$ git checkout <target_branch eg: master>
$ git rebase <source_branch eg: feature branch>
# syntax difference merge vs rebase:

$ git rebase --abort
# if there are conflicts in git rebase

$ git rebase --continue
# after resolve conflicts

$ git pull --rebase origin master
# rebasing local repo with remote

$ git stash 
# stash changes
$ git stash list 
# see all stashed changes
$ git stash apply 
# bring back all stashed changes
$ git stash drop
# drop all stashed changes after applying them
```

By default git will not stash changes made to new files that have not yet been staged, and files that have been ignored
```bash
$ git stash -u
# stash untracked files

$ git stashed -a
# stash all files, even ignored files

$ git stash pop
# apply the last stash and drop it automatically

$ git stash save "some message"
# stash with some message

$ git stash list
$ git stash show stash@{<index>}
# shows changes made at a particular stash
# index can be 0,1,2...

$ git stash apply stash@{<index>}
$ git stash drop stash@{<index>}
# apply and drop a particular stash

$ git stash clear
# delete all stashed changes

$ git stash branch <branch_name>
# stash changes to a new branch

$ git tag <tag_name>
# add a tag

$ git tag --list
# see all tags

$ git tag --delete <tag_name>
# delete a tag

$ git tag -a <tag_name>
# create annotate tag

$ git show <tag_name>
# see details of annotated tag (author, message, etc) 

$ git diff <tag_1> <tag_2>
# compare two tags

$ git tag -a <tag_name> <commit_id>
# tag a specific commit

$ git tag -a <tag_name> -f <comiit_id>
# update a particular tag to a different branch
```

Merge without commit:
```bash
$ git checkout <target>
$ git merge <source> --no-ff --no-commit
$ git restore <files>
$ git add .
$ cit commit -m '<message>'
$ git push origin <target>
```