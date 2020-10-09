# Nmap Quiz

## Task 1 - Help menu
```
-h
```

##Task 2 - steatlh scan/Syn Scan
```
-sS
```

##Task 3 - UDP scan
```
-sU
```

##Task 4 - Operatin system detection
```
-O
```

##Task 5 - Version detection
```
-sV
```

##Task 6 - Verbosity flag
```
-v
```

##Task 7 - Very verbose flag
```
-vV
```

##Task 8 - Save output in XML format
```
-oX
```

##Task 9 - Aggressive scan
```
-A
```

##Task 10 - Set timing to the max level (Insane)
```
-T5
```

##Task 11 - Scan specific port
```
-p
```

##Task 12 - Scan every port
```
-p-
```

##Task 13 - Enable using a script form the nmap scripting engine
```
--script
```

##Task 14 - Run all scripts out of the vulnerability category
```
--script -vuln
```

##Task Switch to not ping the host
```
-Pn
```

# Nmap scannig
```
IP = 10.10.220.200
```
## Task 1 - syn scan
```
nmap -sS

Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-29 19:42 CDT
Nmap scan report for 10.10.220.200
Host is up (0.21s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 18.49 seconds
```

## Task 2 - how many ports do we find open under 1000?
```
2
```

## Task 3 - Comunication protocol for the ports
```
tcp
```

## Task 4 - Perform a service version detection scan, what is the version of the software running on port 22?
```
Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-29 19:45 CDT
Nmap scan report for 10.10.220.200
Host is up (0.23s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.10 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.76 seconds


6.6.1p1
```

## Task 5 - Perform an aggressive scan, what flag isn't set under the results for port 80
```
$sudo nmap -A 

10.10.220.200                            
Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-29 19:45 CDT
Nmap scan report for 10.10.220.200
Host is up (0.21s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   1024 da:52:f8:da:91:85:ac:76:bb:c0:b0:73:00:3a:52:f7 (DSA)
|   2048 23:bd:f8:fb:9d:85:e8:63:3b:f8:a7:7a:49:a5:f4:50 (RSA)
|   256 04:c0:58:62:39:0f:e1:c8:c5:e3:db:f5:8a:02:7a:b9 (ECDSA)
|_  256 07:d9:31:6c:64:96:6b:6a:c7:d2:a1:a9:42:a0:36:4d (ED25519)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
| http-cookie-flags:
|   /:
|     PHPSESSID:
|_      httponly flag not set
| http-robots.txt: 1 disallowed entry
|_/
|_http-server-header: Apache/2.4.7 (Ubuntu)
| http-title: Login :: Damn Vulnerable Web Application (DVWA) v1.10 *Develop...
|_Requested resource was login.php
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=9/29%OT=22%CT=1%CU=41430%PV=Y%DS=4%DC=T%G=Y%TM=5F73D56
OS:2%P=x86_64-apple-darwin19.5.0)SEQ(SP=106%GCD=1%ISR=108%TI=Z%CI=I%II=I%TS
OS:=8)OPS(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M
OS:508ST11NW6%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68
OS:DF)ECN(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=
OS:S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q
OS:=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A
OS:%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y
OS:%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T
OS:=40%CD=S)

Network Distance: 4 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 5900/tcp)
HOP RTT       ADDRESS
1   73.55 ms  10.2.0.1
2   ... 3
4   215.72 ms 10.10.220.200

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 39.74 seconds


httponly
```

## Task 6 - Perform a script scan of vulnerabilities associated with this box, what denial of service (DOS) attack is this box susceptible to?
```
$ nmap --script vuln 10.10.220.200
Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-29 21:22 EDT
Nmap scan report for 10.10.220.200
Host is up (0.22s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
80/tcp open  http
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|       httponly flag not set
|   /login.php: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-enum: 
|   /login.php: Possible admin folder
|   /robots.txt: Robots file
|   /config/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)'
|   /docs/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)'
|_  /external/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)'
| http-slowloris-check: 
|   VULNERABLE:
|   Slowloris DOS attack
|     State: LIKELY VULNERABLE
|     IDs:  CVE:CVE-2007-6750
|       Slowloris tries to keep many connections to the target web server open and hold
|       them open as long as possible.  It accomplishes this by opening connections to
|       the target web server and sending a partial request. By doing so, it starves
|       the http server's resources causing Denial Of Service.
|       
|     Disclosure date: 2009-09-17
|     References:
|       http://ha.ckers.org/slowloris/
|_      https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2007-6750
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.

Nmap done: 1 IP address (1 host up) scanned in 332.59 seconds


http-slowloris-check
```















