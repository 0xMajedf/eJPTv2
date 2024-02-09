# Windows Recon: Nmap Host Discovery
	Commands

- ping -c 5 IP (probably the firewall will block you)
- nmap IP (''Host Seems Down'' Lets try -Pn)
- nmap -Pn IP (will show u some open ports )
- nmap -Pn -p 443 IP
- nmap -Pn -sV -p 80 (-sV version service scan)


# Scan the server 1

	ip addr
	ping ip
	nmap ip
	nmap -sV -O -p- -T4 ip
	nmap ip -p 6421,41288,55413 -sV


 # Scan the server 2


	ip addr
	ping ip
	nmap  -A ip
	nmap ip -p 1-250 -sU
	nmap ip -p134,177,234 -sUV
	nmap ip -p134 -sUV --script=discovery
	tftp ip 134

# Scan the server 3

	ip addr
	nmap ip -T4 -p-
	nmap ip -T4 -sU
	nmap ip -T4 -sU -p 161 -A


# Windows Recon: SMB Nmap Scripts

	Checking the target IP address.
	cat /root/Desktop/target
	ping -c 5 10.0.17.200

- nmap ip

- nmap -p445 --script smb-protocols ip

- nmap -p445 --script smb-security-mode ip

- nmap -p445 --script smb-enum-sessions ip

- nmap -p445 --script smb-enum-sessions --script-args
smbusername=administrator,smbpassword=smbserver_771 ip

- nmap -p445 --script smb-enum-shares ip

-  nmap -p445 --script smb-enum-shares --script-args
smbusername=administrator,smbpassword=smbserver_771 ip

- nmap -p445 --script smb-enum-users --script-args
smbusername=administrator,smbpassword=smbserver_771 ip

- nmap -p445 --script smb-server-stats --script-args
smbusername=administrator,smbpassword=smbserver_771 ip

- nmap -p445 --script smb-enum-domains --script-args
smbusername=administrator,smbpassword=smbserver_771 ip

 - nmap -p445 --script smb-enum-groups --script-args
smbusername=administrator,smbpassword=smbserver_771 ip

- nmap -p445 --script smb-enum-services --script-args
smbusername=administrator,smbpassword=smbserver_771 ip

 - nmap -p445 --script smb-enum-shares,smb-ls --script-args
smbusername=administrator,smbpassword=smbserver_771 ip