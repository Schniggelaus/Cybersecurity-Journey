## What is XSS (Cross-site scripting)

- Web security vulnerability that allows an attacker to compromise interactions users have with vulnerable application
-  XSS allows the attacker to masquerade as a victim user, to carry out actions the user would be able to perform.
  - If the user also would be an admin user -> attacker could get full control

## How does it work

- Manipulating vulnerable web site so they return malicious JavaScript to users
  --> When the script executes inside a victims browser, attacker can fully compromise their interaction with the application

## Types of XSS attacks

- `Reflected XSS` - Script comes from current HTTP request
- `Stored XSS` - Script comes from the website's database
- `DOM-based XSS` - Vulnerability exists in client-side code rather than server-side code

**Reflected XSS**

- Arises when an application receives data in an HTTP request and includes that data within the immediate response in an unsafe way
*Example:*
```
https://insecure-website.com/status?message=All+is+well.
<p>Status: All is well.</p>
```
--> attacker can construct attack like:
```
https://insecure-website.com/status?message=<script>/*+Bad+stuff+here...+*/</script>
<p>Status: <script>/* Bad stuff here... */</script></p>
```


**Stored XSS**

- Arises when an application receives data from an untrusted source and includes that data within its later HTTP response in an unsafe way
- Malicious data can come from comments, user nicknames in a chat room, contact details, customer order,...
  - Example:
  - There is a comment section and the user is commenting `<script>alert("HACKED")</script>`
  - Script got uploaded to the DB and is therefore always getting called when web page gets loaded
  - --> Everytime a user loads the page, they'll get a notification which says "hacked"

**DOM-based XSS**

- Arises on applications containing client-side JavaScript that processes data from untrusted sources in unsafe way
  - Usually writes data back to the DOM
- Usually JS takes data from attacker-controllable source and passes it to a sink that supports dynamic code execution, such as `innerHTML` -> Attacker construct a link with a payload and send it to a victim
- Most common source is the URL which is accessed with the `window.location`
 - Example:

   ```
   var search = document.getElementById('search').value;
   var results = document.getElementById('results');
   results.innerHTML = 'You searched for: ' + search;
   ```
   
If an attacker can control the value of the input field, they can easily construct a malicious value that causes their own script to execute

```
You searched for: <img src=1 onerror='/* Bad stuff here... */'>
```

## Usecase of XSS

- Impersonate or masquerade as victim user
- Carry out any action that the user is able to perform
- Read any data the user is able to access
- Capture users login credentials
- Perform virtual defacement of the website
- Inject trojan functionality

## How to find and test for XSS vulnerabilities

- Use `Burp Suite's web vulnerability scanner`
- **Test every entry point** - Test every entry point for data within the applications HTTP requests including parameters within the URL query string, message body, URL file path, HTTP headers,...
- **Submit random alphanumeric values** - Use `Burp Intruder's number payloads` (randomly generated hex values) or `Burp Intruder's grep payloads settings` ( automatically flag responses that contain submitted value)
- **Determine the reflection context** - Each location within the response where the value is reflected, determine its context (between HTML tags? within a tag attribute?,...)
- **Test a candidate payload** - Based on context of reflection, test initial XSS payload that triggers JavaScript execution
- **Test alternative payloads** - If payload is blocked or modified, test alternative payloads and techniques
- **Test the attack in a browser** - If succeed in finding working payload -> transfer attack to real browser
- 

## How to prevent XSS attacks

- **Filter input on arrival** - filter for expected or valid input
- **Encode data on output**
- **Use appropriate response headers** - HTTP responses that are not intended to contain any HTML or JavaScrip, `Content-Type` and `X-Content-Type-Options` headers can be used
- **Content Security Policy** - Last line of defense
   

## Lab1 Reflected XSS into HTML context with nothing encoded

Step 1:
Find a exploitable HTTP request
In this case i can just use the search bar and search for anything
The HTTP request changes to the following:
```
https://[LAB-ID].web-security-academy.net/?search=test
```

Step2:
Perform the Reflected XSS attack with manipulating the HTTP request to:
```
(https://[LAB-ID].web-security-academy.net/?search=<script>alert("test")</script>
```
The script performs and will show the alert "test" --> Lab solved 

## Lab2: Stored XSS into HTML context with nothing encoded
Similar to Lab1
In this Lab a stored XSS attack is needed therefore searching out for Comment sections is the key

Step 1: 
- After clicking on one random Blog-post there is a comment section below the post
- Comment: `<script>alert("test")</script>` , fill in the missing gaps and post the Comment
- After reloading the page the payload will execute and the Lab is solved

## Lab3 DOM XSS in document.write sink using source location.search

- Goal ist to upload a JS payload to the DOM

Step 1:
- Analyze the source code and search for an alphanumeric object in the serach bar
- Realize how the HTML is changing

Step 2:
- In this case it seems that the alphanumeric object is getting parsed in an `img tag`
- This can be abused by intentionally closing the `img tag` and add the payload afterwards:
```
"><script>alert("payload uploaded")</script>
```
- Searching for this will solve the Lab.
- Successfully uploaded a working JS payload to the DOM 
  
