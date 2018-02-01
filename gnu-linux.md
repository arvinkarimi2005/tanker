[general](#general)

# general
## networking
### ssh
#### SSH Login Without Password
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

