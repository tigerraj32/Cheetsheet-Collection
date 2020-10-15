
# List of mostly used git commands

### Setup local git repo
<br>

- This will transform current directory to a git repo.

        git init 

- Create Empty git repo in the specific directory.

        git init <directory path>



### Git add remote repo
<br>

  - To add remote repo

        git remote add <remote url>

### Clone remote repo
<br>

    git clone <remote repo>

### Git detail
<br>

    git remote -v 

    git remote show origin




### git checkout from specific commit

- First get the commid id for specific branch 
        
        git log origin branch_name
        
- Then copy the comit id you want to revert back
- Now hard reset back to commit id

        git reset --hard commit_id



