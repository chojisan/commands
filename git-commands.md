# Git Commands

git config --global user.name ""
git config --global user.email ""

git init .						// Initialize Local Git Repository
git add <file> 					// Add file(s) to Index
git add .						// Add all files to staging area
git status						// Check Status of working tree
git commit						// Commit chnages in index
	git commit -m "changes"
	git commit -am "changes"	// Add and Commit	
git push origin master			// Push to remote repository (github)
git pull						// Pull latest from remote repository
git clone						// Clone repository to a new directory

git rm --cached <file>			// Remove file to staging area
git branch <branch-name>				// Create branch
git checkout <branch-name>		// Go to branch
	git checkout -b <branch-name>	// create if branch does not exists and switch to branch
git merge <brance-name>			// Merge branch to master

### Add remote repository
git remote add origin <link-to-github-repo>
git push -u origin master

git show <commit-code>
git log
git diff <file-name>

git rm -r --cached directory/		// remove cached file/directory



Updating a feature branch
First we'll update your local master branch. Go to your local project and check out the branch you want to merge into (your local master branch)
```
$ git checkout master
```

Fetch the remote, bringing the branches and their commits from the remote repository. You can use the -p, --prune option to delete any remote-tracking references that no longer exist in the remote. Commits to master will be stored in a local branch, remotes/origin/master
```
$ git fetch -p origin
```

Merge the changes from origin/master into your local master branch. This brings your master branch in sync with the remote repository, without losing your local changes. If your local branch didn't have any unique commits, Git will instead perform a "fast-forward".
```
$ git merge origin/master
```

Check out the branch you want to merge into
```
$ git checkout <feature-branch>
```

Merge your (now updated) master branch into your feature branch to update it with the latest changes from your team.
```
$ git merge master
```

Depending on your git configuration this may open vim. Enter a commit message, save, and quit vim e.g. press "a" to enter insert mode and append text following current cursor position, press Esc key to enter command mode, press :wq to write the file to disk and quit.

This only updates your local feature branch. To update it on GitHub, push your changes.
```
$ git push origin <feature-branch>
```


For a list of files to be pushed, run:

git diff --stat --cached [remote/branch]
example:

git diff --stat --cached origin/master
For the code diff of the files to be pushed, run:

git diff [remote repo/branch]
To see full file paths of the files that will change, run:

git diff --numstat [remote repo/branch]


Inspecting Things
Let’s look at inspecting changes first.

git diff— See all file changes locally. A file name can be appended to show changes for only one file.

git log — See all commit history. Can also be used for a file with git log -p my_file. Enter q to exit.

git blame my_file— See who changed what and when in my_file.

git reflog — Show a log of changes to the local repository’s HEAD. Good for finding lost work.


Undoing Things
git reset, git checkout, and git revert are used to undo the effects of changes to your repository. These commands can be tricky to keep straight.

git reset and git checkout can be used on both commits and individual files.git revert is used only at the commit level.

If you are just dealing with your own local commits that haven’t been merged into collaborative remote work, you can use any of these commands.

If you are working collaboratively and need to neutralize a commit in the remote branch, git revert is your tool.

Each of these commands can take a variety of options. Here are common uses:

git reset --hard HEAD— Discard staged and unstaged changes since the most recent commit.
Specify a different commit instead of HEAD to discard changes since that commit. --hard specifies that both the staged and unstaged changes are discarded.

Make sure you don’t discard a commit from a remote branch that your collaborators are depending upon!

git checkout my_commit— Discard unstaged changes since my_commit.
HEAD is often used for my_commit to discard changes to your local working directory since the most recent commit.

checkout is best used for local-only undos. It doesn’t mess up the commit history from a remote branch that your collaborators are depending upon!

If you use checkout with a branch instead of a commit, HEAD is switched to the specified branch and the working directory is updated to match. This is the more common use of the checkout command.

git revert my_commit —Undo the effects of changes in my_commit. revert makes a new commit when it undoes the changes.
revert is safe for collaborative projects because it doesn’t overwrite history that other users’ branches might depend upon.


### Pull Request

fork repo
	git clone
	git checkout -b “fix name”
	fix
	git status / git diff
	git add
	git commit
	git push origin “fix name”
	github page “compare and pull request”

