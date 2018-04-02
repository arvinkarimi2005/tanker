# Table of Contents:
* [networking](#networking)
   * [firewall](#firewall)
      * [centos](#centos-2)
   * [ssh](#ssh)
      * [SSH Login Without Password](#ssh-login-without-password)
      * [change ssh port](#change-ssh-port)
         * [ubuntu](#ubuntu-3)
         * [centos](#centos-3)

# networking
# proxy terminal
- for all users /etc/profile
- for current users .bashrc
- for current session run this 
```bash
export http_proxy=http://USERNAME:PASSOWRD@proxy-server.mycorp.com:3128/
export http_proxy=http://USERNAME:PASSOWRD@proxy-server.mycorp.com:3128/

```
* add this bash to .bashrc or
create file function.sh and add
`source functions.sh` in .bashrc
```bash
#!/bin/bash

assign_proxy(){
  PROXY_ENV="http_proxy ftp_proxy https_proxy  socks_proxy all_proxy HTTP_PROXY HTTPS_PROXY FTP_PROXY SOCKS_PROXY ALL_PROXY"
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
  PROXY_ENV="http_proxy ftp_proxy https_proxy  socks_proxy all_proxy HTTP_PROXY HTTPS_PROXY FTP_PROXY SOCKS_PROXY ALL_PROXY"
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
___
## firewall
### centos
* install semanage
`yum install policycoreutils-python`
___
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
