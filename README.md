
# Git commands

## Basic Git Settings

* Set name for all projects: `git config --global user.name <username>`
* Set email for all projects: `git config --global user.email <email>`
* Set name for current project: `git config --local user.name <username>`
* Set email for all projects: `git config --local user.email <email>`

---------

## Create a Repository 

* Initialize a project and create a repo: `git init <project name>`
* Add remote address: `git remote add <name> <url>`
* Get a copy from remote repo: `git clone <url>`

--------

## Daily works in git

* Show file changes with last commit: `git status`
* Add a file to stage: `git add <file name>`
* Add all changed files to stage: `git add .` or `git add -A`
* Show difference of a file with last stage: `git diff <file name>`
* Show difference between staged files and last commit: `git diff --staged <file name>`
* Show difference between files and last commit: `git diff HEAD`
* Add new commit: `git commit -m "message"`
* Add all tracked files and commit them: `git commit -a -m "message"`
* Remove a file from repository: `git rm <file name>`
* Save changes without adding new commit: `git stash`
* Restore the last stash: `git stash apply`
* List all of stashes: `git stash list`
* Restore the specific stash: `git stash apply <NAME>` 
* Restore all stashes also with files in .gitignore files: `git stash --all`

---------

## Working with branches

* Show all branches in repository: `git branch`
* Show all branches in repository with remotes: `git branch -a`
* Create a new branch: `git branch <branch name>`
* Checkout to other branch: `git checkout <branch name>`
* Create and checkout to new branch: `git checkout -b <branch name>`
* Merge with current branch: `git merge <branch name>`
* Rebase with current branch: `git rebase <branch name>`
* Remove a branch: `git branch -d <branch name>`
* Show just the current branch name you ar on: `git rev-parse --abbrev-ref HEAD` or `git branch --show-current`


---------

## Communicate with remote repository

* Get & see changes on remote server without merging them with local project: `git fetch <remote name> <branch name>` then `git checkout <branch name>`

  e.g. `git fetch origin main`

* The command of `git pull` is a combination of git fetch and git merge
* Get changes on remote server and merge with local server for all branch: `git pull --all`
* Get changes on remote server and merge with local server for a branch: `git pull origin <branch name>` or `git checkout <branch name >` then `git pull`
* Send your commits to remote server for specific branch: `git push <remote name> <branch name>`
* Fast-forward merge in the destination repository: `git push <remote name> --force`
* Push all of your local branches to the specified remote: `git push <remote name> --all`
* Sends all of your local tags to the remote repository (Tags are not automatically pushed when you push a branch or use the --all option): `git push <remote name> --tags`
* Send new branch to remote server: `git push -u <remote name> <branch name>`


---------

## Show and report commit history & customize output

* Show list of all commits: `git log`
* Show the last m commits: `git log -n <m>`
* Show commits in one line and with graph: `git log --oneline --graph --decorate`
* Show all of commits for an author: `git log --author=<UserName>`
* Show authors with their commit counts: `git shortlog -nse`
* Show colored log for each part of results: 
```
git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --
```
* Create a alias for colored log:
```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --"

```
* Show all commits which have been changed the file: `git log -- <FileName>`


### useful links:
[customizing git history](https://dev.to/robertobutti/git-commit-history-customize-the-output-with-colors-lcc)
[git log cheatsheet](https://www.hatica.io/blog/git-log-cheatsheet/)

---------


## git cheat sheet & useful resources

* [Learn git concept](https://github.com/UnseenWizzard/git_training)
* [Practical Git Guide](https://github.com/sadanandpai/git-guide)
* [Git Cheatsheet (Basics)](https://github.com/0nn0/git-basics-cheatsheet)
* [Git Alias](https://github.com/GitAlias/gitalias)
* [Git Commands](https://github.com/joshnh/Git-Commands)
* [Useful Git Commands](https://github.com/bpassos-zz/git-commands)
* [Git Extra Commands](https://github.com/unixorn/git-extra-commands)
* [Lazy git](https://github.com/jesseduffield/lazygit)
* You can find a Persian git cheat sheet here: [Persian git cheat sheet](https://quera.org/college/cheatsheet/git).

---------

* 








