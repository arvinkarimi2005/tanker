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
## firewall
### centos
* install semanage
`yum install policycoreutils-python`

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


### change ssh port
1. open ssh config file  
`nano /etc/ssh/sshd_config`
2. uncommnet "#Port 22" and change it
3. enable port in firewall
#### ubuntu
`sudo ufw allow {your_port}`
#### centos
`sudo semanage port -a -t ssh_port_t -p tcp {your_port}`
`sudo firewall-cmd --permanent --zone=public --add-port={your_port}/tcp`
`sudo firewall-cmd --reload`
4. restart sshd
`sudo systemctl restart sshd`
5. verify
`ss -tnlp | grep ssh`
6. Login
`ssh user@remote -p {your_port}`
