# Table of Contents:
* [file and directory manipulation](#file-and-directory-manipulation)
   * [directories](#directories)
   * [files](#files)
     * [search](#search)
     * [manipulate](#manipulate)
     * [read](#read)
   * [shadowsocks](#shadowsocks)
      * [install shadowsock client](#install-shadowsock-client)
         * [ubuntu](#ubuntu)
* [other useful commands](#other-useful-commands)
___
# file and directory manipulation
grep,find,locate
tee
cat,
## basic(gnu) tools
### directories
___
### files
#### search
1- grep (print lines matching a pattern)
options :
* -L : list not matched files
* -l : list matched files
* -c : count
* -e : regex
* -i : ingore case
* -v : invert match
* -n : show matched line number
* -r : recursive
* -w : word
* -x : line
```
grep -r -n -c -e "[^gnu].*x" .
```
***
2- find (search for files in a directory hierarchy - find is slow)
options:
* -type: (d: directory, f: file)
* -name (-iname : ignore case)
* -o or, -not (!)
* -perm
    * /a=x
    * /u=r
    * 0566
    * sgid (2564)
* -exec
* -user, -group
* -mtime (modified files by day), -atime (accessed files by day)
* -cmin  (changed by minutes), -amin, -mmin
* -size
```bash
find ./test -name 'abc*' ! -name '*.php'
find ./test -iname "*.Php"
find ./test -not -name "*.php"
find -name '*.php' -o -name '*.txt'
find . -type f ! -perm 0777
find /home/bob/dir -type f -name *.log -size +10M -exec rm -f {} \;
```
***
3- locate (find files by name)
- locate does not check whether files found in database still exist. locate can never report files created after the most recent update of the relevant database.
options:
* -n : limit lines
* -c : count
* -i : ignore case
‍‍‍‍‍‍```bash
locate -i *text.txt*
```
* -S : review locate db
* -d : change db path (-d [dbpath])

- update db : updatedb
files:
* /etc/updatedb.conf
* /var/lib/mlocate/mlocate.db

* To create a private mlocate database as an user other than root, run
```bash
updatedb -l 0 -o db_file -U source_directory
```
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
___
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
___
3- wc (count files or standard output)
options:
* -l : lines
* -w : words
* -m : chars
* -c : bytes
```bash
wc -l file1
```
---
4-  tail (last line of files)
-F follows and retry to connect
```bash
tail -F -n 20 /var/log/nginx/access.log
```
---
5- head (output the first part of files)
```bash
head -n 20 /var/log/nginx/access.log
```
---
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
---
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
---
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
