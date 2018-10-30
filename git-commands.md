#Git Commands

git config --global user.name ""
git config --global user.email ""

git init						// Initialize Local Git Repository
git add <file> 					// Add file(s) to Index
git add .						// Add all files to staging area
git status						// Check Status of working tree
git commit						// Commit chnages in index
	git commit -m "changes"
git push origin master						// Push to remote repository (github)
git pull						// Pull latest from remote repository
git clone						// Clone repository to a new directory

git rm --cached <file>			// Remove file to staging area
git branch <name>				// Create branch
git checkout <branch-name>		// Go to branch
git merge <brance-name>			// Merge branch to master

###Add remote repository
git remote add origin <link-to-github-repo>
git push -u origin master