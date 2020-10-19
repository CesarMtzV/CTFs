# Vault-door-1
Reverse Engineering

## Description 
> This vault uses some complicated arrays! I hope you can make sense of it, special agent. The source code for this vault is here: [VaultDoor1.java](https://jupiter.challenges.picoctf.org/static/ff2585f7afd21b81f69d2fbe37c081ae/VaultDoor1.java)

## Solution
By just looking at the code we can see the flag itself, it's just in a random order. Luckily the minion also provided us with the order, you just have to look at the `charAt()` function and build the flag one by one with the help of the index.

Flag: `picoCTF{d35cr4mbl3_tH3_cH4r4cT3r5_75092e}`