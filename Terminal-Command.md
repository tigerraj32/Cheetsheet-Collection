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
