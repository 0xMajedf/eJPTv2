Course Topic Overview
=
- introduction to The Metasploit Framework
- Metasploit Fundamentals
- Information Gathering & Enumeration with MSF
- Vulnerability Scanning with MSF
- Client-Side Attacks with MSF
- Host and service based exploitation with MSF
- Post Exploitation with MSF
- Metasploit GUIs (Graphical User Interface)

### Metasploit

The Metasploit Framework (MSF) is an open source robust penetration testing and exploitation framework that is used by penetration testers and security researchers worldwide

- it provides penetration testers with a robust infrastructure required to automate every stage of the penetration testing life cycle
- it is also used  to develop and test exploits and has one of the world's largest database of public tested  exploits

Essential Terminology

- interface  - methods of interfacing with the Metasploit Framework
- Module - Pieces of code that perform a particular task an example of a module is an exploit
- Vulnerability - weakness or flow in a computer system or network that can be exploited
- Exploit - Piece of code/module that is used to take advantage a vulnerability within a system service or application
- Payload - Piece of code delivered to the target system by an exploit with the objective of executing 
- Listener - A utility that listens for an incoming connection from a target


### Metasploit Framework Architecture


- module in the context of MSF is a piece of code that can be utilized by the MSF
INTERFACES | msfconsole, msfcii, armitage, web
LIBARIES | rex > Tools, MSF core, MSF base> Plugins
MODULES | exploit, payload, encoder, nop, auxiliary

### MSF Modules

- Exploit - module that is used to take advantage of vulnerability and is typically paired with a payload 
- Payload - Code that is delivered by MSF and remotely executed on the target after successful exploitation
- Encoder - Used to encode payloads in order to avoid AV detection for example shikata_ga_nai used to encode windows payloads
- NOPS - used to ensure that payloads sizes are consistent and ensure the stability of a payload when executed
- Auxiliary - module that is used to perform additional functionality like port scanning and enumeration 

meterpreter payload is an advanced multi-functional payload the is executed in memory on the target system making it difficult to detect

MSF stored module under the following directory on Linux systems:
	 /usr/share/metasploit-framework/modules
User specified modules are stored under the following directory on Linux Systems;
	~/.ms4/modules

Penetration Testing with the Metasploit Framework
- msf can be used to perform and automate various tasks that fall under the penetration testing life cycle
Penetration testin phases
- information gathering >> auxiliary modules
- enumeration >> auxiliary modules
- exploitation >> exploit modules , payloads
	- post-exploitation - privilege escalation, maintaining persistent access, clearing tracks > post exploitation modules meterpreter, persistence


### Metasploit Fundamentals
database msfdb is an integral part of the Metasploit framework and is used to keep track of all your assessments host data scans etc 

the metasploit framework uses PostgreSQL as the priamry database server as a result as will also need to ensure that the postgresql database server is running and configured correctly

the metasploit framework database also facilitates the importation and storage of scan results from various third party tools like Nmap and Nessus

- sudo apt-get update && apt-get install metasploit-framework -y
- sudo systemctl status postgresql 
- sudo msfdb
- sudo msfdb init

### MSFconsole Fundamentals

What You need to Know
- how to search for modules
- how to select modules
- how to configure module options and variables
- how to search for payloads
- managing sessions
- additional functionality
- saving your configuration

Variables
	LHOST ur local host ip
	LPORT ur port
	RHOST ip of the target 
	RHOSTS ip of the targets
	RPORT port of the target

	msfconsole
	versions
	show exploits
	show -h 
	use auxiliary/scanne/portscan/tcp
	show options
	set RHOSTS ip
	back
	search -h
	search cve:2017 type:exploit platform:windows
	search eternalblue
	use exploit/windows/smb/ms17_010_eternalblue
	set RHOSTS ip
	show options
	exploit
	back
	sessions
	connect -h
	connect ip


### Creating and Managing Workspaces
workspace allow you to keep track of all your hosts scans and activates and are extremely useful when conducting penetration tests as they allow you to sort and organize your data based on the target or organization MSFconsole provide you with the ability to create, manage and switch between multiple workspaces depending on your requirements

	db_status
	workspace -h
	hosts
	workspace -a Test
	hosts
	workspace default
	workspace -a INE
	workspace Test
	workspace default
	workspace -d Test
	workspace -h
	workspace -r INE PTA
	


### Nmap with Metasploit (Information Gathering and Enumeration)

Port Scanning and Enumeration with Nmap
Nmap if a free and open source network scanner that can be used to discover hosts on a network as well as scan targets for open ports

- it can also be used to enumerate the services running on open ports as well as the operating system running on the target system
- we can output the result of our nmap scan in to a format that can be imported into MSF for vulnerability detection and exploitation

nmap -Pn -sV -O ip -oX windows_server_2012

### Importing Nmap Scan results into MSF
	service postgresql start
	msfconsole
	db_status
	workspace -a Win2k12
	db_import /root/windows_server_2012
	hosts
	workspace -a Nmap_MSF
	db_nmap -Pn -sV -O ip
	vulns

### Importing Nmap Scan Results into MSF Lab
	service postgresql start
	msfconsole
	db_status
	db_import /root/windows_server_2012
	hosts
	services

### Web server Enumeration
	ifconfig
	service postgresql start
	msfconsole
	setg RHOSTS ip
	search http
	search type:auxiliary name:http
	use auxiliary/scanner/http/http_version
	show options
	exploit
	search http_header
	use auxiliary/scanner/http/http_header
	exploit
	use auxiliary/scanner/http/robots.txt
	exploit
	use auxiliary/scanner/http/http_header
	set TARGETURl /secure
	exploit
	use auxiliary/scanner/http/brute_dirs
	exploit
	use auxiliary/scanner/http/dir_scanner
	set DICTIONARY /usr/share/metasploit-framework/data/wordlists/directory.txt
	exploit
	use auxiliary/scanner/http/dir_listing
	set PATH /data
	exploit
	use auxiliary/scanner/http/files_dir
	set VERBOSE false
	exploit
	use auxiliary/scanner/http/http_put
	set PATH /data
	set FILENAME test.txt
	set FILEDATA "Welcome To AttackDefense"
	exploit
	use auxiliary/scanner/http/http_put
	set PATH /data
	set FILENAME test.txt
	set ACTION DELETE
	exploit
	use auxiliary/scanner/http/http_login
	set AUTH_URl /secure/
	set VERBOSE false
	exploit
	use auxiliary/scanner/http/apache_userdir_enum
	set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
	set VERBOSE false
	exploit







