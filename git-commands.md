```bash
git clone /path/to/repository			# creates a local copy
git clone https://github.com/repopath	# creates a local copy from github repository
git clone "link" 						# download the repository from github to the computer
```

```bash
git init				        # create a git repository locally
git add <filename>              # adds the file to the STAGE
git add *                       # adds all files to the STAGE
git add .                       # adds files to the STAGE
git commit -m "Message"         # commit files to the HEAD, but not yet in the remote repository
git remote add origin <server>  # "Connect" the local files with a remote Repository on github. (if repository was NOT CLONED)
git push origin master      	# Upload the files that are in the HEAD to github. (master can be any branch-name) 
```

BRANCHING
=========
``` bash
git checkout -b branchname      # create a new branch with name "branchname"
git checkout master             # Switch to the Master branch
git branch -d feature_x         # Delete branch
git pull                        # updates local repository
```

MISC
===
```bash
git add -i                       # adds changes interactively
git config color.ui true		 # colors in the console
git config format.pretty oneline # display the log messages in one line only
gitk					         # open built-in git-GUI
```