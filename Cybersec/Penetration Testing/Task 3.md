## Enumeration

```nmap 172.17.0.0/16 -sn
Starting Nmap 7.80 ( https://nmap.org ) at 2023-11-15 11:55 UTC

[1]+  Stopped                 nmap 172.17.0.0/16 -sn
tkh@591196a9e44f:/$ nmap 172.17.0.0/16 -sn -vv
Nmap scan report for 172.17.0.0 [host down, received no-response]
Nmap scan report for 172.17.0.1
Host is up, received syn-ack (0.0017s latency).
Nmap scan report for 591196a9e44f (172.17.0.2)
Host is up, received conn-refused (0.00031s latency).
Nmap scan report for 172.17.0.3
Host is up, received conn-refused (0.00094s latency).

```


### Hostname Machine Access

![[Pasted image 20231115164205.png]]
## Open Ports
### 172.17.0.1

```
PORT      STATE SERVICE    REASON         VERSION
20/tcp    open  tcpwrapped syn-ack ttl 64
21/tcp    open  ftp        syn-ack ttl 64 vsftpd 3.0.5
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV IP 172.17.0.2 is not the same as 172.17.0.1
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:172.17.0.1
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.5 - secure, fast, stable
|_End of status
22/tcp    open  ssh        syn-ack ttl 64 OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
80/tcp    open  tcpwrapped syn-ack ttl 64
111/tcp   open  tcpwrapped syn-ack ttl 64
8081/tcp  open  http       syn-ack ttl 64 Apache httpd 2.4.52 ((Ubuntu))
| http-methods: 
|_  Supported Methods: POST OPTIONS HEAD GET
|_http-server-header: Apache/2.4.52 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
49161/tcp open  tcpwrapped syn-ack ttl 64
```

### 172.17.0.3

```
PORT STATE SERVICE REASON VERSION 
22/tcp open ssh syn-ack ttl 64 OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
```


## Establish Shell to Remote Server

![[Pasted image 20231115225839.png]]

### Create SSH Tunnel
##### Establishes a local port forwarding (tunneling) from your local machine to a remote server
```bash
ssh -L 8181:172.17.0.3:22 -fN kasrawy4@45.84.138.178 
```

![[Pasted image 20231115234044.png]]
![[Pasted image 20231115234100.png]]
![[Pasted image 20231115230412.png]]
```bash
ssh -L 8181:172.17.0.3:22 -fN kasrawy4@45.84.138.178 
ssh local port forwarding sourceport:destinationip:destinationport -fN run in background through kasrawy4
```

```
The `ssh -fN` command is used for setting up an SSH tunnel in the background without executing any commands on the remote server. Let's break down the individual options:

- `-f`: Requests the SSH command to go to the background once the connection is established. This is often used when you want to set up a tunnel without keeping the terminal window occupied.
    
- `-N`: Tells SSH that no commands will be sent once the connection is established. This is useful when you only want to establish a tunnel without executing any specific commands on the remote server.
```


https://explainshell.com/
```
tool where you write the command and it explains you in an easy way the meaning of each of the parameters
```

### Display Active Network Connections

```bash
netstat -tulpn
```

![[Pasted image 20231115230517.png]]

### Attempt to Establish SSH Connection to localhost 172.17.0.3

![[Pasted image 20231115230633.png]]

### Establish Tunneling from Local Machine to Remote Server

Create a tunnel from local machine on port 69 to 172.17.0.3 ssh service at port 22 via kasrawy4 on the remote server.

![[Pasted image 20231118013709.png]]

#### Check Active Connections

![[Pasted image 20231118015732.png]]

Check active connections on local port 69 set LISTEN

![[Pasted image 20231118020150.png]]

#### SSH Connection Successfully Established

![[Pasted image 20231118021603.png]]

#### Check SSH Session Environment

![[Pasted image 20231118023020.png]]

```
SSH_CONNECTION
- `172.17.0.1`: Client's IP address
- `60006`: Source port on the client
- `172.17.0.2`: Server's IP address (your SSH server)
- `22`: Destination port on the server (SSH default port)
```

```
SSH_CLIENT
- `172.17.0.1`: Client's IP address
- `60006`: Source port on the client
- `22`: Destination port on the server (SSH default port)
```

#### Successful Authentication Using Public Key for SSH Connection to 172.17.0.1

![[Pasted image 20231118024054.png]]

![[Pasted image 20231118205331.png]]


![[Pasted image 20231118205206.png]]

![[Pasted image 20231118205307.png]]

