### Windows Recon: SMB Nmap Scripts
	ping  ip
	nmap ip
		discover supported protocols using nmap scripts
	- nmap -p445 --script smb-protocols ip
		run security mode script reuturn information about smb security level
	- nmap -p445 --script smb-security-mode ip
		run sessions script to discover live sessions 
	- nmap -p445 --script smb-enum-sessions ip
		run sessions script to discover live sessions with login credentials
	- nmap -p445 --script smb-enum-sessions --script-args smbusername=adminsitrator,smbpassword=smbserver_771 ip 
		enumerate all available shares
	- nmap -p445 --script smb-enum-shares ip
	- nmap -p445 --script smb-enum-shares --script-args smbusername=administrator,smbpassword=smbserver_771
		enumerate all users
	- nmap -p445 --script smb-enum-users ip
	- nmap -p445 --script smb-enum-users --script-args smbusername=administrator,smbpassword=smbserver_771 ip
		enumerate server stats
	- nmap -p445 --script smb-server-stats --script-args smbusername=administrator,smbpassword=smbserver_771 ip
		enumerate all available domains
	- nmap -p445 --script smb-server-stats --script-args smbusername=administrator,smbpassword=smbserver_771 ip
		enumerate available users group 
	- nmap -p445 --script smb-enum-groups --script-args smbusername=administrator,smbpassword=smbserver_771 ip
		enumerate services 
	- nmap -p445 --script smb-enum-services --script-args smbusername=administrator,smbpassword=smbserver_771 ip
		enumerate shared folders and drives
	- nmap -p445 --script smb-enum-shares,smb-ls --script-args smbusername=administrator, smbpassword=smbserver_771 ip



### Windows Recon: SMBMAP
smbmap used to enumerate the target with all possible shares, sessions, protocols, etc
commands
- ping ip
- nmap ip
- smbmap -u guest -p ""  -d . -H ip (could be anonymous login allowed)
- smbmap -u administrator -p smbserver_771 -d . -H ip
- smbmap -u administrator -p smbserver_771 -H ip -x 'ipconfig'
- smbmap -u administrator -p smbserver_771 -H ip -r 'C$' (to show all available files and folders on c target machine)
- touch backdoor
- smbmap -u administrator -p smbserver_771 -H ip --upload '/root/backdoor'   'C$\backdoor'
- smbmap -u administrator -p smbserver_771 -H ip -r 'C$'
- smbmap -u administrator -p smbserver_771 -H ip --download 'C$\\flag.txt'
- cat flag.txt



### Samba Recon Basics
		1-Find the default tcp ports used by smbd
	- nmap  ip
		2- Find the default udp ports used by smbd
	- nmap -sU --top-ports 25 ip
		3- What is the workgroup name of samba server
	- nmap -p445 -sV ip 
		4- Find the exact version of samba server using nmap script
	- nmap -p445 --script smb-os-discovery ip
		5- Find the exact version of samba server using smb_version msf
	- msfconsole
	- use auxiliary/scanner/smb/smb_version
	- set RHOSTS ip
	- exploit
		6- What is the NetBIOS computer name of smaba server using nmblookup
	- nmblookup -A ip
		7- what is the netBIOS computer name of samba using nmap
	- nmap -p445 --script smb-os-discovery ip
		8- using smbclient to determine whether anonymous connection is allowed or not
	- smbclient -L ip -N
		9- use rpcclient to determine if anonymous connection alllowed or not
	- rpcclient -U "" -N ip



### Samba Recon Basics II
	Find the OS version of samba server using rpcclient
		rpcclient -U "" -N ip
		srvinfo
	Find the OS version of samba server using enum4linux
		enum4linux -o ip
	Find the server description of samba server using smbclient
		smbclient -L ip -N
	is NT LM 0.12 SMBv1 dialects supported by the samba server? use appropriate nmap script
		nmap -p445 --script smb-protocols ip
	is SMB2 protocol supported by the samba server use smb2 metasploit module 
		msfconsole
		use auxiliary/scanner/smb/smb2
		set RHOSTS ip
		exploit
	List all users that exists on the samba using nmap script
		nmap --script smb-enum-users -p445 ip
	List all users that exists on the samba server using smb_enumusers metasploit
		msfconsole
		use auxiliary/scanner/smb/smb_enumusers
		set RHOSTS ip
		exploit
	List all users that exists on the samba server using enum4linux
		enum4linux -U ip
	List all users that exists on tha samba server using rpcclient
		rpcclient -U "" -N ip
		enumdomusers
	Find SID of user "admin" using rpcclient
		rpcclient -U "" -N ip
		lookupnames admin



### Samba Recon: Basics III
	get all available shares with nmap scripts
		nmap -p445 --script smb-enum-shares ip 
	get all available shares with smb_enumshares metasploit
		msfconsole
		use auxiliary/scanner/smb/smb_enumshares
		set RHOSTS ip
		exploit
	List all available on samba server using enum4linux
		enum4linux -S ip
	List all available shares on samba using smbclient
		smbclient -L ip -N
	find domain groups that exists on samba server with enum4linux
		enum4linux -G ip
	find domain groups that exists on samba server with rpcclient
		rpcclient -U "" -N ip
		enumdomgroups
	how many directories are present inside share public
		smbclient //ip/public -N
		ls
		cd secret
		get flag
		exit
		cat flag

### Samba Recon: Dictionary Attack

what is the password of user jane required to access share jane
	- msfconsole
	- use auxiliary/scanner/smb/smb_login
	- set PASS_FILE /usr/share/wordlists/metasploit/unix_passwords.txt
	- set SMBUser jane
	- set RHOSTS ip
	- exploit
what is the password of user admin to access share admin using hydra with password wordlists 
	- gzip -d /usr/share/wordlists/rockyou.txt.gz
	- hydra -l admin -P /usr/share/wordlists/rockyou.txt ip smb
which share is read only use smbmap with credentials 
	- smbmap -H ip -u admin -p password1
u could browse jane shares
	- smbclient -L ip -U jane
get the flag from share admin
	- smbclient //ip/admin -U admin
	- ls 
	- cd hidden
	- ls
	- get flag.tar.gz
	- exit
	- tar -xf flag.tar.gz
	- cat flag
u can list the pipes available on smb server using msfconsole
	- msfconsole
	- use auxiliary/scanner/smb/pipe_auditor
	- set SMBUser admin
	- set SMBPass password1
	- set RHOSTS ip
	- exploit
list sid of unix users 
	- enum4linux -r -u "admin" -p "password1" ip


### ProFTP Recon: Basics
what is the version of ftp server
	nmap -sV ip
use hydra to get the username and password
	hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ip -t ftp
find the password of user sysadmin using nmap
	echo "sysadmin" > users
	nmap --script ftp-brute --script-args userdb=/root/users -p21 ip
login to ftp server with these login credentials
	ftp ip
	ls 
	get secret.txt
	exit
	cat secret.txt

### FTP Anonymous Login VSFTPD Recon: basics
find the version of vsftpd server
	nmap -sV ip
check if the ftp allow anonymous login
	nmap --script ftp-anon ip
	ftp ip 21
	ls
	get flag
	cat flag



### SSH Recon Basics
find the version of ssh server
	nmap -sV ip
fetch the banner of ssh server
	nc ip 22
login with ssh (see if anonymous login allowed)
	ssh root@ip
u could see how many encryption algorithms are supported by ssh server 
	nmap -p22 --script ssh2-enum-algos ip
u can see what is ssh-rsa could by using this  script
	nmap -p22 --script ssh-hostkey --script-args ssh_hostkey=full ip
authentication methods
	nmap -p22 --script ssh-auth-methods --script-args="ssh.user=student" ip
	nmap -p22 --script ssh-auth-methods --script-args="ssh.user=admin" ip
get the flag from /home/student/flag using nmap
	nmap -p 22 --script=ssh-run --script-args="ssh-run.cmd=cat /home/student/FLAG,
ssh-run.username=student,ssh-run.password=" 192.201.39.3


### SSH Recon: Dictionary Attack
(bruteforce with hydra)
	gzip -d /usr/share/wordlists/rockyou.txt.gz
	hydra -l student -P /usr/share/wordlists/rockyou.txt ip ssh
(bruteforce with nmap)
	echo "administrator" > users
	nmap -p22 --script ssh-brute --script-args userdb=/root/users ip
(bruteforce with msfconsole)
	msfconsole
	use auxiliary/scanner/ssh/ssh_login
	set RHOST ip
	set USERPASS_FILE /usr/share/wordlists/metasploit/root_userpass.txt
	set STOP_ON_SUCCESS true
	set verbose true
	exploit
login with ssh root
	ssh root@ip 


### Windows Recon: IIS
	scan the target 
		nmap ip
	use whatweb tool to get all possible information about the target
		whatweb ip
	gether information about index page of html
		http ip
	browse and show the default page of the website
		browsh --startup-url http://ip/default.aspx


### Windows Recon IIS Nmap Scripts

- nmap ip

enumerate the http and discover interesting directories
	nmap --script http-enum -sV -p 80 ip
run header script to get the iis server header information
	nmap --script http-headers -sV -p80 ip

	nmap --script http-methods --script-args http-methods.url-path=/webdav/ip
	nmap --script http-webdav-scan --script-args http-methods.url-path=/webdav/ip


### Apache Recon Basics
	msfconsole
	use auxiliary/scanner/http/http_version
	set RHOSTS ip
	exploit
	curl http://ip
	wget "http://ip/index"
	browsh --startup-url ip
	lynx http://ip
	msfconsole
	use auxiliary/scanner/http/brute_dirs
	set RHOSTS ip
	exploit
	dirb http://ip /usr/share/metasploit-framework/data/wordlists/directory.txt
	use auxiliary/scanner/http/robots_txt
	set RHOSTS ip
	exploit

## ("SQL Enumeration in another file")
