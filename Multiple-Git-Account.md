There are times when we need to have multiple github account to use simulatenously. This can be done via ssh keys.

## 1. Generating the SSH keys
Before generating an SSH key, we can check to see if we have any existing SSH keys: 
``` 
ls -al ~/.ssh 
```

This will list out all existing public and private key pairs, if any.

If **~/.ssh/id_rsa** is available, we can reuse it, or else we can first generate a key to the default ~/.ssh/id_rsa by running:
```
ssh-keygen -t rsa
```

When asked for the location to save the keys, accept the default location by pressing enter. A private key and public key 
**~/.ssh/id_rsa.pub** will be created at the default ssh location **~/.ssh/**.

Let’s use this default key pair for our personal account.

For the work account we will create different SSH Keys.

```
ssh-keygen -t rsa -C "email@work.com" -f "id_rsa_work1"
```

Now we will have two different keys created 

```
~/.ssh/id_rsa
~/.ssh/id_rsa_work1
```

## 2. Adding the new SSH key to the corresponding GitHub account

Copy the public key 

``` 
pbcopy < ~/.ssh/id_rsa.pub 
```

and then log in to your personal GitHub account:

- Go to Settings
- Select SSH and GPG keys from the menu to the left.
- Click on New SSH key, provide a suitable title, and paste the key in the box below
- Click Add key — and you’re done!


## 3. Registering the new SSH Keys with the ssh-agent

To use the keys, we have to register them with the ssh-agent on our machine. Ensure ssh-agent is running using the command
> ssh-agent -s


Add the keys to the ssh-agent like so:

```
ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/id_rsa_work1
```

To list the ssh key attached to SSH agent
> ssh-add -l

In case we need to remove ssh entries 
> ssh-add -D            //removes all ssh entries from the ssh-agent

## 4. Creating the SSH config File
Here we are actually adding the SSH configuration rules for different hosts, stating which identity file to use for which
domain.

The SSH config file will be available at **~/.ssh/config**. Edit it if it exists, or else we can just create it.

```
 cd ~/.ssh/
 touch config           // Creates the file if not exists
 code config            // Opens the file in VS code, use any editor
```

Make configuration entries for the relevant GitHub accounts similar to the one below in your **~/.ssh/config** file:

```
Host github.com
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa
   
# Work account-1
Host github.com-work1 
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa_work1
```

The above configuration asks ssh-agent to:

- Use id_rsa as the key for any Git URL that uses @github.com
- Use the id_rsa_work1 key for any Git URL that uses @github.com-work1


## Ttying it out

### Create new repo

Snippet
> git remote add origin git@{HOST}:{accountname}/{repo}.git


```
git init
git commit -am "first commit'
git remote add origin git@github.com-work1:Company/testing.git
git push origin master

```

### Clone

> git clone git@github.com-work1: accountname/repo_name.git




## Handling Committer Identify

Tutorial: [ref link](https://www.git-tower.com/learn/git/faq/change-author-name-email)

Even after managing the ssh access to handle multiple git account the commiter identity will remain same for all repo and acccount. In order to correct this we need to set git **user.email** and **user.name** per repo.

 - First go to repository directory in your local drive.
 - Create local git user.name and user.email
### Local set

```bash
git config user.email rajantwanabashu@gmail.co
git config user.name 'tigerraj32'
```

### Local get
```bash
git config --get user.email
git config --get user.name
```
This will create the config file inside **~/repo/.git/** which will look like

> cat config 
```bash 
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
[remote "origin"]
	url = git@github.com:accountname/demo.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[user]
	name = tigerraj32
 email = rajantwanabashu@gmail.com

```

### Global set
```
git config --global user.email mahmoud@zalt.me
git config --global user.name 'Mahmoud Zalt'
```
In case you want to change the identity for xcode project
```
xcrun git config --global user.name 'new_user_name'
xcrun git config --global user.email 'new@email.com'
```

### global get
```
git config --global --get user.email
git config --global --get user.name
```

### This will create a global config file in **~/.gitconfig**


Using --amend for the Very Last Commit

In case you want to change just the very last commit, Git offers a very easy way to do this:

```
git commit --amend --author="tigerraj32 <rajantwnabashu@gmail.com>"
```


