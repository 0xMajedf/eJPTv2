Course Topic overview
=
- introduction to system/host based attacks
- overview of windows vulnerabilities
- exploiting windows vulnerabilities
- windows privilege escalation
- windows file system vulnerabilities
- windows credential dumping
- windows lateral movement
- overview of Linux vulnerabilities
- exploiting Linux vulnerabilities
- Linux Privilege escalation 
- Linux file system vulnerabilities
- Linux credential dumping

Introduction to System/Host Based attacks
=
- System host based attacks are attacks that are targeted towards a specific system or host running a specific operating system for example windows or Linux

- Network services are not the only attack vector that can be targeted during penetration test

- System/Host Based attacks usually come in to play after you have gained access to a target network whereby you will be required to exploit servers workstations or laptops on the internal network

- System host based attacks are primarily focused on exploiting inherent vulnerabilities on the targeted OS
	could exploit some vulnerabilities on OS 
	-misconfiguration
	-Winrm
	-SMB
	-RDP
	-IIS
	in this course it is gonna be primarily focusing on windows and Linux vulnerabilities and how they can be exploited


- after u exploit these vulnerabilities on this services (gaining access) then u will go through privilege escalation

Overview of Windows Vulnerabilities
=
Microsoft windows has various OS versions and releases which makes the threat surface fragmented in terms of vulnerabilities for example vulnerabilities that exists in windows 7 are not present in windows 10
- WINDOWS OS have been developed in the C programming language making them vulnerable to buffer overflow arbitrary code execution etc...

- newly discovered vulnerabilities are not immediately patched by Microsoft and given the fragmented nature of windows many systems are left unpatched 


	the frequent releases of new versions of windows is also contributing factor to exploiting as many companies takes subtential length of time to upgrade their systems to the latest version of windows

Types of Windows Vulnerabilities
=
- Information disclosure - vulnerability that allow an attacker to access confidential data
- buffer overflow - caused by a programming error allow attackers to write data to a buffer and overrun the allocated buffer consequently writing data to allocated memory addresses
- remote code execution - vulnerability that allow an attacker to remotely execute code on the target system
- Privilege Escalation - vulnerability that allow an attacker to elevate their privileges after initial compromise
- Denial of Service (DOS) - vulnerability that allow an attacker to consume a system host resources (CPU, RAM, Network ETC consequently  preventing the system from functioning normally


Frequently exploited Windows services
=

	these services provide an attacker with an access vector that they can utilize to gain access to a target host


- Microsoft windows has various native services and protocols that can be configured to run on a host

	1-Micorosoft IIS (internet information services) - 80/443 proprietary/web server software developed by Microsoft that runs on windows
	2- WebDAV (web distributed authoring versioning) - 80/443 HTTP extension that allow clients to update, delete, move and copy files on web server WebDAV is used to enable web server to act as a file server
	3-SMB/CIFS (Server message block protocol) - 445 network file sharing protocol that is used to facilitate the sharing of files and peripherals between computers on a local network (LAN)
	4-RDP (Remote Desktop protocol) - 3389 Proprietary GUI remote access protocol developed by Microsoft and is used to remotely authenticate and interact with a windows system
	5-WinRM (Windows Remote Management) - 5986/443 windows remote management protocol that remote access with windows systems

### Exploiting Windows Vulnerabilities

Microsoft IIS (internet information services) is a proprietary extensible web server software developed by Microsoft for use with the windows NT family

supported executable file extension
- .asp
- .aspx
- .config
- .php

WebDAV (web based distributed authoring and versioning) runs on top Microsoft IIS on port 80/443

- in order to connect to a WebDAV WebDAV server you will need to provide legitimate credentials this is because WebDAV implements authentication in the form of a username and password

- the first step of the exploitation process will involve identifying whether WebDAV has been configured to run on the IIS web server

Tools
=
- davtest - used to scan authenticate and exploit a WebDAV server : pre-installed on most offensive penetration testing distribution like kali linux

- cadaver - cadaver supports file upload download on-screen display in place editing operations (move/copy)

### Exploiting SMB with PsExec

SMB (server message block) is a network file sharing protocol that is used to facilitate the sharing of files and peripherals 

SMB uses port 445 (TCP) however originally SMB ran on top of NetBIOS using port 139

SMB Authentication 
=
- The SMB protocol utilizes two level of authentication namely:
- user authentication
- share authentication

	User authentication - users must provide a username and password in order to authenticate with the SMB server in order to access a share

	Share authentication - users must provide a password in order to access restricted share

Client (authentication request) > server
server (encrypt string with user's hash) > client
client (Encrypted string) > server
server (access granted) > client

PsExec is a lightweight telent-replacement developed by microsoft that allows you execute processes on remote windows systems using any user's credentials
- PsExec authentication is performed via SMB

- it is very similar to RDP however instead of controlling the remote system via GUI commands are sent via CMD

$$SMB Exploitation with PsExec
in order to utilize Psexec to gain access to a windows target we will need to identify legitmate user accounts and their respective passwords or password hashes

$$
the most common technique will involve performing an SMB login brute-force attack


### Exploiting RDP (Remote Desktop protocol)
is used to remotely connect and interact with a windows system

### Exploiting Windows Remote Management (WinRM) 
is a Windows remote management protocol that can be used to facilitate remote access with windows systems over https
- microsoft implemented WinRM in to Windows in order to make life easier for system administrator

	WinRM is typically used in the following ways
	- remotely access and interact with windows hosts on a local network
	- remotely access and execute commands on windows systems 
	- manage and configure windows systems remotely



