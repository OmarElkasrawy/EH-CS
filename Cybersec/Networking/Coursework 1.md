# Network Topology V1


![](https://i.imgur.com/JlIGlz1.png)

# Network Topology V2
I tried to update my topology but it overlaps

![](https://i.imgur.com/4jlkRd3.png)

# Network Topology V3
Final topology

![](https://i.imgur.com/NxsmKqF.png)

![](https://i.imgur.com/NXGpeZa.png)

![](https://i.imgur.com/AdQj91e.png)





## Set up network


![](https://i.imgur.com/N3RwUzt.png)


### Testing Connectivity


![](https://i.imgur.com/drGblqd.png)

![](https://i.imgur.com/vG3f8LH.png)



# Security Concerns

Implement and secure the communication over the multiaccess network by admitting:

## Securing Administrative Access

- Set encrypted password for accessing privileged EXEC mode.
- Configure virtual terminal (vty) lines, which are used for remote management (e.g., through SSH or Telnet).
- Specify that local username and password authentication should be used for accessing the vty lines. (Users will need to enter a valid username and password configured on the device).
- Set a password for accessing the vty lines.
- Enter the configuration mode for the console line (the physical console port of the device).
- Set a password for accessing the console line.
- Then specify that a password is required to access the console line.

### Set Console Password
Restrict physical access to the router's console

![](https://i.imgur.com/Ki2Zjqg.png)

### Set Telnet/SSH Password

![](https://i.imgur.com/cHV7CB6.png)

### Enable Secret Password & Encrypt

![](https://i.imgur.com/4ssgBKf.png)


### Config Login Banner
Display banner when someone connects to the router

![](https://i.imgur.com/wjAogk0.png)

### Setup SSH Usage
Setup SSH only for remote usage

![](https://i.imgur.com/QAkVSm6.png)


#### Test SSH Connection

![](https://i.imgur.com/RtY6nsG.png)

### Console Login

![](https://i.imgur.com/dxy3INC.png)


Test OneDrive

## Implementing Switch Port Security

### Shutdown The Rest of The Interfaces


![](https://i.imgur.com/lfmAAdE.png)


![](https://i.imgur.com/lYWBp4p.png)


### Port Security

![](https://i.imgur.com/SkCnHLa.png)

## Securing OSPF Routing Protocol
### OSPF Routing Protocol
OSPF (Open Shortest Path First) enables a dynamic routing protocol that allows routers within an OSPF Autonomous System (AS) to share information about the networks they are connected to. OSPF is a link-state routing protocol, meaning that routers exchange detailed information about their links and use this information to build a comprehensive map of the network. 

#### Configuration

![](https://i.imgur.com/kx0yB45.png)

#### OSPF Informational Messages

![](https://i.imgur.com/jUx0Etn.png)

![](https://i.imgur.com/V5HmKcO.png)

![](https://i.imgur.com/iDT5KY6.png)

![](https://i.imgur.com/Srb0HO9.png)

final ospf working

![](https://i.imgur.com/KDcz1sz.png)

![](https://i.imgur.com/1ZSWO5U.png)

#### Securing failed


![](https://i.imgur.com/06kjYjP.png)

![](https://i.imgur.com/1TGNzIm.png)



## Additional Security Measurements Considering CIA

Best practice to use AAA (Authentication, Authorization, Accounting) framework to control access to network resources and managing user privileges.

![](https://i.imgur.com/vbTsSDs.png)

### Testing AAA


![](https://i.imgur.com/MSsm4zj.png)

Implementing version 2 ssh since version 1 is vulnerable

![](https://i.imgur.com/ggmtvvZ.png)



start R3 configs
fadel R7 R8