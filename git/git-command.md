
# List of mostly used git commands

## Setup local git repo
<br>

- This will transform current directory to a git repo.

        git init 

- Create Empty git repo in the specific directory.

        git init <directory path>



## Git add remote repo
<br>

  - To add remote repo

        git remote add <remote url>

## Clone remote repo
<br>

    git clone <remote repo>

## Git detail
<br>

    git remote -v 

    git remote show origin
  

## git checkout from specific commit

- First get the commid id for specific branch 
        
        git log origin branch_name
        
- Then copy the comit id you want to revert back
- Now hard reset back to commit id

        git reset --hard commit_id


## git cache user credential

Turn on credential helper so that Git will save your password for some time. By default Git will cache for 15 minutes

        git config --global credential.helper cache
        # set git to use the credential memory cache
        
        # To increase the cache timeout
        git config --global credential.helper 'cache --timeout=3600
        #Set the cache timeout to 1 hrs (setting is in seconds)
        
         
To save user credential permanently. 

         git config --global credential.helper store
         
         #then run git pull and ener username and password.
         git pull

Password is store in plain text in `~/.git-credentials`. So if you change the git password then you should remove the store credential from `~/.git-credential`        
         
         
         
         
         
