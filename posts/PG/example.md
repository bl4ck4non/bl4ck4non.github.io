First enum with nmap

```
# Nmap 7.93 scan initiated Mon Feb 27 18:15:06 2023 as: nmap -sC -sV -oN normal.tcp -p 21,80 -Pn 192.168.0.103
Nmap scan report for 192.168.0.103
Host is up (0.00089s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 1127     1127            0 Jan 27 22:45 first
| -rw-r--r--    1 1039     1039            0 Jan 27 22:45 second
| -rw-r--r--    1 0        0          290187 Feb 11 16:35 secret.jpg
|_-rw-r--r--    1 1081     1081            0 Jan 27 22:45 third
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.0.101
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp open  http    Apache httpd 2.4.54 ((Debian))
|_http-server-header: Apache/2.4.54 (Debian)
|_http-title: Document
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Feb 27 18:15:15 2023 -- 1 IP address (1 host up) scanned in 9.52 seconds
```

![image](https://user-images.githubusercontent.com/126628077/222015610-9d8a5124-5a9c-47d4-8f40-b050a7d2c01f.png)


