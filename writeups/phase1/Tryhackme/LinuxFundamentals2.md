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

## Permissions 101

- Each file has permission rights either a user/group can `Read`,`Write` and/or `Execute` it
  - Great for splitting up permissions in between users without big effort (e.g
- Each file has a set of permissions following the symbolic format: `rwxrwxrwx`
   - First 3 are applying to the Owner
   - Next 3 are applying to a Group
   - Last 3 are applying to the rest
 - Each letter represents a specific permission:
   -`r`= read
   -`w`= write
   -`x`= execute
- Each letter has a numeric value
  -`r`= 4
  -`w`= 2
  -`x`= 1

# Examples

  |Group|Permissions|Calculation|Value|
  |-----|-----------|-----------|-----|
  |Owner | rws | 4+2+1 | 7 |
  |Group | rws | 4+2+1 |7 |
  |Others | rws | 4+2+1 | 7 |

# Common Examples

|Symbolic | Numeric | Meaning |
| rwxr-xr-x | 755 | Owner can do anything, others can read and execute |
| rw-r--r-- | 644 | Owner can read and write, others can only read |
| rwx------ | 700 | Only the owner has access |

## Common Directories

- `/etc`
  - Most important root directories on each system
  - Stores `passwd`and `shadow` files, containing passwords for each user in encrypted format called `sha512`
  - Contains `sudoers` file, containing list of users & groups that have permission to run sudo command

-`/var`
  - Stores data that is frequently accessed or written on (e.g. logfiles)

- `/root`
  - Home for "root" system user

- `/tmp`
  - Unique directory found on Linux
  - Stores data that is only needed to be accessed once or twice
  - On PC restart, content of this folder are cleared
  - Any user can write on this folder by default






  
