What is Authentication -> Read [Portswigger Authentication Writeup](https://github.com/Schniggelaus/Cybersecurity-Journey/blob/main/Writeups/Phase1/PortSwigger/Authentication.md)

Usefull picture:
<img width="1145" height="394" alt="image" src="https://github.com/user-attachments/assets/119a1be8-f02e-432c-9404-4007bc4e1be3" />

## 4 Classes of Authentication Bypass

1. Username Enumeration ([Portswigger Authentication Writeup](https://github.com/Schniggelaus/Cybersecurity-Journey/blob/main/Writeups/Phase1/PortSwigger/Authentication.md))
2. Credential Brute Force ([Portswigger Authentication Writeup](https://github.com/Schniggelaus/Cybersecurity-Journey/blob/main/Writeups/Phase1/PortSwigger/Authentication.md))
3. Logic Flaws - redirect authentication-related workflows by valid-looking input (most sources: Password resets, account recovery)
   - Relies on perfect valid input that drives the application through an unintended sequence of decisions
4. Cookie Manipulation - manipulates session state the server returns to the client after a successful login

Additional Info: **Credential stuffing** - Leaked password can lead to multiple security breaches across other services, since the user might use the same password for different services

New Approach to Brute Force:
- not with Burp Suite -> use ffuf command in Linux

|Command|Description|
|-------|-----------|
|`ffuf`| Executes ffuf command|
|`-w`| Selects wordlist (if more lists are involved -> .txt:W1,...txt:W2 -> mark in `-d`-Tag what list should be used for what attribute|
|`-X POST`| Sets HTTP method (in this case POST method)|
|`-d`| Defines request body|
|`FUZZ`| Is a token used as a placeholder so `ffuf` replaces `FUZZ` with every word from the wordlist|
|`-H`|Set Content-Type|
|`-u`|Sets target URL|
|`-mr`| Outputs responses whose body contains the given string|
|`-fc`| Discards every response that returns the given HTTP code|

## Logical Flaw

Case 1: Case-Sensitive Authorization Bypass
- If routing layer is case-insensitive and the authorization check uses strict string comparison
  - `/admin`-> triggers admin check
  - `/aDMin`-> bypasses admin check
  - Router maps `/aDMin`to `/admin`

Case 2: Parameter Pollution in Password Reset
- Password reset process consists in two steps:
  - Email verification (via query parameter)
  - Username submission (via POST body)
 
--> Application reads mail from the query string for identity validation and uses `$_REQUEST` to construct reset mail
    --> `$_REQUEST` merges multiple sources (prioritized POST-body parameter)-> attacker can inject second mail parameter in `POST` body

What happens:
Attacker receives password reset link for the victim account 


## Cookie Manipulation

Since HTTP is a stateless protocol, Cookies are used to remember a given user already has authenticated
When cookies are not cryptographically signed, the client can modify its contents and change decisions. -> An accepted cookie grants session access
3 different Cookie-Types:

**Plain Text Cookies**
- stored in the header, editable and visible for anyone holding the cookie
Example:
```
Set-Cookie: logged_in=true; Max-Age=3600; Path=/
Set-Cookie: admin=false; Max-Age=3600; Path=/
```
--> no signature or integrity check can lead so the client is free to modify the cooki, granting admin access easly

---

**Hashed Cookies**
- Assumption Hashed cookies are more safe since the same input always produces the same output, but the input cannot be recovered from the output
  - Missplaced assumption, since a hash is not a signature -> Any party knowing or guessing the original value can produce the same hash
  - There are public services holding databases of billions of pre-computed hashes and their source string

 ---
 
**Encoded Cookies**
- Reversible transformation applied to binary or structured data -> carryed through channel that accepts only restriced char set
- Encoding the cookie with base32 or base64
  -> no protection since Session cookie can easly be decoded, manipulated and encoded again so the cookie is valid again
