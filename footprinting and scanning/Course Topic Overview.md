
- introduction to Network Mapping
- Networking Fundamentals
- Host Discovery with nmap
- Port Scanning with nmap
- Host Fingerprinting with nmap
- Introduction to The nmap Scripting Engine (NSE)
- Firewall Detection & Evasion with nmap
- nmap Scan Timing & perfomance
- nmap output and verbosity

Lessons

(Active information gathering)

Penetration testing methodology

- information gathering ===> passive and active
- Enumeration ===> Service and OS enumeration
- Exploitation ===> Vulnerability analysis and threat modeling (developing and modifying exploits and service exploitation)
- Post Exploitation ===> (Local enumeration, privilege escalation, credential access , persistence, defense evasion, lateral movement)
- Reporting ===> Report Writing , Recommendations


(Networking Primer)

Network Fundamentals

Network protocols
	ensure that different computer systems using different hardware and software can communicate with each other


Packets
	packets are nothing but streams of bits running as electric signals on physical media used for data transmission (Ethernet, Wi-Fi etc) 
	 every packet in every protocol has the following structure
	- header
	- payload


the header has a protocol specific structure this ensure that the receiving host can correctly interpret the payload and handle the overall communication

the payload is the actual information being sent it could do something like part of an email message or the content of a file during a download

The OSI Model
	 is divided into seven layers each presenting a specific functionality in the process of network communication
	 the osi model serves as a guideline for developing and understanding network protocols and communication processes

- Application (http, ftp, irc, ssh, dns)
- presentation (ssl/tls, jpeg, gif, ssh, imap)
- session (apis, netbios, rpc)
- transport (TCP, UDP)
- network (IP, ICMP, IPSec)
- data link (Ethernet, PPP, Switches etc)
- Physical (USB ethernet cables, coax fiber, hubs etc)


Network Layer is responsible for logical addressing routing and forwarding data packets between devices across different networks 

the primary goal is to determine the optimal path for data to travel from the source to the destination even if the devices are  no separate networks

	network layer protocols
	ipv4 the most widely used version of IP employing 32-bit addresses and providing the foundation for communication on the internet
	ipv6 developed to address the limitation of ipv4 it uses 128-bit addressess and offers an exponentiallly larger address space

icmp (internet control message protocol) used for error reporting and diagnostics ICMP message include ping (echo request and echo reply) and various error messages

IP : the internet protocol is a central protocol in the suite of protocols that form the foundation of the internet

IP functionality
	allows for the fragmentation of large packets into smaller fragments when traversing networks with varying maximum transmission unit mtu sizes

ip addresses types can be classified into three types unicast (one to one communication)
	broadcast (one to all communication within a subnet) and multicast (one to many communication) 
	subnetting : is a technique that divides a large ip network into smaller more manageable sub network it enchases network efficiency and security

DHCP : used in conjunction with ip to dynamically assign ip addresses to devices on a network simplifying the process of network configuration


ipv4 header field
- version
- header length 
- type of service
- total length
- identification
- flags
- time to live
- source ip address
- destination ip address

reserved ipv4 addresses
- 0.0.0.0-0.255.255.255 representing this network
- 127.0.0.0-127.255,255,255 representing the local host you computer
- 192.168.0.0-192.168.255.255 is reserved for private networks

Transport Layer
this layer ensures reliable end to end communication handling tasks such as error detection flow control and segmentation of data into smaller units

(TCP is for reliable Connection  care for data delivered , UDP is for fast connection and for streaming services doesn't care about data delivered or not)

tcp port range
(Well known ports 0-1023)
80 http hyper text transfer protocol
443 https hyper text transfer protocol secure
21 ftp file transfer protocol
22 ssh secure shell
25 smtp simple mail transfer protocol
110 pop3 post office protocol version 3

(Registered ports 1024-49151) 
3389 rdp remote desktop protocol
3306 mysql database
8080 http alternative port
27017 mongodb database

	cmd (kali linux)
	netstat -antp (to see acitve tcp connection or ports open) like for example if you open firefox the packets information will be shown in the terminal
commands:
- netstat -antp

Host Discovery 

Network Mapping
	how can a penetration tester determine what hosts within an in-scope network are online ? what ports are open on the active hosts and what operating system are running on the active hosts ? answer - Network Mapping

network mapping : the process of discovering and identifying devices hosts and network infrastructure elements within a target network

- discovery of live hosts
- identification of open ports and services
- network topology mapping
- Operating System fingerprinting
- Service Version Detection
- identifying filtering and security measures (discovering firewalls intrusion prevention systems and other security measures)

nmap functionality 
- Host discovery 
- port scanning
- service version detection
- operating system fingerprinting

Host Discovery techniques
- ping sweeps icmp ehco requests sending icmp echo requests ping to range of ip addresses to identify live hosts this is a quick and commonly used method
- arp scanning
- tcp syn ping
- udp ping
- tcp-ack ping
- syn-ack ping 

ICMP ping
- pros : icmp ping is widely used and quick method for identifying live hosts
- cons : some companies of organization configured their firewalls to block ICMP traffic icmp can also be easily detected

TCP-syn ping
- pros : tcp syn ping is stealther than icmp and may bypass firewalls that allow outbound connection
- cons : some hosts may not respons to tcp syn requests and the result can be affected by firewall and security devices


Ping sweeps
	is a network scanning technique used to discover live hosts computer servers other devices within a specific ip address range on a network

	ping www.site.test
	the icmp echo request will not receive an echo reply if the host is offline
	doesn`t really necessarily meant that the host is offline it could be the server blocking icmp request by configure the firewall settings

Host Discovery with nmap Part 1

- nmap -h
- nmap -sn IP/24
- sudo wireshark -i eth1
- nmap -sn IP/24 --send-ip
- nmap -sn IP and another IP


Host Discovery with nmap Part 2
- nano targets.txt (in the target txt u can put all the IPs u want to scan)
- nmap -sn -iL /root/Desktop/targets.txt
- nmap -sn -PS1-1000 IP (PS to specify the tcp syn ping option )
- nmap -PS80,443,3389 IP 
- nmap -sn -PA IP (tcp ack ping)
- nmap -sn -PE IP (icmp echo request u can specify what port you want to scan)


Port Scanning with nmap part 1
- nmap -Pn ip
- nmap -Pn -F ip (fast port scan)
- nmap -Pn -p80 ip
- nmap -Pn -T4 -p- (timing performance)

Port Scanning with nmap part 2
- nmap -Pn -sS -F (syn scan with fast port scan and no ping)
- nmap -Pn -sT ip (tcp scan more reliable and accurate)
- nmap -sU (udp scan)


Service Version and OS Detection

	commands
	nmap -sn ip/24
	nmap -sS ip
	nmap -T4 -sS -p- ip
	nmap -T4 -sS -sV -p- ip
	nmap -T4 -sS -sV -O -p- ip
	nmap -T4 -sS -sV -O --osscan-guess -p- ip
	nmap -T4 -sS  -sV --version-intensity -O --osscan-guess -p- ip
	nmap -T4 -sS  -sV --version-intensity 8 -O --osscan-guess -p- ip




Nmap Scripting Engine (NSE)

	commands
	nmap -sS -sV -O -p- ip
	ls -al /usr/share/nmap/scripts
	ls -la /usr/share/nmap/scripts | grep -e "http"
	nmap -sS -sV -sC -p- -T4 ip
	ls -la /usr/share/nmap/scripts | grep -e "mongodb"
	nmap --script-help=http-info 
	nmap -sS -sV --script=mongodb-info -p- -T4 ip
	ls -la /usr/share/nmap/scripts/ | grep -e "memcached"
	nmap -sS -sV --script=memcached-info -T4 -p- ip
	ls -la /usr/share/nmap/scripts/ | grep -e "ftp"
	nmap -sS -sV --script=memcached-info,ftp-anon -p- -T4 ip
	nmap -sS -sV --script=ftp-anon -T4 ip
	nmap -sS -sV --script=ftp-* -T4 ip
	nmap -sS -sV --script=ftp-syst -p- -T4 ip


Firewall Detection and IDS Evasion

	nmap -Pn -sS -F ip
	nmap -Pn -sA -p445,3389 ip


Scan Timing Performance

	nmap -Pn -sS -F --host-timeout 5s ip/24
	nmap -sS -sV -F --scan-delay 5s ip 
	nmap -sS -sV -Pn (-T1 to -T5) ip


Output and Verbosity after getting the result of scan (how u can save it)

	-oN/-oX/-oS/-oG <file> : output scan in normal, XML, script kiddie, grepable format
	nmap -Pn -sS -F -T4 ip -oN normal.txt
	nmap -Pn -sS -F -T4 ip -oX xml.xml
	service postgresql start
	msfconsole
	workspace -a pentest
	workspace db_status
	db_import xml.xml
	hosts
	services
	db_nmap -Pn -sS -O -sV -p80 ip
	nmap -Pn -sS -F -T4 ip -oG grep.txt
	