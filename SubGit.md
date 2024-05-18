# SVN to GitHub migration using SubGit.

## Download subgit zip from https://subgit.com/
### Extract it and Navigate to subgit/bin directory and execute below command.
`subgit configure --svn-url SVN_Project_url localGitRepo.git`

### Modify files under localGitRepo.git/subgit/ dir (authors.txt, config and passwd) you can find examples on official subgit website.
`subgit install repository.git`

### After installation is successfull, you can verify whether conversion is done properly or not using below command.
`subgit verify repository.git`

### Add remote github repo to push your localgitrepo on github.
`git remote add origin git@github.com:user/GitRepo.git`

### If you have large files, then it's better to move it to git lfs as github won't allow you to push large files (>100MB) into your repository directly. 
`git lfs install`

### list files which are more than 50MB in size
`git lfs migrate info --everything --above="50 MB"`

### Add filetypes from the output of above command to the --include of below command and move them to git lfs.
`git lfs migrate import --everything --include="*.ear,*.war,*.zip" --verbose`

### Verify by listing the files which are moved to git lfs.
`git lfs ls-files --long --size --all`

### Push everything along with lfs files to github repository lfs.
`git push origin --all --follow-tags --verbose`

### If above command didn't work for you then push git branches one by one first master or main branch.
`git push origin master\main`

### Now other branches.
`git push origin refs/heads/branchname`

### Push git tags one by one.
`git push origin tag tagname`
