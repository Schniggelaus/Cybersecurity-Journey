 
Started with a port scan, including OS detection and service version detection, to get as much information as possible right away.
 

`sudo nmap -O -sV -vv taget_ip_address`
 
- `-O` → enables OS detection
- `-sV` → detects service versions on open ports
- `-vv` → more detailed output 
- `sudo` is required for OS detection, as it relies on raw packet sending which needs elevated privileges
**Result:** Only one open port was found:
 
| Port | Service |
|------|---------|
| 21 | FTP |
 
---
 
## Service Enumeration & Access
 
With only FTP open, the next step was to check whether anonymous login was allowed.
 
`ftp 10.10.10.10`
 
Logged in using the username `anonymous` with no password required.
 
**Result:** Login succeeded – anonymous access was enabled on the FTP server.
 
---
 
## Finding the Flag
 
Once connected, I listed the available files on the FTP server with `ls` and found a file containing the flag. Downloaded it using:
 
`get flag.txt`
 
Then read the file locally:
 
`cat flag.txt`
 
---
 
## Lessons Learned
 
- FTP (port 21) is largely legacy at this point – in modern environments it's usually replaced by SFTP, which runs over SSH and encrypts the connection. Seeing plain FTP open is already a sign of an older or intentionally vulnerable setup.
- Anonymous FTP login is a classic misconfiguration check – always worth trying before assuming credentials are needed.
- `nmap -O` requires root/sudo privileges.
<img width="1419" height="209" alt="image" src="https://github.com/user-attachments/assets/593374bf-5219-42ec-857c-5b0c2a6fffe1" />
