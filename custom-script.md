
## Creating custom Shortcut

### Create shell script to copy token into clipboard.
Mac provide some execlusive command such as **pbcopy** and **pbpaste** to copy the standard input into clipboard. Now create 
**token.sh** script with following code

```sh 
php /Volumes/Product/Research/uatci/token.php | pbcopy
```
make the script executable.
> chmod +x token.sh

[Refer here for other platform](https://www.ostechnix.com/how-to-use-pbcopy-and-pbpaste-commands-on-linux/) for similar command like **pbcopy**

### Make the Shell script accessible globally

Create a symbolic link to token.sh in **/usr/local/bin/**  so that you can directly access this script from anywhere.

> ln -s /your-directory-path/token.sh /usr/local/bin/token

After this we can run following command to get token from out php script
> token

output: 
> AwFmHNkA1HdM1VHFadu89nE3xZuKO3pLQ7cHOrCj2x2WZoSt 

