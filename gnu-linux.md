[general](#general)
# services and applications

## shadowsocks
### install shadowsock client
#### ubuntu
```
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```

## proxy terminal
* add this bash to .bashrc or
create file function.sh and add
`source functions.sh` in .bashrc
```bash
#!/bin/bash

assign_proxy(){
  PROXY_ENV="http_proxy ftp_proxy https_proxy all_proxy HTTP_PROXY HTTPS_PROXY FTP_PROXY ALL_PROXY"
  for envar in $PROXY_ENV
  do
     export $envar=$1
  done
  for envar in "no_proxy NO_PROXY"
  do
     export $envar=$2
  done
}

clr_proxy(){
   PROXY_ENV="http_proxy ftp_proxy https_proxy all_proxy HTTP_PROXY HTTPS_PROXY FTP_PROXY ALL_PROXY"
   for envar in $PROXY_ENV
   do
      unset $envar
   done
}

my_proxy(){
#  read -p "ip: " -s  ip && echo -e " "
#  read -p "Port " -s port &&  echo -e " "
#  read -p "method" -s method && echo -e " "
 PROXY='https://127.0.0.1:1080/';
 if [[ ! -z "$1" ]]; then
   PROXY=$1;
 fi

  echo $PROXY;
  proxy_value="$PROXY"
  no_proxy_value="localhost,127.0.0.1,LocalAddress,LocalDomain.com"
  assign_proxy $proxy_value $no_proxy_value
}

```
# users and groups
## add users
* create user with home folder and login
`sudo useradd -m username -p PASSWORD`
* add user to sudo group on ubuntu
### ubuntu
`usermod -aG sudo username`
### centos
`usermod -aG wheel username`
## modify users 
* set expiry time for user
```bash
sudo usermod --expiredate 1 samual
```
* lock user password ( disable ssh )
```bash 
sudo passwd -l samual
```
* enable user password 
```bash
sudo passwd -u training
```
* set expiry time for user password
```bash
sudo passwd -e  2013-05-31 samual
```
* enable root user 
```bash
su - 
passwd 
```
## delete users 
* r option removes home directory 
```bash
userdel -r samual
```
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
```
sudo semanage port -a -t ssh_port_t -p tcp {your_port}
sudo firewall-cmd --permanent --zone=public --add-port={your_port}/tcp
sudo firewall-cmd --reload
```

4. restart sshd
`sudo systemctl restart sshd`
5. verify
`ss -tnlp | grep ssh`
6. Login
`ssh user@remote -p {your_port}`


# other useful commands
* copy file to clipboard 
```bash
xclip -selection clipboard .ssh/id_rsa.pub
```