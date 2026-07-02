## What is IDOR
- `IDOR` (Insecure direct object references) allows a user to retrieve objects without checking whether the user is permitted to access it or not
- Is one OWASP A01: Broken Access Control

**Example**
- Application places an object ID directly in a URL parameter -> used to query a back-end database without any authorisation check
<img width="769" height="327" alt="image" src="https://github.com/user-attachments/assets/4d2cf532-0deb-4459-b40d-4603902c5b0f" />

- "1234" can simply be edited and user gets another result

## Finding IDORs in Encoded IDs
- Most developers encode their identifiers before including them in queries, cookies,...
- Most commonly used encoding schme is `base64`
  - Often ends with `=` or `==`
 
- Exploting encoded IDOR reference involves four steps:
  1. Decode value from the request
  2. Modify decoded output
  3. Re-encode the modified value
  4. Substitute the re-encoded string
 
## Finding IDORs in Hashed IDs
- Hashes converts same input to same output
- MD5 hash most common
-  Many services online with billions of hashes

## Finding IDORs in Unpredictable IDs
- `UUID`- random string which cannot be associated with MD5 or base64 is not 100% safe (i.e. `d3b07384-d9a0-4e9b-8b3c-2f1a6c7e4a90`)
- IDOR vulnerability only gets eliminated if server is not failing to verify the requesting user is authorised
- Standard approach is **two-account technique**
  1. Create 2 acc's
  2. Log into acc1 and record identifiers
  3. Log into acc2 and substitute acc1 identifiers into acc2
--> If server returns acc1 data to acc2, Server is vulnerable


---

**PORTSWIGGER** [IDOR-Course](https://portswigger.net/web-security/access-control/idor)

- No new informations

Lab1: Insecure direct object references

Goal: Chat with the Chatbot and get the password of the `user`: `carlos`

Step 1: Open chat and type anyhthing
Step 2: Download the transcript
Step 3: Noticing that the first downloaded transcript starts with the number "2" (Filename: `2.txt`) there could be a `1.txt`-file
Step 4: Interrupt the traffic with Burp Suite where HTTP requests the script -> send it to the repeater and change the document name 
Step 5: After successfully recieving the `1.txt`-file, the password is written down in the chat and log in data can be used to log in 

--> Lab solved
