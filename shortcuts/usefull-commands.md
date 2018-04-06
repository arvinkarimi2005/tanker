##### copy file to clipboard
```bash
xclip -selection clipboard .ssh/id_rsa.pub
```
##### reconfigure keyboard layout
```bash
sudo dpkg-reconfigure keyboard-configuration
```
##### tee
Copy standard input to each FILE, and also to standard output.
-a appends to file exisists
```bash
ls -a | tee output.file
```
