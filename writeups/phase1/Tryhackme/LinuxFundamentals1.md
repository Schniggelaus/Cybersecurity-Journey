# Linux Fundamentals Part 1

- Linux is OS based on UNIX
- Linux is open-source -> there are a lot of different variants of Linux

## First Commands

**Commands:**
- `echo` : Outputs any text provided
- `whoami` : Outputs the currently logged in user
- `ls` : Listing directorys
- `cd` : Change directory
- `cat` : Concatenate (can be used to show content of a file)
- `pwd` : Print working directory (prints current filesystem)
- `find` : Finds a file 
  - If we know the name of the file : `find -name nameoffile.txt`
  - If we only know it has to be a `.txt`-file: `find -name *.txt`
    - `*` is a wildcard to search for anything that has `.txt` at the end
- `grep` : Search content of files for specific value
  - With `grep "testString" Testfile.txt` it searches for the value: `testString` in `Testfile.txt` and returns the value if found
  - With `grep -R "testString" /etc/` it searches for the value `testSTring` in all files inside the directory `/etc/`

**Shell Operators:**
- `&` : Allows to run commands in the background of the terminal
- `&&` : Allows to combine multiple commands together in one line of the terminal
- `>` : Redirector - Take the output from a command and direct it elsewhere
- `>>` : Does the same as `>` but appends the output rather than replacing (nothing gets overwritten)
 
