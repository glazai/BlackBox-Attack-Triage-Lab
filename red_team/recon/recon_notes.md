#  Recon Notes – Operation BlackBox (Phase 1)

## 🔍 Target Information
- **Target IP**: 192.168.6.3
- **Recon Date**: May 10, 2025
- **Tools Used**: Nmap, Gobuster

---

##  Nmap Scan Summary

Command used:
```bash
nmap -sC -sV -oN nmap_scan.txt 192.168.6.3

Key Open Ports Identified:
Port	Service	Version	Notes
21	FTP	vsftpd 2.3.4	Known backdoor vulnerability (CVE-2011-2523)
22	SSH	OpenSSH 4.7p1	Possible weak password brute-force vector
23	Telnet	Linux telnetd	Plaintext protocol, likely exploitable
25	SMTP	Postfix smtpd	May be fingerprinted for internal relaying
80	HTTP	Apache 2.2.8	Hosts various web apps; vulnerable to many old CVEs
139/445	SMB	Samba 3.0.20	Possible for null session/usermap exploits
3306	MySQL	MySQL 5.0.51a	Older version; check for weak creds
8180	HTTP	Tomcat/Coyote JSP Engine	Potential for default credential login + WAR upload
1524	Backdoor Shell	netcat-like bind shell	Exploitable backdoor, port already open
Others	RPC, VNC, PostgreSQL, IRC	Many additional legacy services detected for future recon

Outline: The host is vulnerable to all sorts of attacks. There is minimal hardening. Services like FTP, HTTP and Tomcat present immediate redd team oportunities

Output saved to: nmap_scan.txt


#🌐 Web Enumeration with Gobuster

Command used:
```bash

gobuster dir -u http://192.168.6.3 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster_output.txt

| Path             | Status | Notes                                                                 |
| ---------------- | ------ | --------------------------------------------------------------------- |
| `/index`         | 200    | Homepage                                                              |
| `/test/`         | 301    | Likely test app — follow up manually                                  |
| `/twiki/`        | 301    | Old wiki software — may contain RCE or LFI                            |
| `/tikiwiki/`     | 301    | Another CMS/wiki — check for known vulns                              |
| `/phpinfo`       | 200    | Full PHP environment leak — great for LFI/command injection targeting |
| `/phpMyAdmin/`   | 301    | High-value target — attempt default logins                            |
| `/server-status` | 403    | Apache mod\_status exists — blocked, but present                      |

Outline: The site exposes multiple outdated applications and configurations. /phpMyAdmin/ and /phpinfo are high-priority targets. Wikis often have plugin RCE issues.

Output saved to: gobuster_output.txt