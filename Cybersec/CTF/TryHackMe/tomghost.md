
# Recon

![](https://i.imgur.com/P1EVlgf.png)

looks like its a normal default tomcat webpage 

![](https://i.imgur.com/d9YTBiq.png)


ah nice, tomcat. lets check if that version is vulnerable or sth

somehow version 9.0.30 has a CVE. 

This vulnerability in Apache Tomcat's AJP (Apache JServ Protocol), a binary protocol for web server to application server communication. The AJP protocol is mainly used in cluster or reverse proxy scenarios, but what is this all about?

CVE-2020-1938 involves file read/inclusion vulnerability in AJP connector, enabled by default on port 8009 which allows remote, unauthenticated attackers to read web application files and potentially achieve remote code execution (RCE), lol very interesting.

since AJP connector is treated with higher trust than HTTP, it leads to unexpected exploitable behaviors xd

# Exploitation

so lets dig deep in this CVE to find anything potential, in this case lets utilize docker on the tomcat image and run it. 

![](https://i.imgur.com/I1KEiwq.png)


since its now up we can get the script which is the CVE python script regarding [AJP 'Ghostcat File Read/Inclusion'](https://www.exploit-db.com/exploits/48143)

so lets run it, the syntax takes the ip of the machine and the default port, and lets specify the page which is the web.xml.

luckily i found credentials in this web.xml

![](https://i.imgur.com/aDag3cN.png)

`skyfuck:8730281lkjlkjdqlksalks`
# Foothold

so since ssh is opened on the machine, we can simply try these credentials to see if we can ssh

![](https://i.imgur.com/uUSBJTn.png)


successfully got initial access on this machine, now its time to find user.txt. we can simply run the find command to look for the user.txt anywhere on this machine.

![](https://i.imgur.com/ZEPeMbQ.png)

and we successfully got the user.txt
`THM{GhostCat_1s_so_cr4sy}`

# Privesc

now its time to escalate my privileges like a mad man and get the root.txt
before doing that i realized there are 2 home directories which means skyfuck and merlin are users, i think we gotta escalate first to merlin then get root  access after

hold on we found pgp and asc files in skyfuck's directory? lets have a look

PGP is pretty good privacy and it looks encrypted as shit

![](https://i.imgur.com/15JqND4.png)


luckily theres a .asc file with a php private key block so we can make use of that as well. 

now we need to create a hash using gpg2john


![](https://i.imgur.com/MoUzxqJ.png)

now we got a hash thats amazing, we can basically just use johntheripper to crack this bitch

![](https://i.imgur.com/PVaJZSs.png)


so after using john i successfully got the password now we need to decrypt the pgp file



![](https://i.imgur.com/o0u2gjI.png)


`tryhackme`


we first need to import the gpg key from the .asc file into my GPG keyring.

![](https://i.imgur.com/qs0ETQg.png)


after that, we can easily decrypt the credential file

![](https://i.imgur.com/3ml9xdm.png)

and there we go we got the credentials of merlin haha what a password

`merlin:asuyusdoiuqoilkda312j31k2j123j1g23g12k3g12kj3gk12jg3k12j3kj123j`

now lets switch user to merlin

![](https://i.imgur.com/VeuZ69h.png)

now we finally got into merlin, now its time to look for root access, lets check SUID executables.

nothing potential, lets check the command sudo -l to see what commands merlin can run as sudo

![](https://i.imgur.com/BpEa3D1.png)

wow looks like we can run /usr/bin/zip as root, lets check gtfo for that

![](https://i.imgur.com/sIIBAlW.png)


lets try that out 

we successfully got root access!!!

![](https://i.imgur.com/XnEiDxB.png)

lets look for the root.txt

![](https://i.imgur.com/gafYTVH.png)


`THM{Z1P_1S_FAKE}`

gg





---
[CVE-2020-1938: Ghostcat-Apache Tomcat AJP File Read/Inclusion Vulnerability](https://github.com/Hancheng-Lei/Hacking-Vulnerability-CVE-2020-1938-Ghostcat/blob/main/CVE-2020-1938.md)