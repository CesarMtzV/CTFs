# [Day 7] Networking - What's Under the CHristmas Tree?

## Topics

- Nmap

### 1. When was Snort created?
```
1998
```

### 2. Using Nmap on 10.10.137.17 , what are the port numbers of the three services running?  (Please provide your answer in ascending order/lowest -> highest, separated by a comma)
```
80,2222,3389
```
We just have to do a normal nmap scan to get the open ports.

__nmap command:__
```bash
nmap 10.10.X.X -sV
```

__nmap output:__
```
Starting Nmap 7.60 ( https://nmap.org ) at 2020-12-22 00:11 GMT
Nmap scan report for ip-10-10-137-17.eu-west-1.compute.internal (10.10.137.17)
Host is up (0.0014s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Apache httpd 2.4.29 ((Ubuntu))
2222/tcp open  ssh           OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
3389/tcp open  ms-wbt-server xrdp
MAC Address: 02:B0:33:6C:7C:7B (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.98 seconds
```

### 3. Use Nmap to determine the name of the Linux distribution that is running, what is reported as the most likely distribution to be running?
```
ubuntu
```

### 4. Use Nmap's Network Scripting Engine (NSE) to retrieve the "HTTP-TITLE" of the webserver. Based on the value returned, what do we think this website might be used for?
```
blog
```

__nmap command:__
```
nmap --script http-title -p 10.10.x.x
```

__nmap output:__
```
Starting Nmap 7.60 ( https://nmap.org ) at 2020-12-22 00:16 GMT
Nmap scan report for ip-10-10-137-17.eu-west-1.compute.internal (10.10.137.17)
Host is up (0.00023s latency).

PORT   STATE SERVICE
80/tcp open  http
|_http-title: TBFC&#39;s Internal Blog
MAC Address: 02:B0:33:6C:7C:7B (Unknown)
```
