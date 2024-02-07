Introduction To Information gathering
- information gathering is the first step of any penetration test and involves gathering or collecting information
about an individual

- the more information you have on your target the more successful you will be during the scanning and exploitation phase

information gathering is typically broken down into two types
- Passive information gathering : involves gathering as much information as possible without actively engaging with the target
- Active information gathering : involves gathering as much information as possible by actively engaging with the target


as a penetration tester what information are you looking for ?

Passive information gathering (information u can get)
=
- Identifying IP addresses & DNS information
- Identifying domain names and domain ownership information
- Identifying email addresses and social media profiles
- Identifying subdomains



Active information gathering (information u can get)
=
- Discovering open ports on target machine
- Learning about the internal infrastructure of a target network/organization
- Enumerating information from the target systems

(Tools)

Website Recon & Footprinting (Passive Information Gathering)
host [domain name] ===> kali tool for DNS Lookup utility
- https://domain.com/robots.txt
- https://domain.com/sitemap.xml
- firefox ad- dons or google .. etc >> Builtwith and Wappalyzer 
-  Httrack website copier ===> apt install webttrack



Whois Enumeration (Passive Information Gathering)
- Whosis [domain name]  ===> Kali Tool for Whois Information about Domain
-  https://who.is  ===> Check Whois Information online



Website Footprinting With Netcraft (Passive Information Gathering)
-  https://www.netcraft.com  ===> Checks all Footprinting and Enumeration online



DNS Recon (Passive Information Gathering)

-  dnsrecon -d [domain name]  ===> Kali Tool for DNS Enumeration
Check with "zonetransfer.me" for unsecure dns records
- https://dnsdumpster.com ===> DNS Recon, Find & Lookup DNS



WAF Detection With wafw00f (Passive Information Gathering)
- wafw00f [domain name] ===> Kali Tool Web Application Firewall Detection on a Domian



Subdomain Enumeration With Sublist3r (Passive Information Gathering)
- apt install sublist3r ===> Download the tool and install on Kali Linux
- sublist3r -d [domain name] ===> List subdomains for Domain



Google Dorks (Passive Information Gathering)
- site:[domian name]  ===> Search Results from Specific Domain
- site:[domian name] filetype:pdf  ===> Search PDF Files from Specific Domain
- Google Dorks Cheat Sheets
-  GHDB



Email Harvesting With theHarvester (Passive Information Gathering)
$ theHarvester -d [domain name] -b google ===> Harvest Email Addresses for Domain from Google



Leaked Password Databases (Passive Information Gathering)
-  https://haveibeenpwned.com/



DNS Zone Transfers (Active Information Gathering)
- Use this website "zonetransfer.me" for testing
- https://dnsdumpster.com then search for "zonetransfer.me" domain ==> you will see limited results
- zone transfer must be enabled on the DNS Server in order to make a transfer
-  dnsenum zonetransfer.me ==> Get a full list of information



Host Discovery with Nmap (Active Information Gathering)
$ sudo nmap -sn 192.168.43.0/24
$ sudo apt install netdiscover -y
$ sudo netdiscover -i eth0 192.168.43.0/24 --> Discovery by sending ARP Requests to IPs



Port Scanning with Nmap (Active Information Gathering)
--
$ nmap 192.168.43.1 ==> SYN Scan on the most common 1000 ports

$ nmap -Pn 192.168.43.1 ==> SYN Scan on the most common 1000 ports in case Windows Firewall blocks ICMP

$ Follow the cheat sheet file for more examples



Lab01 Windows Recon: Nmap Host Discovery Solution
Step 1: Checking the target IP address
$ cat /root/Desktop/target

Step 2: Ping the target machine to see if it’s alive or not.
$ ping -c 5 IP

Step 3: Run a Nmap scan against the target IP.
$ nmap IP

Step 4: Running Nmap using the -Pn option to discover all alive ports.
$ nmap -Pn IP

you could specify the port by typing its number for example..
$ nmap -Pn -p 443 IP

Step 5 we could use option -sV and this option is used to determine the application version information
$ nmap -Pn -sV -p 80 IP

to scan all the ports use option -p-
$ nmap -Pn -sV -p- IP