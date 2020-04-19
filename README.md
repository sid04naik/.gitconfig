# gitconfig-template
gitconfig-template is a sample of git config file (Example .gitconfig)

## Configurations
This configurations can be set globally or it can be set locally within the git project.

### Setting user credentials
```
[user]
	name = <name>
	email = <emailaddress>
	username = <github/gitlab/bitbucket username>
```
### Setting core details like editor and pager config
```
[core]
	editor = nano -w
	pager = less -F -X
```
### Setting colors config
```
[color]
	ui          = true
	pager       = true
	interactive = auto
	grep        = always
	decorate    = always
	showbranch  = always
```
### Setting colors for branch
```
[color "branch"]
	current = green bold
	local   = magenta
	remote  = cyan
```
### Setting colors for diff - (comparing files)
```
[color "diff"]
	meta       = cyan
	frag       = magenta
	old        = red
	new        = green
	whitespace = yellow reverse
```
### Setting colors for status
```
[color "status"]
	added     = green
	changed   = cyan
	untracked = magenta
	deleted   = red dim
	branch    = green bold
```
### Setting diff tools
```
[diff]
	tool = vimdiff
```
### Setting diff tool config
```
[difftool]
	prompt = false
```
### Setting credentials helpers
```
[credential]
    //storing data in MacOS keychain and fetching it from there.
	helper = osxkeychain
	useHttpPath = true
```
### Setting aliases for some important commands.
You can use `[alias]` to set custom aliases.
```
[alias]
```
#### Important settings for getting branch info
- `bc`: Will display current branch name
- `bu`: Will display current branch full symbolic name
```
bc = rev-parse --abbrev-ref HEAD

bu = !git rev-parse --abbrev-ref --symbolic-full-name "@{u}"
```
#### Setting Upstream
```
set-up-stream = !git branch --set-upstream-to=$(git bu)
```
#### Deleting branch locally
```
delete-branch = branch -D
```
#### Displaying branches
```
display-branch = !git branch

display-branch-all = !git branch -a
```
#### Listing git tracked files
```
ls = ls-files
```
#### Switching between branches
```
co-dev = !git checkout dev && git pull origin $(git bc)

co-staging = !git checkout staging && git pull origin $(git bc)

co-master = !git checkout master && git pull origin master
```
#### Aliases for checkout
```
co = checkout

co-branch = checkout -b
```
#### Aliases for cherry-pick
```
cp = cherry-pick

cp-abort = cherry-pick --abort

cp-continue = cherry-pick --continue
```
#### Alias for commits
- `cm-add`: Will add all files and commit it
```
cm = !git commit -m

cm-add = !git add -A && git cm
```
- `cm-edit`: Change the last commit message
```
cm-edit = commit -a --amend
```
- `amend`: Will use the last commit message.
- `amend-all`: Will add all files to last commit
```
amend = !git commit --amend --no-edit

amend-all = !git add -A && git amend
```
#### Fetching the history locally.
```
read = !git fetch -p

read-all = !git fetch -a -p
```
#### Updating branches
- `up`: Updating the branch and rebasing my changes on top of all
  -  If I have any local commits, it’ll rebase them to come after the commits I pulled down.
  -  The --prune option removes remote-tracking branches that no longer exist on the remote.
- `update`: Update the current branch with master branch and rebasing my changes on top of all
- `update-branch`: Update my current local branch with origin.
- `update-master`: Update my current local branch with master branch
```
up = !git pull --rebase --prune $@

update = !git read && git rebase origin/master

update-branch = !git pull origin $(git bc)

update-master = !git pull origin master
```
#### Pushing changes
- `push-master`: Pushing changes to master
- `push-branch`: Pushing changes to origin
```
push-master = push origin master

push-branch = !git push origin $(git bc)
```
#### Submodules
- `update-sm`: Updating all the submodules in the project
- `cm-sm`: Commiting the submodule with predefined commit message.
```
update-sm = !git pull --recurse-submodules && git submodule update --init --recursive

cm-sm = !git cm "SUBMODULE REFERENCE UPDATED"
```
#### Saving Changes
- `save`: Saving work in a commit (including untracked files).
- `wip`: Saving work in a commit (only tracked files).
```
save = !git add -A && git commit -m 'SAVEPOINT'

wip = commit -am "WIPPOINT"
```
#### Resetting
- `undo`: Resets the previous commit, but keeps all the changes from that commit.
- `unstage`: Reset added files for commits.
- `uncommit`: Resets the previous commit, but keeps all the changes from that commit.
- `reset-head`: Resets branch with HEAD
- `reset-branch`: Resets branch with origin
- `wipe`: This commits everything in my working directory and then does a hard reset to remove that commit. -- You can run the `git reflog` command and find the SHA of the commit
```
undo = reset HEAD~1 --mixed

unstage = reset

uncommit = reset --soft HEAD^

reset-head = reset HEAD --hard

reset-branch = reset --hard $(git bu)

wipe = !git add -A && git commit -qm 'WIPE SAVEPOINT' && git reset HEAD~1 --hard
```
#### Alias for merging
```
mg = !git merge

mg-nf = !git merge --no-ff
```
#### Advance logging
- `lg`: Alias for log
- `lg-lite`: Logging commit in one line
- `lg-latest`: Logging the latest commit with detailed info.
- `lg-branch`: Logging the current branch change with detailed info.
- `lg-all`: Logging all commits with detailed info.
- `lg-mychange`: Logging my own commits.

```
	lg = log

	lg-lite = log --oneline --decorate

    lg-latest = log --abbrev-commit --decorate --format=format:'%C(bold red)%h%C(reset) - %C(bold blue)%aD%C(reset) %C(bold green)(%ar)%C(reset) %C(bold yellow)%d%C(reset) %n''%C(dim yellow)%H%C(reset) - %C(white)%s%C(reset) %n''%C(green)-(Committer: %cn <%ce>)%C(reset) %C(dim white)-(Author: %an <%ae>)%C(reset)' -1 HEAD --stat

    lg-branch = log --graph --abbrev-commit --decorate --format=format:'%C(bold red)%h%C(reset) - %C(bold blue)%aD%C(reset) %C(bold green)(%ar)%C(reset) %C(bold yellow)%d%C(reset) %n''%C(dim yellow)%H%C(reset) - %C(white)%s%C(reset) %n''%C(green)-(Committer: %cn <%ce>)%C(reset) %C(dim white)-(Author: %an <%ae>)%C(reset)' HEAD --stat

    lg-all = log --graph --abbrev-commit --decorate --format=format:'%C(bold red)%h%C(reset) - %C(bold blue)%aD%C(reset) %C(bold green)(%ar)%C(reset) %C(bold yellow)%d%C(reset) %n''%C(dim yellow)%H%C(reset) - %C(white)%s%C(reset) %n''%C(green)-(Committer: %cn <%ce>)%C(reset) %C(dim white)-(Author: %an <%ae>)%C(reset)' --all --stat

    lg-mychange = "!myname=$(git config --get user.name);myemail=$(git config --get user.email); git log --graph --abbrev-commit --decorate --author $myemail " HEAD --stat
```
## LICENSE

[MIT License](https://github.com/sid04naik/gitconfig-template/blob/master/LICENSE)

Copyright (c) 2020 Siddhant Naik
