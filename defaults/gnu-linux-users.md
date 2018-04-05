___
# users and groups
## add users
* create user with home folder and login
`sudo useradd -m username -p PASSWORD`
* add user to sudo group on ubuntu
### ubuntu
`usermod -aG sudo arvin`
### centos
`usermod -aG wheel arvin`
## modify users
* set expiry time for user
```bash
sudo usermod --expiredate 1 arvin
```
* lock user password ( disable ssh )
```bash
sudo passwd -l arvin
```
* enable user password
```bash
sudo passwd -u training
```
* set expiry time for user password
```bash
sudo passwd -e  2013-05-31 arvin
```
* enable root user
```bash
su -
passwd
```
## delete users
* r option removes home directory
```bash
userdel -r arvin
```
