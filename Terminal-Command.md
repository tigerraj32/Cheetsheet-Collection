# List of usefull terminal commands


## Fix Crackling or Garbled Sound By Killing Core Audio

    sudo killall coreaudiod
    
Now, enter your user password (assuming you have admin access) to authorize the command. 
The coreaudiod process will be killed and should automatically relaunch itself. 
Try playing some music or other sound to see if you still have the issue.

If you have no audio at all, you might need to manually restart Core Audio with the following Terminal command:

    sudo launchctl stop com.apple.audio.coreaudiod && sudo launchctl start com.apple.audio.coreaudiod   

## Hide Show Hidden files in Mac

> Command + Shift + Period to instantly toggle to show hidden files


## Check Process Running On Specific Port

`lsof` command (List Open Files) is used to list all open files on a Linux system To find the process/service listening on a particular port, type (specify the port).

```bash 
$ lsof -i :80

//output
COMMAND   PID USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
node    19440 user   32u  IPv6 0x2a96b6da87ea8e57      0t0  TCP *:irdmi (LISTEN)
```
To kill the process 
> kill 19440




