### Linux Fundamentals Part 2 

## Accessing Your Linus Machine Using SSH 

- `SSH` ( Secure Shell) is used as a protrocol to have encrypted communication between 2 devices.
  - Used to do commands on another Linux machine remotly
 
- Command for using SSH is: `ssh tryhackme@10.1010.10`
- `tryhackme` is the username SSH will try to login to
- `@` seperates the username and the IP-Address of the machine SSH is trying to login onto
- `10.10.10.10` is the IP-Address of the machine SSH is trying to login onto
- after executing the command properbly, console wants you to login with the correct password. If done correctly your logged in onto the machine SSH with the IP-Address given

## Introduction to Flags and Switches

- Most commands (e.g. `ls`, `cd`, `grep`,...) arguments can be provided for further info
- Using `--help` will always show every argument a command accepts and a short description of what it does
- For more detailed view, use: `man`(e.g. `man ls`)

## Filesystem Interaction Continued

|Command| Description | Full Name|Example|
|-------|-------------|----------|-------|
|touch|Create file|touch|`touch note`|
|mkdir|Create a folder|make directory|`mkdir mydirectory`|
|cp|Copy a file or folder|copy|`cp note note 2`|
|mv|Move a file or folder|move|`mv note2 note3`|
|rm|Remove a file or folder|remove|`rm mydirectory`|
|file|Determime the type of a file|file|`file note`|

