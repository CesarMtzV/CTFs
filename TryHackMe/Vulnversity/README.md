# Vulnversity

## Task 1 - Deploy the machine

## Task 2 - Reconnaissance

### 1. Scan the box, how many ports are open?
```
6
```

### 2. What version of the squid proxy is running on the machine?
```
3.5.12
```
For the first two questions we do a regular nmap scan.
__nmap ouput:__
```
kali@kali:~$ nmap -sC -sV -T4 -A 10.10.84.242
Starting Nmap 7.80 ( https://nmap.org ) at 2020-12-18 17:07 EST
Stats: 0:00:05 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 21.05% done; ETC: 17:08 (0:00:15 remaining)
Stats: 0:01:03 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 99.75% done; ETC: 17:08 (0:00:00 remaining)
Nmap scan report for 10.10.84.242
Host is up (0.20s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 5a:4f:fc:b8:c8:76:1c:b5:85:1c:ac:b2:86:41:1c:5a (RSA)
|_  256 ac:9d:ec:44:61:0c:28:85:00:88:e9:68:e9:d0:cb:3d (ECDSA)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
3128/tcp open  http-proxy  Squid http proxy 3.5.12
|_http-title: ERROR: The requested URL could not be retrieved
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Vuln University
Service Info: Host: VULNUNIVERSITY; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h40m00s, deviation: 2h53m12s, median: 0s
|_nbstat: NetBIOS name: VULNUNIVERSITY, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: vulnuniversity
|   NetBIOS computer name: VULNUNIVERSITY\x00
|   Domain name: \x00
|   FQDN: vulnuniversity
|_  System time: 2020-12-18T17:08:22-05:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2020-12-18T22:08:22
|_  start_date: N/A
```

### 3. How many ports will nmap scan if the flag -p-4-- was used?
```
400
```

### 4. Using the nmap flag -n what will it not resolve?
```
dns
```

### 5. What is the most likely operating system this machine is running?
```
ubuntu
```

### 6. What port is the web server running on?
```
3333
```

## Task 3 - Locating directories using gobuster

### 1. What is the directory thas has an upload form page?
```
/internal/
```
We just have to run a normal gobuster scan to disocver the directories:

__gobuster output:__
```
kali@kali:~$ gobuster dir -u http://10.10.84.242:3333/ -w /usr/share/wordlists/dirb/common.txt 
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.84.242:3333/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2020/12/18 17:19:38 Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 293]
/.htaccess            (Status: 403) [Size: 298]
/.htpasswd            (Status: 403) [Size: 298]
/css                  (Status: 301) [Size: 317] [--> http://10.10.84.242:3333/css/]
/fonts                (Status: 301) [Size: 319] [--> http://10.10.84.242:3333/fonts/]
/images               (Status: 301) [Size: 320] [--> http://10.10.84.242:3333/images/]
/index.html           (Status: 200) [Size: 33014]                                     
/internal             (Status: 301) [Size: 322] [--> http://10.10.84.242:3333/internal/]
/js                   (Status: 301) [Size: 316] [--> http://10.10.84.242:3333/js/]
```

## Task 4 - Compromise the webserver

### 1. Try upload a few file types to the server, what common extension seems to be blocked? 
```
.php
```

### 2. Run this attack, what extension is allowed?
```
.phtml
```

### 3. What is the name of the user who manages the webserver?
```
bill
```

### 4. What is the user flag?

1. Run `cat /etc/passwd` to see all available user
2. We can identify the user `bill` as well as his directory
3. Change into his directory with `cd /home/bill`
4. Read the flag with `cat user.txt`