## What is privilege escalation?

- Involves going from lower permission account to a higher permission one
- Rare cases where attacker gains admin access directly -> Privilege escalation is needed to gain more and more levels of access to gain more controll

## Enumeration

First step once access was gained -> get more and more information about the system

### hostname
Command can expose purpose of the system (e.g. SQL-PROD-01 will be a production SQL server)

### uname -a
Command prints system informations giving additional details about the kernal used by the system -> useful when searching for any potential kernel vulnerabilitie that could lead to privilege escalation

### proc/version
Command gives information about target system processes -> Info about kernal version and additional data such as whether a compiler is installed

### /etc/issue
System can be identified by looking at `/etc/issue`-file -> It contains some infos about OS

### ps Command
Shows running processes 
Output shows following:
PID: Process ID
TTY: Terminal type
Time: Amount of CPU time used by the process
CMD: command or executable running

- `ps -A` = Shows all running processes
- `ps axjf` = Shows process tree
- `ps aux` = shows processes of all users

### env
Shows environmental variablesls

### sudo -l
Target system may be configured to allow users to run some commands with root privileges. This command lists all commands the user can run

### id 
Provides general overview of users privilege level and group memberships

### /etc/passwd
Can be easy way to discover users on the system
With `/etc/passwd | cut -d ":" -f 1` -> cut and convert to a useful list for brute-force attacks

### history
Looking at earlier commands -> can give idea about target system, sometimes containing infos such as passwords and usernames

### ifconfig
Target system may be a pivoting point to another network -> Command gives info about network interfaces 
- `ip route` shows network routes

### netstat
Gather informations about existing connections
- `netstat -a` = shows all listening ports and established connections
- `netstat -at` or `netstat -au` = list TCP/UDP protocols respectively
- `netstat -l` = list ports in "listening" mode - Ports are open and ready to accept incoming connections
- `netstat -lt` = Only lists ports that are listening using TCP protocol
- `netstat -s` = Lists network usage stats
- `netstat -i` = Shows interface stats
- `netstat -ano` = Display all sockets, does not resolve names, Display times

### find
Different command useCases:
- `find . -name flag.txt` = finds all files named "flag.txt" in current directory
- `find /home -name flag.txt` = finds all files named "flag.txt" in /home directory
- `find / -type d -name config` = finds directory named config under "/"
- `find / - type f -perm 0777` = finds all files with 777 permissions
- `find / -perm a=x` = finds executable files
- `find /home -user frank` = finds all files for user frank in /home
- `find / -mtime 10 ` = finds all files that were modified in the last 10 days
- `find / -atime 10` =  finds all files that were accessed in the last 10 days
- `find / -cmin -60` =  finds all files that were changed within the last hoour
- `find / -amin -60` = finds all files that were accessed  within the last hoour
- `find / -size 50M` = finds files with 50MB size (can be used with `+`or `-` to show files greater/shorter)
- `find / -writable -type d 2>/dev/null` = Find world-writeable folders
- `find / -perm -222 -type d 2>/dev/null` = Find world-writeable folders
- `find / -perm -o w -type d 2>/dev/null` = Find world-writeable folders

## Privilege Escalation: Karnel Exploits
Goal: Gain root privileges; sometimes archieved threw simply exploiting an exisiting vulnerability
Always go for the 3 Steps:
1) Identify the kernal version
2) Search and find exploit code for the kernel version of the target version
3) Run the exploit

IMPORTANT: runable exploit must be written in a language which is also installed on the target system

## Privilege Escalation: SUID

With the Command `find / -type f -perm -04000 -ls 2>/dev/null` console is showing files that have SUID(Set-user Identification) or SGID(Set-Group Identification) bits
-> With help of [GTFOBins](https://gtfobins.org/) it is again possible to search for exploitable missconfigurations. In this case it is base64 which allows the user to read files without having root privilege

## Privilege Escalation: Capabilities

- System admins can increase privilege level of a process or binary is "Capabilities"
- `getcap -r /` lists enabled capabilities
- With help of [GTFOBins](https://gtfobins.org/) it is again possible to search for exploitable missconfigurations.

## Privilege Escalation: Cron Jobs

Cron jobs are used to run scripts or binaries at specific times. By default they run with the privilege of their owners and not the current user
-> Can provide privilege escalation vector
Idea: Task that runs with root privileges and user can change the script ( for example reverse shell to gain root access)
Sometimes useless scripts are not getting cleaned up and just deleted 

## Privilege Escalation: PATH


