
# Git commands

## Basic Git Settings

* Set name for all projects: `git config --global user.name <username>`
* Set email for all projects: `git config --global user.email <email>`
* Set name for current project: `git config --local user.name <username>` or `git config user.name <username>`
* Set email for current project: `git config --local user.email <email>` or  `git config user.email <email>`

---------

## Create a Repository 

* Initialize a project and create a repo: `git init <project name>`
* Add remote address: `git remote add <name> <url>`
* Get a copy from remote repo: `git clone <url>`
  
  ### Git clone with a token
  In some cases (like gitlab) you can clone a repo with the token which is defined in your profile.
  To clone, you can use this command:
```bash
git clone https://oauth2:your-token-in-profile@gitlab.com/username/repo-name
```
  ### Access with ssh key
  * To create ssh key: `ssh-keygen -t ed25519 -C "email@gmail.com"`
  * If you have id_ed25519 file before, you can use: `ssh-keygen -t ed25519 -C "email@gmail.com" -f ~/.ssh/id_ed25519_new`
  * If you want to manage several ssh key for several projects, you must modify `~/.ssh/config` like this:
```bash
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_github

Host gitlab.com
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/id_ed25519_gitlab

Host gitea.com
    HostName gitea.com
    User git
    IdentityFile ~/.ssh/id_ed25519_gitea

```

  * To test that to have correct ssh settings:
```bash
ssh -T git@github.com
ssh -T git@gitlab.com
ssh -T git@gitea.com
```

  * If everything is correct, you can get welcome message with each service
  * **Note about Gitlab**
    * If you have problem to access to gitlab from Iran, you must have extra setting in `config` file to force ssh access from your proxy setting
    * You need to use `ProxyCommand` so before using it, you need to be sure of installing `netcat` or `netcat-openbsd` in your system
    * If you need to install it: `sudo apt install netcat-openbsd`
    * Then you need to change your `~/.ssh/config` file (suppose that you have run SOCKS5 in 1086 port):
```bash
Host gitlab.com
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/id_ed25519_gitlab
    ProxyCommand nc -x 127.0.0.1:1086 %h %p
```
    * If you need, you can add port to ssh like this:
```bash
Host gitlab.com
    HostName gitlab.com
    User git
    Port 443
    IdentityFile ~/.ssh/id_ed25519_gitlab
    ProxyCommand nc -x 127.0.0.1:1086 %h %p
```
    * After this configuration, you need to check your ssh connection with: `ssh -T git@gitlab.com`
    * If you have problem, you can active verbose for git: `GIT_SSH_COMMAND="ssh -v" git clone git@gitlab.com:username/repo-name.git`
    * You can also check it with more level in verbose: `ssh -vT git@gitlab.com` or `ssh -vvvT git@gitlab.com`
  * 

---------

## Fork, modify and pull request from upstream repo
* Add a fork from `upstream-repo-name` repository
* Before cloning your fork, you can use this command to save your username & password:
```bash
git config --global credential.helper store
```

* Clone your fork in your local system.
```bash
git clone https://github.com/your-username-in-github/upstream-repo-name
```

* To see all branches: `git branch -a`
* To create new branch in your repo: 
```bash
git checkout -b <username/feature/task-name>

#e.g.
git checkout -b mehdi/basket/add-item
```
* After adding your code, you must add it to stage: `git add .`
* Then you must commit your changes: `git commit -m "Your Explanation About Your Changes"`
* Push your changes to your fork in remote repository (for first time, maybe it wants to get your username and password):
```bash
git push origin <username/feature/task-name>

#e.g.
git push origin mehdi/basket/add-item
```
* Now you can see a message to suggest you to send a `Pull Request` in your forked repository
* Put your comments and send your pull request


  ### Syncing with main repository

If main repository updated, you need to update your fork. You can do these steps in the following:

* Add main repository as a remote upstream 
```bash
git remote add upstream https://github.com/username/upstream-repo-name.git
```

* Then you need to fetch all changes from upstream: `git fetch upstream`
* You need switch to your main branch (or whatever branch you want) : `git checkout main`
* Now you must merge upstream with your branch: `git merge upstream/main`
  * **Note**: Sometimes when you want to merge, you will face with conflict. So you need to solve it and then add new commit before merging
* Now you can send your changes to your forked remote repository: `git push origin main`

--------

## Daily works in git

* Show file changes with last commit: `git status`
* Add a file to stage: `git add <file_name>`
* Add all changed files to stage: `git add .` or `git add -A`
* Show difference of a file with last stage: `git diff <file_name>`
* Show difference between staged files and last commit: `git diff --staged <file_name>`
* Show difference between files and last commit: `git diff HEAD`
* Add new commit: `git commit -m "message"`
* Add all tracked files and commit them: `git commit -a -m "message"`
* Remove a file from repository: `git rm <file_name>`
* Save changes without adding new commit: `git stash`
* Restore the last stash: `git stash apply`
* List all of stashes: `git stash list`
* Restore the specific stash: `git stash apply <NAME>` 
* Restore all stashes also with files in .gitignore files: `git stash --all`
* To reset all change back to HEAD: `git reset --hard HEAD`
* To reset changes back to 1 commit before HEAD: `git reset --hard HEAD~1`

---------

## Working with branches

* Show all branches in repository: `git branch`
* Show all branches in repository with remotes: `git branch -a`
* Create a new branch: `git branch <branch_name>`
* To create a new branch with starting specific commit id: `git branch <branch_name> <commit_id>`
* Checkout to other branch: `git checkout <branch_name>`
* Create and checkout to new branch: `git checkout -b <branch_name>`
* Merge with current branch: `git merge <branch_name>`
* Rebase with current branch: `git rebase <branch_name>`
* Remove a branch: `git branch -d <branch_name>`
* Show just the current branch_name you ar on: `git rev-parse --abbrev-ref HEAD` or `git branch --show-current`

  ###  Divergent branches
  The message you received means that your local branch and the remote main branch have diverged. Both branches have changes, and they’ve moved in different directions, so Git requires you to specify how to reconcile them. Git provides three options for handling this situation, each representing a different approach to merging or synchronizing the changes. Here’s a breakdown of each option

  1. **Merge**

    Git moves your changes to the top of the main branch’s changes, reorganizing them as if your changes happened after the main branch’s latest changes. This results in a cleaner, more linear history.
    
    **Pros**:

   * The Git history is cleaner and more linear, making it easier to read.
   * There are no additional merge commits, which can simplify history in large projects.
    
    **Cons**:

    * Rebase can modify and rewrite your commits, which may not be ideal in certain projects or for some developers.
    * If multiple developers are working on the same branch, rebase may lead to more conflicts. You have many ways to do it

```bash 
# suppose you are in dev branch and want to pull main branch
# with applying rebase config for project locally
git config pull.rebase false
git pull origin main

# with applying rebase config for project globally
git config --global pull.rebase false
git pull origin main

# if you want to unset config after these commands:
git config --unset pull.rebase
git config --global --unset pull.rebase

# Merge without set config for rebase
git fetch origin main
git merge origin/main

# Merge with setting the flag
git pull origin main --no-rebase

```

   2. **Rebase**
   
   This command configures Git to use rebase instead of merge. With rebase, Git moves your changes to the top of the main branch’s changes, reorganizing them as if your changes happened after the main branch’s latest changes. This results in a cleaner, more linear history.
    
  **Pros**:

 * The Git history is cleaner and more linear, making it easier to read.
 * There are no additional merge commits, which can simplify history in large projects.
  
  **Cons**:

  * Rebase can modify and rewrite your commits, which may not be ideal in certain projects or for some developers.
  * If multiple developers are working on the same branch, rebase may lead to more conflicts.

```bash 
# suppose you are in dev branch and want to pull main branch
# with applying rebase config for project locally
git config pull.rebase true
git pull origin main

# with applying rebase config for project globally
git config --global pull.rebase true
git pull origin main

# if you want to unset config after these commands:
git config --unset pull.rebase
git config --global --unset pull.rebase

# Merge without set config for rebase
git fetch origin main
git rebase origin/main

# Merge with setting the flag
git pull origin main --rebase

```
   3. **Fast-Forward Only**

This command configures Git to pull only if it can perform a fast-forward merge. This means that if your branch is ahead of the main branch, Git will not allow the pull and will require you to handle the changes manually.

**Pros**:

  * Git history remains clean and simple, only allowing pulls when changes are linear.
  * This method doesn’t add any extra commits (like merge or rebase commits).

**Cons**:

  * If both branches have simultaneous changes, this method is not effective, as the pull will not be possible.

```bash 
# suppose you are in dev branch and want to pull main branch
# with applying rebase config for project locally
git config pull.ff only
git pull origin main

# with applying rebase config for project globally
git config --global pull.ff only
git pull origin main

# Merge without set config for fast-forwarding
git fetch origin main
git merge --ff-only origin/main

# Merge with setting the flag
git pull origin main --ff-only

```

  **Comparison and Recommendation**

  - If you prefer a clean and linear history to avoid clutter, Rebase is a good choice, especially in larger projects with many branches.
  - If you want a clear history where all changes are shown without rewriting any commits, Merge is often preferable.
  - If you want to synchronize only when the changes are linear, Fast-Forward Only is an option, though it’s less common in multi-branch projects.
  - For general use, especially if you want to keep both sets of changes intact, merge is typically recommended (git config pull.rebase false).

---------

## Communicate with remote repository

* Get & see changes on remote server without merging them with local project: `git fetch <remote_name> <branch_name>` then `git checkout <branch_name>`

  e.g. `git fetch origin main`

* To see the state of your branch with fetched branches, you can use: `git status`
* To merge all of changes with your branch (after `git fetch`): `git merge`
* The command of `git pull` is a combination of git fetch and git merge
* Get changes on remote server and merge with local server for all branch: `git pull --all`
* Get changes on remote server and merge with local server for a branch: `git pull origin <branch_name>` or `git checkout <branch_name >` then `git pull`
* Send your commits to remote server for specific branch: `git push <remote_name> <branch_name>`
* Fast-forward merge in the destination repository: `git push <remote_name> --force`
* Push all of your local branches to the specified remote: `git push <remote_name> --all`
* Sends all of your local tags to the remote repository (Tags are not automatically pushed when you push a branch or use the --all option): `git push <remote_name> --tags`
* Send new branch to remote server: `git push -u <remote_name> <branch_name>`


---------

## Show and report commit history & customize output

* Show list of all commits: `git log`
* Show the last m commits: `git log -n <m>`
* Show commits in one line and with graph: `git log --oneline --graph --decorate`
* Show all of commits for an author: `git log --author=<username>`
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
* [A Visual and Practical Guide to Git](https://www.freecodecamp.org/news/gitting-things-done-book)
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








