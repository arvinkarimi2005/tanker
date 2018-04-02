# Table of Contents:
* [services and applications](#services-and-applications)
   * [files](#files)
     * [manipulate](#manipulate)
     * [read](#read)
   * [shadowsocks](#shadowsocks)
      * [install shadowsock client](#install-shadowsock-client)
         * [ubuntu](#ubuntu)
* [proxy terminal](#proxy-terminal)
* [users and groups](#users-and-groups)
   * [add users](#add-users)
      * [ubuntu](#ubuntu-2)
      * [centos](#centos-1)
   * [modify users](#modify-users)
   * [delete users](#delete-users)
* [other useful commands](#other-useful-commands)
___
# services and applications
## basic(gnu) tools
### files
#### manipulate
1- tee (Copy standard input to each FILE, and also to standard output.)
-a appends to file exisists
```bash
ls -a | tee output.file
```
#### read
1- cat (concatenate files and print on the standard output,Copy standard input to standard output.
)
options:
* -n : number all output lines
* -A : equivalent to -vET
* -E : display $ at end of each line, -e:-vE
* -T : display TAB characters as ^I, -t: -vT
* -v : use ^ and M- notation, except for LFD and TAB
```bash
cat test test1
cat -n test
```
2- more, less, sort
  1- more (file perusal filter for crt viewing), less (opposite of more) 
  ```bash
  more file1
  tail -F /var/etc/nginx/access.logs | more
  ```
  2- sort (sort lines of text files)
    options:
    * -n : numeric-sort
    * -r : reverse
    * -k : sort by column number
    * -t : field separator
    * -u : unique
    * -h : human-numeric-sort
    ```bash
      sort file1 file2
      sort file1
      sort -r file1
      sort -t "," -nk3 file1.csv
      sort -u file1
      ls -alh | sort > sorted.text
    ```
3- wc (count files or standard output)
options:
* -l : lines
* -w : words
* -m : chars
* -c : bytes
```bash
wc -l file1
```
4-  tail (last line of files)
-F follows and retry to connect
```bash
tail -F -n 20 /var/log/nginx/access.log
```
5- head (output the first part of files)
```bash
head -n 20 /var/log/nginx/access.log
```
6- split (split a file into pieces)
* options
 -l : customize line (ex -l1200)
 -b : file size (ex: -b 500M)
 -n : Generate n chunks output files (ex -n5)
 -e : Prevent Zero Size Split output files
in below example ~ngx~ is prefix of generated file
```bash
split /var/log/nginx/access.log ngx
```
merge splited files
```bash
cat Split_IS0_a* > Windows_Server.iso
```
7- join (join lines of two files on a common field)
* 1- (field of first file)
* 2- (field of second file)
```bash
join -1 2 -2 1 wine.txt reviews.txt
```
sorted
```bash
join -1 2 -2 1 <(sort -k 2 wine.txt) <(sort reviews.txt)
```
specify a field separator for joining
* -t ,
```bash
join -1 2 -2 3 -t , names.csv transactions.csv
```
8- paste (paste - merge lines of files)
```bash
paste file1 file2
```
---
## shadowsocks
### install shadowsock client
#### ubuntu
```
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```
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
***
# other useful commands
* copy file to clipboard
```bash
xclip -selection clipboard .ssh/id_rsa.pub
```
* reconfigure keyboard layout
```bash
sudo dpkg-reconfigure keyboard-configuration
```
