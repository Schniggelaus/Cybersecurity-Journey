# Linux Fundamentals 3

## Introducing terminal text editors

- Introduction to `nano`
  - Create new file: `nano filename`
  - Usefull for:
    - Searching text (`Ctrl+W`)
    - Copying and Pasting (`Ctrl+K` + `Ctrl+U`)
    - Jumping to line number (`Ctrl+_`)
    - Finding out what line number you are on

## General/Usful Utilities

### Downloading files
- Download files with `wget`
- Needs full address of the resource

### Transferring Files from your Host - SCP (SSH)
- `SCP` (Secure copy) allows to transfer files between two computers using the SSH protocol
- Can be used for both directions: current system -> remote system || remote system -> current system
- Example: `scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt`

### Serving Files From Your Host - WEB
- Ubuntu comes with `python3` -> Helpfull and easy-to-use module "HTTPServer"
  - Turns computer into a web server which can be used to serve files and others can download files with `curl` or `wget`
 
- Start Webserver with: `python3 -m http.server`

## Processes 101

- Processes are programs running on the machine
- Each Process has ID (`PID` - Process ID) incrementing with every process (60th process will have PID 60)
- `ps` command gives a list of running processes and additional infos such as status code, CPU usage, name the program is executed with
- `ps aux` to see processes from all users, including those not attached to a terminal session
- `top` command gives a real-time statistics about processes running

### Managing Processes

- Use `kill` command to kill a process (e.g. `kill 1010` will kill the process with PID : 1010)
  3 different kind of kills:

| Term | Description |
|------|-------------|
|SIGTERM | Kill the process, but allow it to do some cleanup|
|SIGKILL | Kill the process - doesn't cleanup |
|SIGSTOP | Stop/suspend a process (process can be continued later|

### Getting Processes/Services to Start on Boot

- Use of command: `systemctl` to start processes on Boot (e.g. servers,..)
- Format: `systemctl [option] [service]`
- Option can be:
  - Start
  - Stop
  - Enable
  - Disable
  - Status
 **Example:**
`systemctl enable apache2` -> Server `apache2` will start at boot

 ## Maintaining your system: Automation

 - `cron` command to schedule tasks after the system has booted
 - `crontabs` to interact with `cron`
 - Crontab requires 5 values + 1 command:
|Value|Description|
|-----|-----------|
|MIN | What minute to execute at|
|HOUR | What hour to execute at|
|DOM | What day of the month to execute at|
|MON | What month of the year to execute at|
|DOW| What day of the week to execute at|
|CMD| Command being executed|
**Example**
`0 */12 * * * cp -R /home/cmnatic/Documents /var/backups/`
- backups every Document every 12 hours

## Maintaining your System: Logs

- In `/var/log/` there are different services running
  - Servers(e.g. apache2, fail2ban (which monitored attempted brute force attacks), UFW (firewall)
  - Interesting logs are: `access log` and `error log`





