<h2>Recon</h2>
  
  <h3>PortScan</h3>
  
  >command: sudo nmap -A -p- -T4 -v
```
# Nmap 7.93 scan initiated Sat Jan  7 10:06:55 2023 as: nmap -A -v -T4 -p- -oN twiggy 192.168.242.62
Nmap scan report for 192.168.242.62
Host is up (0.44s latency).
Not shown: 65529 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 447d1a569b68aef53bf6381773165d75 (RSA)
|   256 1c789d838152f4b01d8e3203cba61893 (ECDSA)
|_  256 08c912d97b9898c8b3997a19822ea3ea (ED25519)
53/tcp   open  domain  NLnet Labs NSD
80/tcp   open  http    nginx 1.16.1
|_http-favicon: Unknown favicon MD5: 11FB4799192313DD5474A343D9CC0A17
|_http-title: Home | Mezzanine
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS
|_http-server-header: nginx/1.16.1
4505/tcp open  zmtp    ZeroMQ ZMTP 2.0
4506/tcp open  zmtp    ZeroMQ ZMTP 2.0
8000/tcp open  http    nginx 1.16.1
|_http-open-proxy: Proxy might be redirecting requests
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Site doesn't have a title (application/json).
|_http-server-header: nginx/1.16.1
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Linux 3.X|4.X|5.X (89%)
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4.4 cpe:/o:linux:linux_kernel:5.1
Aggressive OS guesses: Linux 3.10 - 3.12 (89%), Linux 4.4 (89%), Linux 4.9 (89%), Linux 3.10 - 3.16 (86%), Linux 3.10 - 4.11 (85%), Linux 3.11 - 4.1 (85%), Linux 3.2 - 4.9 (85%), Linux 5.1 (85%)
No exact OS matches for host (test conditions non-ideal).
Uptime guess: 0.003 days (since Sat Jan  7 10:08:04 2023)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=264 (Good luck!)
IP ID Sequence Generation: All zeros

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   698.61 ms 192.168.49.1
2   698.68 ms 192.168.242.62

Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Jan  7 10:12:07 2023 -- 1 IP address (1 host up) scanned in 312.17 seconds
```

We have 6 ports opened here, port 22 which runs ssh, port 53 which runs domain, port 80,8000 which runs http, port 4505,4506 which runs zmtp. We won't waste our time on port 80 because I din't find anything interesting there. This means our enumeration will be focused more on port 8000.

<h2>Enumeration</h2>
  Going to the webpage should get you something like this

![image](https://user-images.githubusercontent.com/126628077/222027526-326df050-f1bc-47c9-8d0e-e84616a9ad5e.png)

Lets try checking the header using curl

>command: curl -v http://192.168.87.62:8000/

![image](https://user-images.githubusercontent.com/126628077/222029279-e15a72ea-5f99-463c-83f5-3d482a21fc86.png)

From the screenshot above we can see that it is running salt-api/3000-i, checking it up online I found this

![image](https://user-images.githubusercontent.com/126628077/222030012-7a1b8f63-701e-4ee3-950e-80e5b5c4c8b2.png)






























