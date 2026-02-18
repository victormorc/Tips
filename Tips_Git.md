---
project: "-"
brief: Notes related to Git commands
author: Victor Moreno Calvo
date: 24-03-2018
version: 1.0
---

# BASIC GIT COMMANDS:

## CLONE REPOSITORY:
- Clone repository:
$ git clone --recursive <url_repository>

## INFO BRANCH:
- Get local branches, which branch is tracking and last commit and its message.
$ git branch -vvv (v for verbose).

- Show local and remote branches.
$ git branch -a (a for all).	// Remote branches should be displayed as remotes/origin/<name_branch>

## SHOW:
- Show modified files in commit
$ git show --name-only <commit_sha_1>
$ git show HEAD (shows modified files in latest commit).

- Show file from a different branch without change to it.
$ git show <branch_name>:<path_to_file>

## PUSH:
- Push into remote repository local branch:
$ git push -u origin <branch_name>

- Delete remote branch:
$ git push <remote_name> --delete <branch_name>
$ git push <remote_name> :<branch_name>

## PULL:
- Pull (merge commits) from remote repository into local branch while working in other local branch:
$ git pull <remote_name> <remote_branch_name>:<local_branch_name>

- Pull a submodule from a branch.
$ git submodule update --init --recursive

- Pull the latest commit in remote of a branch, overwrite local files (discarding local files and commits).
$ git fetch		(downloads the latest from remote)
$ git reset --hard origin/<branch_name>

## UNDO:
- Delete unstaged changes for a working file:
$ git checkout -- <file_name>
$ git checkout -- .			(Delete recursively all unstaged changes in versioned files)

- Untrack a file, already tracked in git.
$ git rm --cached <file_name>

-Undo a merge, already commited:
$ git revert <commit_sha> -m 1	(revert last commit)

- Undo a commit, already push into remote.
$ git revert <commit_sha>

- Delete a file, but didn't commit.
$ git checkout HEAD^ <file_name>

- Delete a file and committed the deletion:
$ git reset --hard HEAD~1

- I committed the deletion and then I did more commits:
$ git checkout <commit hash> -- <file_name>  (Last commit which still had the file).
$ git checkout <deletion commit hash>~1 -- <file_name>

- I deleted a file, commited and pushed:
$ git checkout HEAD^ <file_name>
$ git commit --amend
$ git push -f

## RESET VS REVERT:
- Git reset should generally be considered a 'local' undo method. 
    A reset should be used when undoing changes to a private branch.
    
- Git revert is the best tool for undoing shared public changes

## COMMIT:
- Commit all files (staged and unstage) with a message:
$ git commit -am "Message for the commit"

## RESET:
- Unstage files (move them to stage area):
$ git reset			# Move files from unstage to stage area. Default option with reseet is --mixed 
$ git reset -- . 	# Move files recursively which are in current folder from unstage to stage area.

## MOVE:
- Move your unstaged changes from the working branch to another:
$ git stash                     # Save the unstaged changes
$ git checkout <other_branch>   # Move to another branch
$ git stash pop                 # Apply the saved changes and delete the changes from stash

- Copy file(s) from from another branch to the current one in Git, there are two possible options:
    use the git show command:
    $ git show <branch_name>:path/to/file > path/to/local/file
    
    use the git checkout command:
    $ git checkout <branch_name> -- <paths>

## STASH:
https://es.atlassian.com/git/tutorials/saving-changes/git-stash

- Save unstaged changes into stash with a message:
$ git stash save -- "Message"

## LOG:
- Show history changes of a file. Display the full diff of each commit.
$ git log -p -- <file_name>

- Show the lastest n commits information.
$ git log -n

- Show each commit info in one line.
$ git log --oneline

- Show graph with all branches
$ git log --all --decorate --oneline --graph

## DIFF:
- Diff any change between two branches.
$ git diff <name_branch_1> <name_branch_2>

- Diff file between two branches.
$ git diff <name_branch_1> <name_branch_2> -- <file_name>

- Show only files that are different between the two branches (without changes themselves):
$ git diff --name-status <name_branch_1>..<name_branch_2>

- Show diff between two files in different paths in the branches.
$ git diff <branch1>:<path_file_1> <branch2>:<path_file_2>

- Show diff between two commits (same branch)
$ git diff <commit_sha_1> <commit_sha_2>   # (commit_sha_1 -> older, commit_sha_2 -> newer)

- Show diff between two commits in file.
$ git diff <commit_sha_1> <commit_sha_2> -- <file_name>

- Show changed files between two commits (only the name):
$ git diff <commit_sha_1> <commit_sha_2> --name-only

## RENAME:
- Rename local and origin branch.
$ git branch -m <old_name> <new_name>	# move locally, only if you are in a different branch
$ git push [-u] origin <new_name> 		# push new branch, --set-upstream, optional
$ git push origin :<old_name>			# delete in origin old branch

## REMOTE:
- Update list of remote branches name
$ git remote prune origin

- Show remote url:
$ git remote -v <name_remote>
$ git remote -v 	                    # Shows all remote urls

- List and show remote branches name.
$ git branch -r                         # -r, --remotes

## CHECKOUT:
- Checkout a specific commit for a file:
$ git checkout <sha1_commit> <path_to_file>

- Checkout remote branch and bring it to local repository.
$ git checkout -t <name_remote>/<name_branch>	                # $git checkout -t origin/BRANCH-44_TESTING

- Set up a local branch with a different name than the remote branch.
$ git checkout -b <local_branch> -t <name_remote>/<name_branch>  # $git checkout -t BLACK_BOX_TESTING origin/BRANCH-44_TESTING

- "Merge" specific files from another branch:
$ git checkout <source_branch> -- <path_to_file>

- Restore a deleted branch.
$ git checkout -b <name_branch> <sha>   # If you do not know the value of <sha> -> $ git reflog --no-abbrev


## TRACK BRANCH:
- Make local branch to track remote branch:
$ git branch -u <name_remote>/<name_branch>		# ($ git branch -u NAME_LOCAL_BRANCH origin/NAME_REMOTE_BRANCH

## INIT REPOSITORY:
- Create a repository.
$ git init		                                # Set up the current folder as git repository in local
$ git remote add origin <url> 	                # Create a remote repository in the url given

## TAGS:
- Create an annotated tag:
$ git tag -a <tag_name> -m "Message"

- Show all tags in local:
$ git tag

- Show specific tag:
$ git show <tag_name>
$ git show -l "tag_name_1_0*"	                # Show all tags which start with tag_name_1_0

- Push tag into remote:
$ git push <remote_name> <tag_name>

- Delete tag:
$ git tag -d <tag_name>

## REBASE:
- Join several commits into one.
$ git rebase -i <commit_hash_not_included>  # The commit specified is not included in the rebase, newers yes.

## CHERRY-PICK
- Select commit from any other place (branch, reflog, stash, ...) and put it on top of current branch:
$ git cherry-pick <commit_hash>

## EMPTY DIRECTORY:
- Commit an empty directory (Create a .gitignore file inside the directory):
$ echo "# Ignore everything in this directory" >> .gitignore
$ echo "*" >> .gitignore
$ echo "# Except this file" >> .gitignore
$ echo "!.gitignore" >> .gitignore

## MISCELLANEOUS
- See file in a different branch without changing branches
$ git show <branch_name>:<path_file>

- See changes to a specific file by a commit.
    $ git show <commit_hash> -- <path_file>
    $ git diff <commit_hash>^1 <commit_hash> -- <path_file>

- Show files changed in a commit.
    $ git log --raw

- Pull of all the remote branches.
    $ git branch -r | grep -v "HEAD" | sed "s/origin\///" | xargs -I {} git checkout -b {} origin/{}

- See changes to a specific file by a commit.
    $ git show <commit_hash> -- <path_file>
    $ git diff <commit_hash>^1 <commit_hash> -- <path_file>
    
- Show commit which modified a specific file.
    $ git log -- <path_file>

- Show files changed in a commit.
    $ git log --raw

- Delete remote branch.
    $ git push <remote_name> -d <branch_name>		# git push origin -d FEATURE_NEW_APP
    
- Empty commit.
    $ git commit --allow-empty -m "Message"
    
- Make a symbolic name for a branch.
    $ git symbolic-ref refs/heads/FEAT_X refs/heads/PROJ_FEATURE_X   (In this case the real branch is called PROJ_FEATURE_X, however you can use FEAT_X to refer to it).

- Identify files which are currently being tracked:
    $ git ls-tree -r <branch_name> --name-only
    
- Identify the commit for a submodule for an specific commit in the main repository:
    $ git ls-tree <commit_main_repository> -- <submodule>
    
- Push a single tag:
    $ git push <remote_name> tag <tag_name>
    
- Identify which branches contain a specific commit
    $ git branch -a --contains <commit_hash>
    
- Identify the hash for a ref (branch name, tag, stash, ...).
    $ git rev-parse <ref>
    
- Show all references in a local repository:
    $ git show-ref
    
- Show tags in remote:
    $ git ls-remote --tags <remote>		# $git tag -> lists all local tags.
    
- Search for a commit with a string in the current branch :
    $ git log --grep="string"           # If you want to search in all the branches use the --all option
	
## CONFIGURE:
* https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration#Formatting-and-Whitespace
	
- Avoid problems with Line Feed (LF) when working with different O.S. "core.autocrlf"

- Checkout Windows-style, commit Unix-style line endings.
    $ git config core.autocrlf true

- Checkout as-is, commit Unix-style line endings.
    $ git config core.autocrlf input

- Checkout as-is, commit as-is
    $ git config core.autocrlf false.

- Set color to git output to terminal
    $ git config --global core.ui auto

- Show configuration
    $ git config --list




    

