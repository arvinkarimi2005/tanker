[general](#general)

# users and groups
## add users
* create user with home folder and login
`sudo useradd -m username -p PASSWORD`
* add user to sudo group on ubuntu
### ubuntu
`usermod -aG sudo username`
### centos
`usermod -aG wheel username`

# networking
## ssh
### SSH Login Without Password
1. create rsa on client
`ssh-keygen -t rsa`
2. copy public key to remote
* make .ssh directory on remote
`ssh user@remote mkdir -p .ssh`
* copy public key to .ssh dir
`cat .ssh/id_rsa.pub | ssh user@remote 'cat >> .ssh/authorized_keys'`
* set permision to directory and files
` ssh user@remote "chmod 700 .ssh; chmod 640 .ssh/authorized_keys"`

 or just do this
`ssh-copy-id user@remote`



#ubuntu
##users
