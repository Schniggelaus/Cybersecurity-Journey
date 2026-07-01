- `IDOR` (Insecure direct object references) allows an user retrieve objects without checking whether the user is permitted to access it or not
- Is one of the OWASP Top 10

**Example**
- Application places an object ID directly in a URL parameter -> used to query a back-end database without any authorisation check
<img width="769" height="327" alt="image" src="https://github.com/user-attachments/assets/4d2cf532-0deb-4459-b40d-4603902c5b0f" />
- "1234" can simply be edited and user gets another result

## Finding IDORs in Encoded IDs
- Most developers encode their identifiers before including them in querys, cookies,...
- Most commonly used encoding schme is `base64`
  - Often ends with `=` or `==`
 
- Exploting encoded IDOR reference involves four steps:
  1. Decode value from the request
  2. Modify decoded output
  3. Re-encode the modified value
  4. Substitute the re-encoded string
 
## Finding IDORs in Hashed IDs
- Hashs converts same input to same output
- MD5 hash mos common
-  Many services online with billions of hashes

## Finding IDORs in Unpredictable IDs
- `UUDI`- random string which cannot be assosiated with UD5 or base64 is not 100% safe (i.e. `d3b07384-d9a0-4e9b-8b3c-2f1a6c7e4a90`)
- IDOR vulnerability only gets eliminated if server is not failing to verify the requesting user is authorised
- Standard approach is **two-account technique**
  1. Create 2 acc's
  2. Log into acc1 and record identifieres
  3. Log into acc2 and substitute acc1 identifieres into acc2
--> If server returns acc1 data to acc2, Server is vulnerable
