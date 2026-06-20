Ran an nmap scan to identify open ports and services running on the target.
 

`sudo nmap -O -sV -vv target_ip_address`

 
**Result:** Several ports were found open:
 
| Port | Service | Version |
|------|---------|---------|
| 135 | msrpc | Microsoft Windows RPC |
| 139 | netbios-ssn | Microsoft Windows netbios-ssn |
| 445 | microsoft-ds | SMB |
| 5985 | http | Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP) |
 
Port 445 (SMB) stood out as the most promising entry point, since SMB shares are a common attack surface on Windows hosts.
 
---
 
## Service Enumeration – SMB
 
`smbclient` is the tool used to interact with SMB shares from Linux.
 
First, I listed all available shares on the target:
 

`smbclient -L 10.10.10.10`
 
This returned a list of shares, including some default Windows admin shares (`ADMIN$`, `C$`, `IPC$`) and one custom share called `WorkShares`.
 
**Note:** A `$` at the end of a share name indicates a hidden share. Hidden shares aren't shown during normal browsing but can still be accessed directly if you know the name – they're "hidden," not protected.
 
I tried connecting to each share with an empty password:
 
```bash
smbclient \\\\10.10.10.10\\ADMIN$
smbclient \\\\10.10.10.10\\C$
smbclient \\\\10.10.10.10\\IPC$
```
 
The default admin shares (`ADMIN$`, `C$`, `IPC$`) returned an error when trying to list contents:
```
NT_STATUS_NO_SUCH_FILE listing \*
```
This means access itself worked (no login error), but there was nothing accessible/listable inside these shares for this context.
 
---
 
## Access – WorkShares
 
Connecting to the custom `WorkShares` share worked without issues:
 
`smbclient \\\\10.10.10.10\\WorkShares`
 
Logged in with no password required.
 
Inside, I found a directory named `James.P`, which contained the flag file.
 
---
 
## Finding the Flag
 
Downloaded the flag file using:
 
`get flag.txt`

Then read it locally:

`cat flag.txt`

 
---
 
## Lessons Learned
 
- SMB (port 445) is a classic Windows file-sharing protocol and a very common attack surface.
- Share names ending in `$` are hidden, not protected 
- Default Windows admin shares (`ADMIN$`, `C$`, `IPC$`) often exist on Windows hosts but typically require proper credentials/permissions to actually read content from, even if you can technically "connect."
- Always test empty/blank passwords and anonymous logins as a first step before assuming credentials are required – it's a common misconfiguration on intentionally vulnerable or poorly secured machines.
- <img width="1373" height="205" alt="image" src="https://github.com/user-attachments/assets/e8cc3965-5f44-4db7-ae71-8f037fe4469f" />

