
Recon shows 2 ports opened

![](https://i.imgur.com/J9mXwFc.png)


dir listing showed this

![](https://i.imgur.com/wxT7UUu.png)


after inspecting i wanted to go to uploads and panel. looks like panel is used for file upload and uploads is used to show the files that were successfully uploaded

![](https://i.imgur.com/y2nqGCT.png)


![](https://i.imgur.com/rkIA0Pf.png)


now we need to check which file type it takes

so according to burp it shows that it takes any file apart from .php
![](https://i.imgur.com/7l53MD1.png)

so then we can just use any extension really. i used phtml and ran a listener


![](https://i.imgur.com/XqotIJj.png)

 and i gained shell gg
as given hint by tryhackme it shows that the flag is in user.txt so i can just run find and get the txt file

![](https://i.imgur.com/JeGysSK.png)


and the flag is here

![](https://i.imgur.com/fCIg0la.png)


now we need to privesc by searching for files with SUID perm



we found something sus which is python SUID??? so i decided to go to GTFO bins and get it and found the command to use which successfully got me root access

![](https://i.imgur.com/CSETDqX.png)


![](https://i.imgur.com/7nn3vEo.png)
\

and heres the root flag

![](https://i.imgur.com/PSEzjhB.png)


