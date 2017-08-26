# How to coordinate with team members at Github

### Setting up local git repo to connect to Github repo
1. cloned repository in github `git clone <repo> <folder>`
2. `git pull origin master` to update master branch
3. create own branch `git branch <branch_name>`, note: convention is own name, sometimes features
- optional: if making own feature create a new branch with feature_name
4. `git checkout <branch_name>` then `git push --set-upstream origin <branch_name>` to connect local branch to Github repo branch

### After commiting own changes
1. `git push origin <branch_name>`
2. Go to Github, compare and create new pull request to Github repo
3. IF OWNER
  * check for conflicts and merge if no conflicts
  * if conflicts, fix conflicts before merge

### Update local branch master ()
1. Get Github master updates by merging local branch master `git pull origin master` NOTE: make sure to checkout to 'master' branch first.

### Merge local branch master with own branch
1. `git checkout <branch_name>` then `git merge master`
