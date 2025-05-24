Recon Notes - Operation Blackbox

Target IP(Metasploitable2 IP): 192.168.6.3

Nmap Summary
Services Detected

Key Vulnerabilities from the Nmap Scan

Port 21

Service vsftpd 2.3.4, Known backdoor vulnerability(CVE-2011-2523)

Port 22

Service SSH, Brute-force possible, no key-only auth

Port 23

Service Telnet, Plain text login, legacy system - potential weak credentials

Port 80

Service HTTP(Apache 2.2.8), Likely hosts DVWA or vulnerable web apps

Port 3306

Service MySQL 5.0, Old version, weak/no auth often configured

Port 8180

Service Tomcat, Apache Tomcat Manager often exposed, likely default creds

Port 5432

Service PostgreSQL, Possible default credentials

Port 139/445

Service Samba, Exploitable via smbclient or Metasploit (e.g., usermap_script)

Port 1524

Service Backdoor Shell, This is literally a root shell waiting to connect to

Port 6667

Service IRC (UnrealIRCd), Known backdoor RCE in certain UnrealIRCd versions

------------------------------------------------------------------------------------------

Web Discovery(Gobuster)

Command Used: gobuster dir -u http://192.168.6.3 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster_output.txt

Outcome: 
Path	Status	Notes
/index	200	Default page
/test/	301	Accessible, possibly dynamic
/twiki/	301	Old wiki software — potential for RCE
/tikiwiki/	301	Another wiki — needs further analysis
/phpinfo	200	Exposes PHP configuration — useful for LFI/RCE
/phpMyAdmin/	301	High-value target, try default creds
/server-status	403	Apache mod_status — blocked, but confirms existence