Github recently emphasis us to use github token over username password to be used in github api. Here's how we can configure in current local repo.

## Steps

## Step 1: Remove github from KeyChain
- Open `Keychain Access`
- Select `All Items`
- Search `github`
- Delete all or internet password.
- Or there is a terminal command for this

```bash
git credential-osxkeychain erase
host=github.com
protocol=https
> Enter

// If it is successful, nothing will print out. To test that it works, try and clone a private repository from GitHub. If you are prompted for a password, the keychain entry was deleted.


```

### Step 2: Generate token

- Go to `github.com` and login via username and password.
- Go to `profile` and `setting`
- Go to `Developer Setting`
- Go to `Personal Token` and generate one for you.

### Step 3: Update your remote origin in your machine.

- Go to `github.com` and login via username and password.
- Get the repo url of the repository you want to use token auth. Use `https` as token auth only works for `https`
- Go to your repo directory from terminal
- Remote origin first

> git remote rm origin

- Add the new remote origin that uses token

> git remote add origin https://username:password@github.com/yourpage/repo.git

That's it.

