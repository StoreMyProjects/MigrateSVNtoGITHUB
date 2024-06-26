# MigrateSVNtoGITHUB
## Step 1: Convert the SVN Repository to a Local Git Repository
### 1.1. Create a text file named authors-transform.txt to translate SVN commit authors into Git format.
```
svn log -q <SVN_REPO_URL> | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "",  $2); print $2" = "$2" <"$2">"}' | sort -u > authors-transform.txt
```
### 1.2. Edit the authors-transform.txt file to match the syntax you need for your Git author information.
```
svn_username = Firstname Lastname <email@example.com>
```
### 1.3. Clone the SVN repository into a new local Git repository.
```
git svn clone --authors-file=authors-transform.txt --stdlayout <SVN_REPO_URL> your_project
```
### 1.4. Navigate to the new directory that was just created by the git svn clone command.
```
cd your_project
```
## Step 2: Convert SVN Tags and Branches to Local Git Tags and Branches
### 2.1. Convert SVN tags to local Git tags.
```
for t in $(git for-each-ref --format='%(refname:short)' refs/remotes/origin/tags); do git tag $(basename $t) $t && git branch -D -r $t; done
```
### 2.2. Convert SVN branches to local Git branches.
```
for b in $(git for-each-ref --format='%(refname:short)' refs/remotes); do git branch $(basename $b) $b && git branch -D -r $b; done
```
## Step 3: Push the Local Git Repository to GitHub
### 3.1. Add the GitHub repository as a remote.
```
git remote add origin https://<USERNAME>@github.com/<USERNAME>/<REPO_NAME>.git
```
### 3.2. Migrate lfs objects if there are large files in size
```
git lfs install
git lfs migrate info --everything  --above="50 MB"
git lfs migrate import --everything --include="*.ear,*.7z,*.aix" --verbose
git lfs ls-files --long --size --all
```
### 3.3. Push the local Git repository to the GitHub repository.
```
git push --all origin
git push --tags origin
```
Now you have successfully migrated your SVN project repository with multiple branches, tags, and a trunk to a GitHub repository.
