Authentication is the process of verifying the identity of a user or client. There are 3 main types of authentification:
- `Knowledge factors` - Something the user **knows**, such as a password
- `Possession factors` - Something the user **has**, such as a physical object or a security token
- `Inherence factors` - Something the user **does** - biometrics or patterns of behavior

## Difference between Authentication and Authorization

- `Authentication`: Process of verifying that a user is who tey claim to be
- `Authorization`: Process verifying wether a user is allowed to do something

## Impact of vlunerable authentication

- If attacker bypasses authentication or brute-force their way into another user's account, they have access to all the data and functionality that the compromised account has
- If attacker compromises a high-privileged account, such as an admin, they could take control over entire application
- Even low-privilieged accounts can still give the attacker the chance to further attack additional pages

## Brut-force attacks

- Attacker guesses valid user credentials
- Usually automated by using wordlists
- Not only taking completly random guesses for usernames and passwords
  - Also using basic logic or publicy available knowledge, the attack can be fine-tuned
- Web-pages only using passowrd-based logins as their only method of authentication are highly vulnerable to brute-force attacks if there is not implementent sufficient brute-force protection

## Brut-forcing usernames

- Very easy to guess if they have recognizable pattern, such as email address
- Common business logins :`firstname.lastname@companyname.com`
- High-priviledged accounts often use usernames like `admin` or `administrator`

---

**During Auditing**
- Check if user profiles are accessable without login -> even if content is blocked, the name used in the profile is sometimes the same as the login username
- Check HTTP responses to see if any email addresses are disclosed

## Brut-forcing passwords

- Passwords can be brute-forced, depending on the strength of the password
- Mnay web-pages adopt with a more secure password policy. Usually this policy involves enforcing the passwords with:
  - A minimum number of chars
  - A mixture of lower and uppercase letters
  - At least one special char
 
- With basic knowledge of human behavior passwords seem to be safe but are not due to humans want to remember their passwor more easily
Example:
`mypassword` is not allowed because there are no uppercase letters and no special char
--> users may try something like `Mypassword1!` or `Myp4$$w0rd` instead

- In systems the system forces the user to change their password on a regular base, it is common to just make minor predictable changes
  - `Mypassword1!` --> `Mypassword1?` or `Mypassword2!`

  ## Username enumeration

- Validates whether a given username is valid or not
- Usually occurs on login page when entered a valid username but wrong password -> registration form tells you the entered username already is taken
- While attempting to brute-force a login page, pay attention to any differences in:
  - **Status codes** - If guess returns different status code -> indicates that the username is correct (not always)
  - **Error messages** - Sometimes error message is different depending whether both the username AND password are incorrect or only the password was incorrect
  - **Response times** - Similar response time indicates there is happening the same behind the scenes -> Username might be correct
 
  
## Lab1: Username enumeration via different responses

Goal: Using given wordlists, enumerate a valid username, brute-force this user's password and then access their account page

In this Lab `Brup Suite` is required

Step 1:
- Catch the traffix in the Proxy, when logging in intentionally with wrong credentials

Step 2:
- Send catched traffic to Intruder
- In the POST HTTP request, at the end there is located `username=test&password=testpassword`
- Add the payload at the username and paste the List given from [Portswigger](https://portswigger.net/web-security/authentication/auth-lab-usernames)
- Start the attack

Step 3:
- Sorting results by length, there is one username with another response
- In the HTTP Response the tag `Invalid username` had changed to `Invalid password` refering to that the username was correct

Step 4:
- Use the username and add a payload for the passwords-bracket given from [Portswigger](https://portswigger.net/web-security/authentication/auth-lab-passwords)
- Start the attack

Step 5:
- Identify the correct password by searching a differen Status code and Response length

Step 6: 
- Log in with username and password and the Lab is solved


## Lab2: Username enumeration via subtly different responses

Goal: Using given wordlists, enumerate a valid username, brute-force this user's password and then access their account page
This time there will be subtly different responses so analysis the length of the response will not lead to success

Step 1: 
- Set up a Grep-Extract Item
  - This helps of keeping track of responses and their warnings they give
  - Add an Extract Item and mark `Invalid username or password.`
  - The Item created will now display a new row in the attack scan `Warnings` and captures every warning from `-warning>` to </p> so it captures the excact warning-response
 
Step 2:
- Set up the attack like in Lab1 and start the attack

Step 3: 
- Sort the results for the warnings
- One username will have a slight different warning answer -> this is the correct username

Step 4:
-Set up password brute-force like in Lab1

Step 5:
- Filter for status codes and find correct password
- Login with credentials found and Lab is solved

## Lab3: Username enumeration via response timing

Goal: Using given wordlists, enumerate a valid username, brute-force this user's password and then access their account page
This time it is necessary to get the username due to higher response timing
This Lab also has extra protection against brute-force attacks with the Tag: `X-Frame-Options` -> Easly workaround with manipulating the HTTP Request by adding the `X-Forwarded-For`-Tag

Step 1:
- Same as Lab 1

Step 2: 
- Same as Lab 1
- Add the `X-Forwarded-For`-Tag and add another payload to that tag
  - Add numbers from 0-100 as payload

Step 3:

- Evaluate the results and sort the response time
  - One username will have extremly higher response time than the others -> Accepted username found

Step 4:

- Start another attack with password payloads and found username

Step 5: 

- Search for Responsecode `302` and login with found credentials
-->Lab solved
