## What are file upload vulnerabilities

- File upload vulnerabilities are when a web server allows users to upload files to its filesystem without sufficient validation with things like name,type,content, or size
--> Basic image upload could be used as uploading server-side scripts that enables remote code execution

## Impact of file upload vulnerabilities

- Depends on two Factors:
  - Which aspect of the file the websites fails to validate properly (size,type,content,...)
  - Which restricitons are imposed on the file once it has been successfully uploaded
  - Worst case: files type is not validate proper and server config allows certain types (e.g. `.php`) to be executed
  - If filename is not validated properly, an attacker could upload files with the same name and overwrite critical files
  - If size of file is not checked, the risk of DoS attack rises
 
  ## How do file upload vulnerabilities arise?

  - Blacklist filetypes -> can't be 100% sure every suspicious file type is blocked and maybe some that may still be dangerous are still allowed
  - Verify properties -> Can be manipulated via Burp Proxy or Repeater

  ## How do web servers handle requests for static files?

  - Server parses the requested path to identify the file extension
    -> Determine type of file (comparing it to a list of preconfigured mappings between extensions and MIME types)
    -> What happens next depends on file type and server config:
    - File type: Non-executable (e.g.(image, static HTML page) -> server may just send file's content to client in HTTP response
    - File type: Executable (e.g. PHP file) **and** server is configured to execute files of this type ->assign variables based on headers and parameters in the HTTP request before running the script 
    - File type: Non-executable and server **is not** configured to execute files of this type -> generally respond with an error
   
## Exploiting unrestricted file uploads to deploy a web shell

- Worst case: Server allows uploading server-side scripts, such as PHP, and is configured to execute them as code
PHP one-liner: `<?php echo file_get_contents('/home/carlos/secret'); ?>`
PHP more versatile web shell one-liner: `<?php echo system($_GET['command']); ?>`
--> Enables to pass random system command via query parameter like: `GET /example/exploit.php?command=id HTTP/1.1`

---

## Lab 1: Remote code execution via web shell upload
Goal: Upload web shell in PHP to get the contents of the file `/home/carlos/secret`

Step 1: Log in with given credentials
Step 2: Create own note with the PHP-line: `<?php echo file_get_contents('/home/carlos/secret'); ?>`
Step 3: Upload the file as Avatar
Step 4: Intercept Get request where script is getting executed -> Response contains the searched answer

---

## Exploiting flawed validation of file uploads

- Very unlikly to find a website without protection against file upload attacks
- Sometimes it is possible to exploit flaws to obtain web shell for remote code execution

**Flawed file type validation**

- Regular HTML forms send data as `POST` request with content type `application/x-www-form-urlencoded`
  - Fine for simple text (name, address,...) but not suitable for binary data (images, PDFs,...)
- For binary data → `multipart/form-data` is used instead
  - Message body gets split into separate parts for each form input
  - Each part has its own `Content-Disposition` header (input field info) and optionally a `Content-Type` header (MIME type)

## MIME Type Validation – and why it fails

- Some servers validate file uploads by checking the `Content-Type` header of each part against an expected MIME type
  - e.g. only allowing `image/jpeg` or `image/png`
- **Problem:** If the server only checks the `Content-Type` header without verifying the actual file contents → easily bypassed
  - Burp Repeater can be used to manually change the `Content-Type` header to an allowed MIME type while uploading a malicious file

---

## Lab 2: Web shell upload via Content-Type restriction bypass
Goal: Same as in Lab 1
Step 1: Log in
Step 2: Upload web shell as Avatar
Step 3: Capture traffic where web shell gets uploaded and send it to Intruder
Step 4: Change Content-Type of the web Shell
Step 5: Response contains answer -> Lab solved

---

## Preventing file execution in user-accessible directories

- First line of defense: prevent dangerous file types from being uploaded
- Second line of defense: stop the server from executing scripts that slip through
  - Servers are configured to only execute scripts with explicitly allowed MIME types
  - Other scripts get returned as plain text instead of executed → no web shell possible
  - Side effect: can leak source code if script contents are returned as plain text

**Bypass – Directory Exploitation**
- Upload restrictions and execution controls often differ between directories
- Upload directory → strict controls
- Other directories on the filesystem → assumed out of reach, less strict
- If attacker can upload a script to a different directory (not the standard upload folder) → server may execute it









































