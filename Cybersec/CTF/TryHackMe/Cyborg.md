
# Recon
## Nmap
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 db:b2:70:f3:07:ac:32:00:3f:81:b8:d0:3a:89:f3:65 (RSA)
|   256 68:e6:85:2f:69:65:5b:e7:c6:31:2c:8e:41:67:d7:ba (ECDSA)
|_  256 56:2c:79:92:ca:23:c3:91:49:35:fa:dd:69:7c:ca:ab (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## Feroxbuster
```
──(kali㉿kali)-[~]
└─$ feroxbuster --url http://10.10.24.156 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.1
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.24.156
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.10.1
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET        9l       31w      274c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
403      GET        9l       28w      277c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET       15l       74w     6143c http://10.10.24.156/icons/ubuntu-logo.png
200      GET      375l      968w    11321c http://10.10.24.156/
301      GET        9l       28w      312c http://10.10.24.156/admin => http://10.10.24.156/admin/
301      GET        9l       28w      310c http://10.10.24.156/etc => http://10.10.24.156/etc/
[>-------------------] - 71s    21741/441101  22m     found:4       errors:2498   
🚨 Caught ctrl+c 🚨 saving scan state to ferox-http_10_10_24_156-1703197686.state ...
[>-------------------] - 71s    21803/441101  22m     found:4       errors:2530   
[#>------------------] - 71s    11054/220546  156/s   http://10.10.24.156/ 
[>-------------------] - 68s    10724/220546  158/s   http://10.10.24.156/admin/ 
[####################] - 0s    220546/220546  515294/s http://10.10.24.156/etc/ => Directory listing
[--------------------] - 0s         0/220546  -       http://10.10.24.156/icons/ubuntu-logo.png     
```

as you can see there are admin and etc hidden directories

![](https://i.imgur.com/YSJEXMD.png)


and etc

![](https://i.imgur.com/PB6egNW.png)


checking the archive button we can basically download an archive

![](https://i.imgur.com/KhX6zs0.png)


which downloaded these

![](https://i.imgur.com/H2dsPXL.png)


its time to explore each one of them

also the etc directory had these

![](https://i.imgur.com/di9i9HN.png)

![](https://i.imgur.com/KPzAnsv.png)


![](https://i.imgur.com/jFX3TPf.png)


looks like the passwd is a hash, by using hashcat i was able to crack the password and turns out it was `squidward`

![](https://i.imgur.com/PZNK9OP.png)


# Exploitation

so after that i went inside the README file which states that i gotta go to this link to see documentation

`https://borgbackup.readthedocs.io/en/stable/`

after that i checked a video and then followed what it says by listing the encrypted archive and extracting information

![](https://i.imgur.com/1iMyUiQ.png)


looks like thats the archive that we saw in the website, lets list it!

and we got some useful information

![](https://i.imgur.com/KP64Yup.png)


now what we can do is basically extract it!

![](https://i.imgur.com/iBy9cVF.png)

now we gotta explore...


# Foothold

looks like we found notes and secret text files, lets cat them

![](https://i.imgur.com/wdrZas3.png)

`alex:S3cretP@s3`

we found a user and password!! we can basically then ssh to the machine by using the credentials, since ssh is opened

now that we got credentials we managed to get in as user alex

![](https://i.imgur.com/BuuM91F.png)

`flag{1_hop3_y0u_ke3p_th3_arch1v3s_saf3}`

# Privesc

now its time to privesc, i tried to find the files that have SUID persm but nothing potential showed, so next  decided to actually check on the bash_history, the bash_history is actually owned by alex so why not just view what's inside??

![](https://i.imgur.com/rxoU545.png)

![](https://i.imgur.com/sjgQyZ2.png)


looks like the user ran both commands whoami and /bin/bash. so this means that the backup.sh script will be executed with /bin/bash and whoami as its command. and after checking the perms of this file it looks like alex is actually the owner of this file too!! 

![](https://i.imgur.com/9el1Okv.png)

i can simply do as follows so i can successfully run the command and get privilege escalation

```bash
chmod +w backup.sh
```

after running the command `sudo bash -c /bin/bash` i  successfully got root access!!

![](https://i.imgur.com/OoNo9Nk.png)


now its time to find the root text

![](https://i.imgur.com/xo6GQ7V.png)

and we successfully found the root flag gg
`flag{Than5s_f0r_play1ng_H0p£_y0u_enJ053d}`

## Method 2
another way to gain root access in a better way.

we can basically just remove everything inside the backup.sh  and add our own bash

![](https://i.imgur.com/vTrDPP7.png)


`cp /bin/bash /tmp/pwn;chmod +s /tmp/pwn`



# Conclusion

# 
---
[Borg](https://borgbackup.readthedocs.io/en/stable/)
