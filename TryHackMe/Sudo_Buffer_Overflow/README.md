# Sudo Buffer Overflow

## Task 1 - Deploy

Just deploiy it. Press the big green button.

## Task 2 - Buffer Overflow

The exploit has been already uploaded into the machine and compiled. All we have to do is login into the machine.
```bash
ssh tryhackme@10.10.74.22 -p 4444
```
Once logged in, we can find the already compiled exploit. We just have to run it.
```bash
./exploit
```
The program is going to run and in a couple of seconds we should become root. 

### 1. What's the flag in /root/root.txt?
```
THM{buff3r_0v3rfl0w_rul3s}
```