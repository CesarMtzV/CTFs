# [Day 10] Networking - Don't be sElfish!

## Topics

- Samba

### 1. Using enum4linux, how many users are there on the Samba server?
```
3
```
For questions 1 and 2 we are going to use the same command, because it outputs the answer for both questions.
```bash
./enum4linux.pl -U -S 10.10.x.x
```
__Note:__ The tool is found in `/root/Desktop/Tools/Miscellaneous` in the AttackBox of THM.

__enum4linux ouput:__
```
 ============================== 
|    Users on 10.10.193.183    |
 ============================== 
index: 0x1 RID: 0x3e8 acb: 0x00000010 Account: elfmcskidy	Name: 	Desc: 
index: 0x2 RID: 0x3ea acb: 0x00000010 Account: elfmceager	Name: elfmceager	Desc: 
index: 0x3 RID: 0x3e9 acb: 0x00000010 Account: elfmcelferson	Name: 	Desc: 

user:[elfmcskidy] rid:[0x3e8]
user:[elfmceager] rid:[0x3ea]
user:[elfmcelferson] rid:[0x3e9]

 ========================================== 
|    Share Enumeration on 10.10.193.183    |
 ========================================== 
WARNING: The "syslog" option is deprecated

	Sharename       Type      Comment
	---------       ----      -------
	tbfc-hr         Disk      tbfc-hr
	tbfc-it         Disk      tbfc-it
	tbfc-santa      Disk      tbfc-santa
	IPC$            IPC       IPC Service (tbfc-smb server (Samba, Ubuntu))
```
__Note:__ There's more to the output, I just grabbed the important bits.

### 2. Now how many "shares" are there on the Samba server?
```
4
```

### 3. Use smbclient to try to login to the shares on the Samba server. What share doesn't require a password?
```
tbfc-santa
```
With the discovered list of shares, we have to try to login into one of them. Only one of them doesn't have a password.\
To try and connect to a share, use `smbclient //10.10.x.x/tbfc-xx`. The `xx` after `tbfc` should be replaced with the names of the sharenames.\
Upon trying and failing, we find that the directory with no password is `tbfc-santa`.

### 4. Log in to this share, what directory did ElfMcSkidy leave for Santa?
```
jingle-tunes
```
Once logged in, just do a `ls` to find the folder name and some interesting files.