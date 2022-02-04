## Introduction to git large file storage (git-lfs)

First of all git allows to track files under 100 MB with normal git command. If you wish to track large files above 100 MB, then we will require to use **git lfs**

## Install Git large file storage.
**Debian:**

```
$ curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash

$ sudo apt-get install git-lfs
```

**MacOS (Using Homebrew)**

```
$ brew update
$ brew install git-lfs

// Update global git config

$ git lfs install
```

**Ubuntu**

```
$ curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash

sudo apt-get install git-lfs
```


**Windows**


Use the Windows installer here: https://github.com/git-lfs/git-lfs/releases

After installing, you can install using:

>git lfs install

## Uses of git lfs
> ref: https://help.github.com/en/github/managing-large-files/configuring-git-large-file-storage

- Open Terminal.
- Change your current working directory to an existing repository you'd like to use with Git LFS.
- To associate a file type in your repository with Git LFS, enter git lfs track followed by the name of the file extension you want to automatically upload to Git LFS. For example, to associate a .psd file, enter the following command:

```
$ git lfs track "*.psd"
Adding path *.psd
```

- Every file type you want to associate with Git LFS will need to be added with git lfs track. This command amends your repository's .gitattributes file and associates large files with Git LFS.
Tip: We strongly suggest that you commit your local .gitattributes file into your repository. Relying on a global .gitattributes file associated with Git LFS may cause conflicts when contributing to other Git projects.
- Add a file to the repository matching the extension you've associated:

```
$ git add path/to/file.psd
Commit the file and push it to GitHub:
$ git commit -m "add file.psd"
$ git push origin master
```

- You should see some diagnostic information about your file upload:
> Sending file.psd
> 44.74 MB / 81.04 MB  55.21 % 14s
> 64.74 MB / 81.04 MB  79.21 % 3s



