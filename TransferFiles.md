# Transfer files from PC to Android or Android TV

This process will allow the transfer of files from pc to android tv or android device in local network. 

### Install SSH Server

- Install `ssh server` on android device
  - https://play.google.com/store/apps/details?id=com.theolivetree.sshserver
- Open `ssh server`
- Click on start server. This will start the ssh server and you will be provided with ip, username, password and home directory info.

### Transfer file

- Open terminal
- ssh -oHostKeyAlgorithms=+ssh-dss  ssh@192.168.100.157 -p 2222 
- scp -P  2222 -oHostKeyAlgorithms=+ssh-dss  movie.mkv ssh@192.168.100.157:/storage/emulated/0/Movies
