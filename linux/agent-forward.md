Agent Forward
=============

SSH agent forward can bring ssh key to remote console. So if you have
server A and server B. And on both server, you can use ssh key to login.
Then with agent forward. You can login to server A and bring the ssh key
to server A. Then use it login to server B from your server A. 
With the following command:

    ssh -A account@a.your.server

You can login to server A and forward your ssh key to server A. The benefit 
is you don't need to put your private key on server A permanent. But you 
still can use it to login to server B through server A.

The secret behind this is when you forward your key to server A. There will
be a new environment variable `$SSH_AUTH_SOCK`. The value is location of a 
temp unix socket file, looks like:

    /tmp/ssh-ReYIMSuBsk/agent.4021

And when you use ssh trying to connect to server B. ssh will first look 
at `$SSH_AUTH_SOCK` to see is it possible to get ssh key for this connection.

And since I know its based on an file and environmet variable. I can pass it 
to another account. So if I want to use another account `http` on server A
to pull git commit from github use the same ssh key. I can use the following 
commands. First, login to server A:

    ssh -A account@a.your.server

Then give tmp socket file access to another account:

    setfacl -m u:http:rw $SSH_AUTH_SOCK
    setfacl -m u:http:x $(dirname $SSH_AUTH_SOCK)

`setfacl` is a command of file Access Control Lists. A new generation file access
control system. Current linux distributions already have acl enabled. 
Then sudo command:

    sudo -u http -s /bin/sh -c "env SSH_AUTH_SOCK=$SSH_AUTH_SOCK git push"
    
Why take so much efforts to do a simple git pull, because I use it to sync 
website, and:

  1. I need every file with correct owner `http`.
  2. I need push new change to github later: 
  
        sudo -u http -s /bin/sh -c "env SSH_AUTH_SOCK=$SSH_AUTH_SOCK git push"

     This requires correct ssh keys for my github account.

  3. I don't want to upload my ssh key to web server. And I don't want to 
     put it under account `http`

There are still one issue now. With `-c`, I can only execute one command once.
So if I want to switch to the user. And do a series commands, like change codes,
commit, push. I have to export the env to the `http` account. There are two methods
to do it. First:

    sudo -E -u http -s /bin/sh

But this will bring all environment to new login session(account `http`). So
another method is to edit the config of sudoer. Add the following line to `/etc/sudoers`

    Defaults env_keep+=SSH_AUTH_SOCK

Then all session will bring `$SSH_AUTH_SOCK` when use sudo.

Refs: [1][]

[1]:https://serverfault.com/questions/107187/ssh-agent-forwarding-and-sudo-to-another-user/448972#448972
