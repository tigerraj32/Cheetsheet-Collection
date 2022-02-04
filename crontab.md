
`crontab` found in unix operating system is a service that can schedule command to be executed periodically. 

### Sample crontab job

> '* * * * * /bin/execute/this/script.sh'

There are five basic time terms that need to understand in crontab that are represented by * in above command. 
 
- minute (from 0 to 59)
- hour (from 0 to 23)
- day of month (from 1 to 31)
- month (from 1 to 12)
- day of week (from 0 to 6) (0=Sunday)

If you use * then it means every minute , hour, day, month or weekdsya

### List crontab job

    crontab -l

### To add or edit crontab jobs

    crontab -e

To open in nano editor

    env EDITOR=nano crontab  -e

## Before config the crontab file
The macOS Mojave 10.14.0 release introduces some updates to the Apple user-centric inter-app data-sharing security model. After that user has to give access to cron to the calendar and certain directories, otherwise the crontab command will result in error message shown in the subtitle.

- Step 1: Go to System Preferences > Security & Privacy > Privacy > Full Disk Access:
- Step 2: Click on the (+) icon to add an item to the list:
- Step 3: Press command+shift+G then type /usr/sbin/cron and Enter, you can find cron in finder:
- Step 4: Select the cron file and click Open:


## Examples

Following are some of sample job scheduling via `crontab`

Command | Description
--------| ----------
`*/1 * * * * /bin/execute/this/script.sh` | Run script every minute
`0 1 * * 5 /bin/execute/this/script.sh`| Run script every friday @ 1 AM
`0 1 * * 1-5 /bin/execute/this/script.sh` | Run script every 1AM on workdays from Sunday - Friday
`@reboot /bin/execute/this/script.sh` | Run script on every reboot or startup.
`@yearly /bin/execute/this/script.sh` | Run script once a year "0 0 1 1 *" 
`@monthly /bin/execute/this/script.sh` |   Run once  a month    "0 0 1 * *"
`@weekly  /bin/execute/this/script.sh` |   Run once  a week     "0 0 * * 0"
`@daily  /bin/execute/this/script.sh`|    Run once  a day      "0 0 * * *"
`@hourly  /bin/execute/this/script.sh`|   Run once  an hour    "0 * * * *"


## Verify crontab job 

Since cron tab jobs are executed in background it is not that clear if the job is executed or not. 
In order to verify the job status  we can print some status text after executing completes in a file. 
Then we monitor that file, as below

###Content of script.sh

```bash
echo "hello" > ~/var/log/job_log.txt
```

Now monitor using foollowing command

    tail -f /var/log/job_log.txt
    

### Tips



