
# Setting up password less SSH Login.


Normally `ssh` with username and password can be hacked by some kind of password crack mechanism.  
There is better way we can connect via `ssh` that is by cusing public key based authentication.


## Generate new ssh key pair

    sh-keygen -t rsa -b 4096 -C "your_email@domain.com"


This command create 4096 but ssh key pair with email address as comment. Then accept default values. 
After this we sill have `private key - id_rsa` and `public key - id_rsa.oub`. We need to secure `private key` 
but we can share the `public key`. 

## Copy the public key to remote server

Now that you have generated an SSH key pair, in order to be able to login to your server 
without a password you need to copy the public key to the server you want to manage.

The easiest way to copy your public key to your server is to use a command called ssh-copy-id.
On your local machine terminal type:

    ssh-copy-id remote_username@server_ip_address   


## Login to your server using SSH keys

After completing the steps above you should be able log in to the remote server without being prompted for a password.

To test it just try to login to your server via SSH:

        ssh remote_username@server_ip_address
        
       
