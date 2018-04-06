# Table of Contents:
* [file and directory manipulation](#file-and-directory-manipulation)
   * [directories](#directories)
      * [general](#general)
      * [disk usage and information](#disk-usage-and-information)
      * [compress and extract](#compress-and-extract)
   * [files](#files)
     * [search](#search)
     * [manipulate](#manipulate)
     * [read](#read)
___
# file and directory manipulation
## general
### permisions :
* Srwxrwxrwx
r = 4
w = 2
x = 1
S = sticky bit, define directory or file

```bash
chown -R www-data:www-data /var/www
chmod -R 755 test/ # uga+rwx, u=wg
chmod -R 6711 test/ # setgid and setuid bit
```
#### create dir, mv, cp, touch
```bash
mkdir -p  1/2/3 # create: p stands for parents
rm -rf 1 #remove(dir and file): r stands for recursive, f stands for force
mv 1 p/o/ # move file and directory`
mv 1 p # rename file directory
cp -r -v 1/  o/ # copy
touch touched.txt # change file access time , if file does not exist will make it.
```
### directories
#### disk usage and information
##### df (report file system disk space usage)
* options :
  * -h : print sizes in human readable format (e.g., 1K 234M 2G)
  * -H : likewise, but use powers of 1000 not 1024
  * -i : list inodes
```bash
df -h
```
##### du (estimate file space usage)
* options :
  * -h : print sizes in human readable format (e.g., 1K 234M 2G)
  * -H : likewise, but use powers of 1000 not 1024
  * -c : total
  * -s : summerize
  * --max-depth
  * --time
```bash
du -h -c /home/arvin/
```
#### compress and extract
##### tar
* options :
  * -v : verbose
  * -x : extract
  * -f : file
  * -z : gzip, -j : bzip2
  * -c : create, -r : append, --delete : delete from archive
```bash
tar -cvfj test.tar.bz /home 1.txt 2.txt
tar -xvf test.tar.bz
tar -rf test.tar.bz 3.txt
```
___
### files
#### search
##### grep (print lines matching a pattern)
* options :
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
##### find (search for files in a directory hierarchy - find is slow)
* options:
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
##### locate (find files by name)
- locate does not check whether files found in database still exist. locate can never report files created after the most recent update of the relevant database.
* options:
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
/etc/updatedb.conf
/var/lib/mlocate/mlocate.db

To create a private mlocate database as an user other than root, run
```bash
updatedb -l 0 -o db_file -U source_directory
```
___
#### manipulate
* copy and convert files
1-  dd
options:

2- iconv

3- sed
___
#### read
##### cat (concatenate files and print on the standard output,Copy standard input to standard output.)
* options:
  * -n : number all output lines
  * -A : equivalent to -vET
  * -E : display $ at end of each line, -e:-vE
  * -T : display TAB characters as ^I, -t: -vT
  * -v : use ^ and M- notation, except for LFD and TAB
```bash
cat test test1
cat -n test
```
#####  more, less, sort
  1- more (file perusal filter for crt viewing), less (opposite of more)
  ```bash
  more file1
  tail -F /var/etc/nginx/access.logs | more
  ```
  2- sort (sort lines of text files)
    * options:
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
#####  wc (count files or standard output)
* options:
  * -l : lines
  * -w : words
  * -m : chars
  * -c : bytes
```bash
wc -l file1
```
##### tail (last line of files)
* options :
  * -F : follows and retry to connect ( -f --retry)
```bash
tail -F -n 20 /var/log/nginx/access.log
```
##### head (output the first part of files)
```bash
head -n 20 /var/log/nginx/access.log
```
##### split (split a file into pieces)
* options
 * -l : customize line (ex -l1200)
 * -b : file size (ex: -b 500M)
 * -n : Generate n chunks output files (ex -n5)
 * -e : Prevent Zero Size Split output files
in below example ~ngx~ is prefix of generated file
```bash
split /var/log/nginx/access.log ngx
```
merge splited files
```bash
cat Split_IS0_a* > Windows_Server.iso
```
##### join (join lines of two files on a common field)
* options
  * -1 (field of first file)
  * -2 (field of second file)
  ```bash
  join -1 2 -2 1 wine.txt reviews.txt
  ```
  sorted
  ```bash
  join -1 2 -2 1 <(sort -k 2 wine.txt) <(sort reviews.txt)
  ```

  * -t (specify a field separator for joining)
  ```bash
  join -1 2 -2 3 -t , names.csv transactions.csv
  ```
---
##### paste (paste - merge lines of files)
```bash
paste file1 file2
```
