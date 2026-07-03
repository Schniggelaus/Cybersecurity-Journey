## What is path traversal?

- Also known as directory traversal
- Enables attacker to read random files on the server - including such as application code and data, credentials, sensitive OS files
- Sometimes attacker can write random files on the server, allowing to modify application data or behavior

## Reading arbitrary files via path traversal

**Example:**
```
<img src="/loadImage?filename=218.png">
```
LoadImage URL takes a filename parameter and returns content of specified file. In this case it is the file 218.jpg. The image files are stored on disk in the location `/var/www/images/`
So to return the file the whole path will transform to:
```
/var/www/images/218.png
```
This can be easily manipulated by changing the URL to
```
https://insecure-website.com/loadImage?filename=../../../etc/passwd
```
which will let the application read from following path:
```
/var/www/images/../../../etc/passwd
```
- `../` goes up one directory
--> with the `/../../../` the attacker is directing to the root system and so the file that gets actually read is `/etc/passwd`

---

## Lab 1: File path traversal, simple case

Solved by just using the example above

---

## Common obstacles to exploiting path traversal vulnerabilities

- Many applications have defenses against path traversal attack but they often can be bypassed with following techniques:
  - Maybe it is possible to use an absolute path from root system, to directly reference a file: `filename=/etc/passwd`
  - Use traversal sequences, such as `....//` or `....\/` -> bypass filters that strip simple ../ sequences
  - Sometimes the file needs proper file extension (e.g.: `.txt`) -> use null byte to terminate file path: `filename?../../../etc/passwd%00.png`
    - works since %00 gets terminated in many languages and .png will get ignored from the filesystem and the app audits .png as correct ending
  - If application expects some base folder -> use base folder followed by suitable traversal sequences (look at Lab 1)


Every Lab here is basically just using one of the exploits from above

## How to prevent path traversal attacks

- Avoid passing user-supplied input to filesystem API's altogether.
- If it is not avoidable then 2 layers of defense is recommended
  - Layer 1
     - Validates user input before processing
     - Compare user input with a whitelist of permitted values
     - If not possible, verify input contains only permitted content
  - Layer 2
     - After avlidating input, append input to base directory and use a platform filesystem API to canonicalize the path
     - **Example**
       ```
        File file = new File(BASE_DIRECTORY, userInput);
        if (file.getCanonicalPath().startsWith(BASE_DIRECTORY)) {
       // process file
        }
       ```



























