## Solution Lab "SQL injection vulnerability in WHERE clause allowing retrieval of hidden data"
1. Use a search filter so the URL changes to something like: `https://0a1100db04525af580f4dab600df001b.web-security-academy.net/filter?category=Accessories`
2. Add `'+OR+1=1--` to the URL 
3. Lab solved since another new articel is displayed. 

## Solution Lab: "SQL injection vulnerability allowing login bypass"
1. Navigate to `My account` to get to the Login page
2. To bypass the login with a correct PW i can enter `administrator'--` in the Box for Username
3. I have to use something for the box `password` otherwhise it won´t let me log in. Since i already bypassed the password check i can enter anything as password
4. Log in will be successful

## Solution Lab: SQL injection with filter bypass via XML encoding
1. Start Burp Suite and install extension `Hackvertor`
2. Start Burp webpage and paste URL given from Portswigger
3. Track the traffic when selecting a product and when checking a location
4. I can analyize that there is a `productId` used for different products and when checking the stock i see a XML command in the Request
   - Since in this Lab the WAF is blocking direct SQLIn i need to workaround with `XML encoding` and `Hackvertor`
   - Send the Request to the Repeater so i can manipulate the Request manually. 
   - In the Tag `storeId` i can add `UNION SELECT username || '~' || password FROM users`, mark it and navigate to: Extension->Hackvertor->Encode->hex_entities
   - Press send in the top left corner
   - As response i recieve 3 different users. Since i need the admin log in data to successfully bypass the Lab i use the admin logins to login.
  
