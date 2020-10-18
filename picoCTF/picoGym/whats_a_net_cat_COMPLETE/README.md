# What's a net cat?
General Skills

## Description
> Using netcat (nc) is going to be pretty important. Can you connect to jupiter.challenges.picoctf.org at port 40752 to get the flag?

## Hints
1. nc [tutorial](https://linux.die.net/man/1/nc)

## Solution
You just have to use the `nc` command to connect to the given site. The flag will appear once you connect.
Here's a simple bash script to connect and send the flag to a txt file.
```bash
#!/bin/bash

nc jupiter.challenges.picoctf.org 40752 | grep "picoCTF" > flag.txt
```
We connect to the server using `nc`, use `grep` to just grab the flag and use `>` to send it to the file.

Flag: `picoCTF{nEtCat_Mast3ry_d0c64587}`